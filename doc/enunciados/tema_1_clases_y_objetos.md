<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta

La Programación Orientada a Objetos (POO) se fundamenta en cuatro pilares que permiten gestionar la complejidad del software. El abstracción consiste en identificar las características esenciales de un objeto, ignorando los detalles irrelevantes para el sistema; por ejemplo, de un coche solo interesa su matrícula y posición para un gestor de tráfico, no el color de los asientos. El encapsulamiento permite agrupar datos y métodos en una misma unidad, protegiendo el estado interno de interferencias externas mediante niveles de acceso.

La herencia es el mecanismo por el cual una clase nueva (clase hija) adquiere los atributos y comportamientos de una clase existente (clase padre), facilitando la reutilización de código. Finalmente, el polimorfismo permite que una misma operación se comporte de manera distinta según el objeto que la realice, permitiendo tratar objetos de diferentes tipos de forma uniforme siempre que compartan una interfaz común.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

Existen numerosos lenguajes que implementan este paradigma, cada uno con sus matices. Java es uno de los más emblemáticos, diseñado bajo la premisa de "escribir una vez, ejecutar en cualquier lugar" y puramente orientado a objetos. C++, por su parte, es una extensión de C que añade capacidades de objetos, permitiendo un control de bajo nivel junto con abstracciones complejas.

Otros lenguajes de gran calado son Python, conocido por su sintaxis limpia y su flexibilidad al tratar todo (incluso los números) como objetos, y C#, desarrollado por Microsoft para competir en el ecosistema empresarial con una estructura muy robusta. Estos lenguajes dominan actualmente el desarrollo de aplicaciones móviles, videojuegos y sistemas de gran escala.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta

La programación estructurada es un paradigma basado en el uso de estructuras de control (bucles, condicionales) y funciones, evitando el uso de saltos incondicionales de código (como el goto). Se centra en la división del flujo del programa en bloques lógicos jerárquicos, lo que mejora la legibilidad y el mantenimiento en comparación con el código secuencial primitivo que se asemeja a "espagueti".

Por otro lado, la programación modular da un paso más allá al organizar el código en archivos o unidades independientes llamadas módulos. Cada módulo agrupa funciones y datos relacionados, permitiendo que un equipo trabaje en partes distintas del sistema sin interferencias. Es la base sobre la que luego se asienta la POO, aunque en estos paradigmas los datos y las funciones que los manipulan suelen estar separados.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta

Un objeto se define fundamentalmente por su estado, su comportamiento y su identidad. El estado está compuesto por los atributos o datos que el objeto almacena en un momento dado, representando sus propiedades (por ejemplo, la velocidad actual de un coche). El comportamiento se define mediante los métodos o funciones que el objeto puede ejecutar, que son las acciones que puede realizar o las reacciones ante estímulos (acelerar, frenar).

La identidad es lo que permite distinguir a un objeto de otro, incluso si ambos tienen exactamente el mismo estado (los mismos valores en sus atributos). En memoria, esto se traduce en que cada objeto ocupa una dirección única, de la misma forma que dos gemelos idénticos son dos personas distintas aunque tengan el mismo aspecto y nombre.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta

Una clase se define como el plano, molde o plantilla que describe cómo será un objeto; especifica qué variables tendrá y qué funciones podrá ejecutar. No es un objeto en sí mismo, sino la definición teórica. El objeto es la entidad física que se crea en la memoria siguiendo ese molde, y al acto de crear ese objeto se le denomina instancia. Por tanto, un objeto es una instancia de una clase.

