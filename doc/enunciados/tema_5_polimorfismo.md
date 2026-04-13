<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta

El polimorfismo es la capacidad que tienen los objetos de diferentes clases de responder a un mismo mensaje (llamada a un método) de maneras distintas. En programación orientada a objetos, esto permite tratar a objetos de diversas subclases como si fueran de una superclase común, delegando en cada objeto la responsabilidad de ejecutar la acción que le corresponde según su tipo real. Es una herramienta clave para reducir el acoplamiento y aumentar la flexibilidad del sistema.

La sobreescritura (overriding) es la técnica que hace posible el polimorfismo. Consiste en redefinir en una subclase un método que ya existe en la superclase, manteniendo la misma firma (nombre y parámetros). Al sobreescribir, se proporciona una implementación específica que oculta o sustituye a la implementación original cuando el método es invocado sobre una instancia del subtipo.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta

La ligadura dinámica, también llamada enlace tardío (late binding), es el mecanismo por el cual la asociación entre una llamada a un método y el código que realmente se ejecuta se decide en tiempo de ejecución, y no en tiempo de compilación. Cuando el compilador encuentra una llamada a un método sobre una referencia de una superclase, no sabe qué tipo de objeto real habrá ahí; es la Máquina Virtual la que consulta el tipo del objeto en memoria y salta a la implementación correspondiente.

La necesidad de indicar esto explícitamente varía según el lenguaje. En Java y Python, el polimorfismo y la ligadura dinámica son el comportamiento por defecto; todos los métodos (salvo los estáticos, finales o privados en Java) son virtuales. En cambio, en C++, se debe indicar explícitamente mediante la palabra clave virtual en la clase base para que el compilador habilite la tabla de métodos virtuales (vtable) y permita el enlace tardío.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta

En este diseño, la clase Soldado define un comportamiento general que es posteriormente especializado por sus descendientes. Mientras que Artillero mantiene el saludo estándar, Zapador reemplaza completamente la lógica original. Lo relevante es que, desde el punto de vista del programa principal, solo se manejan "Soldados", ignorando las particularidades de cada uno hasta que llega el momento de actuar.

Al recorrer el array con una referencia de tipo Soldado, se observa el polimorfismo en acción: aunque el código siempre invoca s.saludar(), la salida por consola variará dependiendo de si s apunta a un objeto Artillero o Zapador. La estructura del bucle es limpia y no necesita saber cuántos tipos de soldados existen.

Java
public class Soldado {
    public void saludar() { System.out.println("Soldado presentándose."); }
}

public class Zapador extends Soldado {
    @Override
    public void saludar() { System.out.println("¡Zapador despejando el camino!"); }
}

public class Artillero extends Soldado {
    // Hereda el saludo estándar de Soldado
}

// Implementación del polimorfismo
Soldado[] unidades = { new Artillero(), new Zapador() };
for (Soldado s : unidades) {
    s.saludar(); // Ejecuta el método según el objeto real
}

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta

Es posible y muy frecuente invocar el método de la clase base desde una sobreescritura para aprovechar la lógica ya existente y simplemente añadirle algo más. Esto evita la duplicación de código y permite que la subclase actúe como una "extensión" funcional en lugar de un reemplazo total. Es especialmente útil cuando la superclase realiza tareas complejas de inicialización o validación que la subclase debe respetar.

Para lograr esto en Java, se utiliza la palabra clave super. Al escribir super.saludar(), se le indica a la Máquina Virtual que busque y ejecute la versión del método definida en la jerarquía superior inmediata, permitiendo que el flujo del programa regrese después al método de la subclase para continuar con sus instrucciones específicas.

Java
public class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar(); // Llama al saludo normal del Soldado
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta

Para que exista sobreescritura, la firma del método en la subclase debe ser idéntica a la de la superclase: mismo nombre y mismos parámetros en orden y tipo. El tipo de retorno debe ser el mismo o un subtipo (retorno covariante). Es vital diferenciarlo de la sobrecarga (overloading), que ocurre cuando en una misma clase hay métodos con el mismo nombre pero distintos parámetros; la sobrecarga es un polimorfismo en tiempo de compilación, no de ejecución.

La anotación @Override es una instrucción para el compilador. Sirve para validar que realmente se está sobreescribiendo un método de la superclase. Si se comete un error tipográfico en el nombre del método o en los parámetros, el compilador lanzará un error. Es una práctica recomendada de seguridad para evitar crear accidentalmente un método nuevo (sobrecarga) cuando la intención era redefinir uno existente.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta

Efectivamente, el polimorfismo es omnipresente en Java desde el primer día. Debido a que todas las clases heredan de la clase raíz Object, cualquier clase que definamos ya posee métodos como toString(), equals() o hashCode(). Cuando un programador redefine toString() para que un objeto muestre sus atributos de forma legible, está realizando una sobreescritura polimórfica.