Aunque la mayoría de lenguajes populares (Java, C++, C#) usan clases, no todos los lenguajes orientados a objetos lo hacen. Existen lenguajes basados en prototipos, como JavaScript, donde los objetos se crean clonando otros objetos ya existentes en lugar de usar una plantilla de clase formal. En estos sistemas, la jerarquía y estructura se definen de forma más dinámica.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta

En la mayoría de los lenguajes modernos como Java, los objetos se almacenan en una región de memoria dinámica llamada Heap (montículo). A diferencia de las variables locales simples que van al Stack (pila), los objetos en el Heap pueden persistir más allá del ámbito de la función que los creó. Sin embargo, en lenguajes como C++, es posible elegir si un objeto se guarda en el Stack (gestión automática) o en el Heap (gestión manual).

La recolección de basura (Garbage Collection) es un proceso automático del sistema encargado de liberar la memoria ocupada por objetos que ya no tienen ninguna referencia activa. En lenguajes como C, el programador debe liberar la memoria manualmente (usando free), lo que suele causar errores de fugas de memoria. En Java, el recolector identifica qué objetos son inalcanzables y los elimina de forma transparente para el usuario.

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta

Un método es una función que está definida dentro de una clase y que representa el comportamiento de los objetos de dicha clase. A diferencia de una función global en C, un método siempre actúa en el contexto de un objeto específico y tiene acceso a sus atributos internos. Es el mecanismo principal para interactuar con la información encapsulada en el objeto.

La sobrecarga de métodos consiste en definir varios métodos con el mismo nombre dentro de la misma clase, pero con diferentes listas de parámetros (ya sea en número o en tipo de datos). El compilador decide qué versión ejecutar basándose en los argumentos pasados durante la llamada. Esto permite que una acción, como "dibujar", pueda aceptar distintos tipos de entrada (coordenadas enteras o un objeto de tipo Color) sin cambiar de nombre.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta

Se presenta a continuación la definición de la clase Punto. En Java, las operaciones matemáticas se realizan mediante la clase Math, como el cálculo de la raíz cuadrada ($\sqrt{x}$) y la potencia ($x^n$).Javapublic class Punto {
    double x; // Atributos con visibilidad por defecto
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(Math.pow(x, 2) + Math.pow(y, 2));
    }
}
Para utilizar esta clase, se debe instanciar el objeto y asignar valores a sus campos antes de invocar el comportamiento deseado.Javapublic class Principal {
    public static void main(String[] args) {
        Punto miPunto = new Punto();
        miPunto.x = 3.0;
        miPunto.y = 4.0;
        
        double distancia = miPunto.calculaDistanciaAOrigen();
        System.out.println("Distancia: " + distancia);
    }
}


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

El punto de entrada es siempre el método public static void main(String[] args). La palabra clave static indica que el método pertenece a la clase en sí y no a una instancia específica; esto permite que la Máquina Virtual de Java lo ejecute sin necesidad de crear un objeto previo con new. Sin static, el programa no podría arrancar porque no habría objeto inicial desde el cual llamar a main.No es exclusivo de main; se usa en atributos y métodos que deben compartirse entre todos los objetos de la misma clase (por ejemplo, un contador de instancias). Cuando se combina con final, se crean constantes de clase. Un atributo static final es una variable que no cambia de valor y que es única para toda la aplicación, como el valor de $\pi$.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta

Java es un lenguaje híbrido. Primero se utiliza el comando javac Programa.java, que es el compilador; este no genera código máquina directo para el procesador, sino un código intermedio universal llamado Bytecode, guardado en ficheros .class. Posteriormente, se ejecuta con java Programa, que lanza la Máquina Virtual de Java (JVM) para interpretar o traducir ese bytecode al hardware específico en tiempo de ejecución.

La JVM actúa como una capa de abstracción que permite que el mismo fichero .class funcione en Windows, Linux o Mac sin cambios. Por tanto, se dice que Java es "compilado a bytecode e interpretado por la máquina virtual". Esto garantiza la portabilidad del lenguaje a costa de una pequeña sobrecarga de rendimiento compensada por optimizadores modernos.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta

El operador new es el encargado de solicitar memoria en el Heap para un nuevo objeto y de invocar al constructor. El constructor es un método especial que tiene el mismo nombre que la clase y no devuelve ningún tipo de dato; su función principal es inicializar los atributos del objeto en el momento de su creación para asegurar que el objeto nazca en un estado válido.

A continuación se muestra el ejemplo de la clase Empleado con su constructor correspondiente:

Java
public class Empleado {
    String dni;
    String nombre;
    String apellidos;

    // Constructor
    public Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta

La palabra clave this es una referencia que el objeto utiliza para referirse a sí mismo. Es especialmente útil dentro de los constructores o métodos cuando los nombres de los parámetros coinciden con los nombres de los atributos de la clase (lo que se conoce como "sombreado de variables"). No se llama igual en todos los lenguajes; por ejemplo, en Python se utiliza self.

En el siguiente ejemplo, se observa cómo this permite diferenciar el atributo de la clase del parámetro recibido en el constructor:

Java
public class Punto {
    double x;
    double y;

    public Punto(double x, double y) {
        this.x = x; // 'this.x' es el atributo, 'x' es el parámetro
        this.y = y;
    }
}

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta

El método distanciaA utiliza la fórmula de la distancia euclídea entre dos puntos: $d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}$. En este caso, $(x_1, y_1)$ son las coordenadas del objeto actual (this) y $(x_2, y_2)$ las del objeto pasado por parámetro.Javapublic double distanciaA(Punto otroPunto) {
    double dx = otroPunto.x - this.x;
    double dy = otroPunto.y - this.y;
    return Math.sqrt(Math.pow(dx, 2) + Math.pow(dy, 2));
}
Este diseño permite que un objeto de tipo Punto interactúe con otro de su misma clase de manera natural, accediendo a los datos del parámetro otroPunto para realizar el cálculo.

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta

En Java, técnicamente todo se pasa por valor, pero hay una distinción crítica: en los objetos, lo que se pasa por valor es la referencia (la dirección de memoria). Esto significa que si se modifican los atributos del Punto dentro del método, los cambios sí afectan al objeto original fuera de él, ya que ambos apuntan al mismo lugar en el Heap. Sin embargo, si se intenta reasignar la variable entera a un "nuevo objeto", la variable original fuera del método no cambiará su puntero.

Por el contrario, los tipos primitivos como int se pasan por copia de su valor real. Si se recibe un int y se modifica dentro de la función, el cambio es local al método y no afecta a la variable original. Esta diferencia es fundamental para evitar efectos secundarios no deseados al trabajar con datos básicos frente a estructuras complejas.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta

El método toString() es un método heredado de la clase base Object que devuelve una representación en cadena de texto del objeto. Se invoca automáticamente cuando se intenta imprimir un objeto (por ejemplo, en System.out.println). Casi todos los lenguajes tienen un concepto equivalente, como __str__ o __repr__ en Python, para facilitar la depuración y visualización de datos.

Se recomienda sobrescribir este método para proporcionar información útil en lugar de la dirección de memoria por defecto:

Java
@Override
public String toString() {
    return "Punto[x=" + x + ", y=" + y + "]";
}


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta

Una clase puede verse como una evolución de un struct de C, pero con diferencias conceptuales profundas. Un struct es puramente un contenedor de datos (un registro), mientras que una clase une esos datos con el comportamiento (los métodos). En C, los datos están "muertos" y necesitan funciones externas que los procesen; en la POO, los objetos son entidades "vivas" que saben realizar acciones sobre sí mismos.

Para que un struct fuera como una clase, le faltaría la capacidad de incluir funciones en su interior, mecanismos de ocultación de datos (privacidad) y la capacidad de herencia. Además, el concepto de "instancia" implica una gestión de ciclo de vida (constructores/destructores) y una identidad en memoria que el struct básico maneja de forma mucho más manual y primitiva.

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta

Para emular la POO en C, se define un struct para los datos y funciones externas que reciben un puntero a esa estructura como primer argumento. Ese puntero manual es exactamente lo que Java hace de forma automática con this.

C
// Definición de datos
typedef struct {
    double x;
    double y;
} Punto;

// Emulación del método (this es el puntero p)
double calculaDistanciaAOrigen(Punto* p) {
    return sqrt(pow(p->x, 2) + pow(p->y, 2));
}
En este enfoque, this no desaparece, simplemente se vuelve explícito. Mientras que en Java escribimos miPunto.calculaDistancia(), en C haríamos calculaDistancia(&miPunto). La POO "esconde" ese paso del puntero al objeto para que el código sea más limpio y esté mejor organizado.