Un ejemplo claro es cuando se imprime un objeto con System.out.println(objeto). Internamente, este método llama a objeto.toString(). Gracias al polimorfismo, el sistema operativo de Java no necesita saber qué tipo de objeto estás imprimiendo; simplemente confía en que el objeto tiene una implementación de toString() (ya sea la de Object por defecto o una personalizada) y la ejecuta dinámicamente.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta

Una clase abstracta es una clase que no está completa y, por lo tanto, no se pueden crear instancias de ella (no se puede hacer new Soldado()). Su propósito es servir de molde o base para otras subclases. Un método abstracto es una declaración de un método que no tiene cuerpo (no tiene llaves {}); es una promesa de que todas las subclases no abstractas implementarán dicho método obligatoriamente.

Se debe colocar la palabra clave abstract tanto en la definición de la clase como en cada uno de los métodos que carezcan de implementación. Esto obliga a los desarrolladores de las subclases a decidir cómo se realiza esa acción concreta, garantizando que todos los objetos de la jerarquía compartan la misma interfaz pero con comportamientos específicos.

Java
public abstract class Soldado {
    public void saludar() { System.out.println("Presentándose."); }
    
    // Método sin cuerpo: obliga a los hijos a implementarlo
    public abstract void atacar(); 
}

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta

La palabra clave final actúa como una restricción de seguridad que corta la cadena de herencia o sobreescritura. Si una clase se marca como final, no puede tener subclases. Si un método se marca como final, no puede ser sobreescrito por ninguna subclase. Esto es útil para proteger algoritmos críticos o garantizar que el comportamiento de una clase sea inmutable y predecible.

En relación al polimorfismo, final impide que un método participe en la ligadura dinámica desde un punto determinado de la jerarquía. Un ejemplo clásico en la API de Java es la clase String. La clase String es final por razones de seguridad y optimización; nadie puede crear una subclase de String y cambiar cómo funcionan los métodos de manejo de texto básicos del lenguaje.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta

Las interfaces en Java son contratos que definen un conjunto de métodos que una clase debe implementar. A diferencia de las clases abstractas, las interfaces tradicionalmente no contenían estado (atributos) ni implementación de métodos (aunque esto ha cambiado ligeramente en versiones recientes con los métodos default). Mientras que una clase solo puede heredar de una superclase, puede implementar múltiples interfaces.

Esta capacidad permite que una clase juegue varios roles en un sistema. Por ejemplo, una clase Robot podría heredar de Maquina (herencia simple) e implementar las interfaces Caminante, Nadador y ConectorUSB. Las interfaces son la herramienta principal de Java para lograr algo similar a la herencia múltiple de C++, pero de una forma mucho más controlada y sin los problemas de colisión de código.

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta

En este escenario, la clase Linea demuestra la potencia del polimorfismo al interactuar con objetos de tipo Punto sin importarle si son de dos o tres dimensiones. El método obtenerLongitud simplemente llama a calcularDistanciaA, y la ligadura dinámica se encarga de saltar a la versión 2D o 3D según corresponda. La encapsulación se mantiene, pues la línea no conoce la lógica interna de los puntos.

Dentro de los puntos, el uso de instanceof y el downcasting es necesario para asegurar que la operación sea matemáticamente válida (no se puede calcular la distancia entre un punto 2D y uno 3D). Este diseño permite extender el sistema a puntos 4D o coordenadas polares en el futuro sin modificar la clase Linea.

Java
public abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}

public class Punto2D extends Punto {
    private double x, y;
    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) throw new IllegalArgumentException();
        Punto2D p = (Punto2D) otro; // Downcasting
        return Math.sqrt(Math.pow(p.x - this.x, 2) + Math.pow(p.y - this.y, 2));
    }
}

public class Linea {
    private Punto p1, p2;
    public Linea(Punto p1, Punto p2) { this.p1 = p1; this.p2 = p2; }
    public double getLongitud() { return p1.calcularDistanciaA(p2); }
}

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta

Al igual que las clases, las interfaces pueden heredar de otras interfaces utilizando la palabra clave extends. Esto permite crear jerarquías de capacidades, donde una interfaz hija acumula las promesas de métodos de su madre. A diferencia de las clases, una interfaz sí puede heredar de múltiples interfaces a la vez, lo que permite combinar contratos de manera muy granular.

En el ejemplo del fichero, FicheroEscribible hereda la capacidad de lectura de Fichero y añade nuevas responsabilidades. Cualquier clase que decida implementar FicheroEscribible estará obligada por el compilador a proporcionar el código para todos los métodos definidos en toda la cadena de herencia de la interfaz, asegurando un comportamiento completo.

Java
public interface Fichero {
    String leer();
}

public interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void eliminar();
}