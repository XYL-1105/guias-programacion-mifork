<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta

La herencia es un mecanismo fundamental de la orientación a objetos que permite definir una nueva clase a partir de una ya existente. La relación "A es-un B" establece que la subclase es una especialización de la superclase. Esto significa que cualquier instancia de la subclase posee todas las características y capacidades de la clase base, además de las suyas propias. Es una evolución sobre los struct de C, donde para lograr algo similar habría que incluir una estructura dentro de otra manualmente sin que el compilador reconozca una relación de jerarquía.

Esta relación tiene dos implicaciones críticas. Primero, la herencia de estado y comportamiento, que permite que los atributos y métodos de la superclase se repliquen en la subclase automáticamente. Segundo, la compatibilidad de tipos (polimorfismo), que permite tratar a un objeto de la subclase como si fuera de la superclase. Esto es extremadamente útil para gestionar colecciones de objetos heterogéneos bajo una misma interfaz común.

Java
public class Soldado {
    private String nombre;
    public Soldado(String nombre) { this.nombre = nombre; }
    public void saludar() { System.out.println("Soy el soldado " + nombre); }
}

public class Artillero extends Soldado {
    private int cohetes;
    public Artillero(String nombre, int cohetes) { super(nombre); this.cohetes = cohetes; }
    public int getCohetes() { return cohetes; }
}

public class Zapador extends Soldado {
    private int minas;
    public Zapador(String nombre, int minas) { super(nombre); this.minas = minas; }
    public int getMinas() { return minas; }
}

// Ejemplo de uso y compatibilidad de tipos
Soldado[] ejercito = { new Artillero("Ivan", 10), new Zapador("Ana", 5) };
for (Soldado s : ejercito) { s.saludar(); }


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta

En Java, la creación de un objeto de una subclase implica una reacción en cadena de constructores. Siempre se ejecuta primero el constructor de la superclase y luego el de la subclase. Esto garantiza que la "base" del objeto esté correctamente inicializada antes de que la parte especializada intente añadir o modificar algo. Incluso si no se ve en el código, Java inserta una llamada invisible a super() en la primera línea de cada constructor.

La palabra reservada super se utiliza dentro de un constructor para invocar explícitamente a un constructor específico de la superclase. Si la clase base no tiene un constructor sin parámetros (o este no es visible), es obligatorio llamar a super pasando los argumentos necesarios. De lo contrario, el compilador dará un error, ya que no sabría cómo construir la parte "padre" del objeto antes de seguir con la "hija".

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta

Cuando se instancia una subclase, el objeto en memoria reserva espacio para todos los atributos definidos en la jerarquía, incluidos los marcados como private en la superclase. Un objeto Artillero tiene en su bloque de memoria el espacio para el nombre (heredado de Soldado) y para los cohetes. Físicamente, el objeto es un bloque único que contiene la suma de todos los campos.

Sin embargo, la visibilidad es un concepto distinto a la existencia en memoria. Aunque el atributo nombre reside dentro del objeto Artillero, el código escrito dentro de la clase Artillero no puede acceder directamente a él si es private. Para interactuar con ese dato, la subclase debe usar los métodos públicos o protegidos que la superclase proporcione. Es una forma de proteger la integridad de los datos: solo la clase que definió el atributo sabe cómo gestionarlo correctamente.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta

La compatibilidad de tipos es la piedra angular de la extensibilidad en sistemas complejos. Permite que el código que gestiona objetos (como el bucle que pide saludos) se escriba de forma genérica, apuntando a la superclase. Gracias a esto, el sistema puede crecer con nuevos tipos de datos sin necesidad de tocar, recompilar o testear de nuevo la lógica que ya funciona.

Si se añade, por ejemplo, un Medico, este nuevo tipo se integra perfectamente en las colecciones de Soldado existentes. El código del "cliente" que recorre el array no cambia en absoluto, demostrando el principio de que el código debe estar abierto a la extensión pero cerrado a la modificación.

Java
public class Medico extends Soldado {
    public Medico(String nombre) { super(nombre); }
    // El método saludar se hereda automáticamente
}

// El código anterior sigue funcionando sin cambios:
Soldado[] ejercito = { new Artillero("Ivan", 10), new Medico("Elena") };
for (Soldado s : ejercito) { s.saludar(); }

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta

Una referencia de un supertipo puede apuntar a cualquier objeto de sus subtipos; sin embargo, la referencia limita qué métodos se pueden ver. Con una referencia de tipo Soldado, solo se pueden invocar métodos definidos en Soldado, aunque el objeto real sea un Artillero. El upcasting es esta subida en la jerarquía (automática y segura), mientras que el downcasting es el proceso inverso y manual para recuperar la interfaz del subtipo.

Para realizar un downcasting seguro, se emplea el operador instanceof, que comprueba el tipo real del objeto en tiempo de ejecución. Esto evita errores catastróficos si intentamos tratar como artillero a algo que no lo es. Es el equivalente a realizar una comprobación de tipo antes de hacer un "cast" de punteros en C, pero con la seguridad que aporta la máquina virtual de Java.

Java
for (Soldado s : ejercito) {
    if (s instanceof Artillero) {
        Artillero a = (Artillero) s; // Downcasting
        System.out.println("Cohetes: " + a.getCohetes());
    }
}

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta

El modificador protected es un nivel intermedio de visibilidad diseñado específicamente para la herencia. Un atributo o método protegido es accesible para la propia clase, para todas sus subclases (sin importar en qué paquete estén) y para otras clases dentro del mismo paquete. En C, esto no existe: o un miembro es accesible globalmente o está limitado al archivo/estructura.

Se implementa mediante la palabra clave protected antes de la declaración del miembro. Es útil cuando queremos que las subclases tengan libertad para manipular datos internos por eficiencia, pero no queremos que el resto del mundo exterior pueda verlos.

Java
public class Soldado {
    protected String nombre; // Accesible para hijos
}

public class Zapador extends Soldado {
    public void ponerBomba() {
        // Uso directo del nombre gracias a ser protected
        System.out.println(nombre + " está colocando una bomba...");
    }
}

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta

El modificador protected es un nivel intermedio de visibilidad diseñado específicamente para la herencia. Un atributo o método protegido es accesible para la propia clase, para todas sus subclases (sin importar en qué paquete estén) y para otras clases dentro del mismo paquete. En C, esto no existe: o un miembro es accesible globalmente o está limitado al archivo/estructura.

Se implementa mediante la palabra clave protected antes de la declaración del miembro. Es útil cuando queremos que las subclases tengan libertad para manipular datos internos por eficiencia, pero no queremos que el resto del mundo exterior pueda verlos.

Java
public class Soldado {
    protected String nombre; // Accesible para hijos
}

public class Zapador extends Soldado {
    public void ponerBomba() {
        // Uso directo del nombre gracias a ser protected
        System.out.println(nombre + " está colocando una bomba...");
    }
}

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta
La herencia múltiple es la capacidad de una clase de heredar de más de una superclase simultáneamente (por ejemplo, que un CocheAnfibio herede de Coche y de Barco). En Java, no existe la herencia múltiple de clases. Esto se decidió para evitar problemas de ambigüedad, como el famoso "problema del diamante", donde el compilador no sabría qué versión de un método ejecutar si ambas superclases lo definen.

En lugar de herencia múltiple, Java ofrece las interfaces, que permiten que una clase cumpla con múltiples contratos o roles sin heredar la estructura interna de varias clases. Es una solución más limpia que evita los conflictos de memoria y de nombres que suelen aparecer en lenguajes como C++.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta

En Java, las excepciones son objetos que forman parte de una jerarquía de herencia cuya raíz es Throwable. Al crear una excepción personalizada, se suele heredar de RuntimeException si queremos que sea una excepción no controlada (unchecked), es decir, que no obligue al programador a usar try-catch o throws explícitamente.

Al ser objetos, podemos añadirles atributos para transportar información extra sobre el error. En el siguiente ejemplo, la excepción guarda el objeto Usuario que falló, facilitando el diagnóstico del problema. Además, se sobrecarga el constructor para permitir el encadenamiento de excepciones (causas).

Java
public class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario u) {
        super("No se encontró el usuario");
        this.usuario = u;
    }

    public UsuarioNoEncontradoException(Usuario u, Throwable causa) {
        super("No se encontró el usuario", causa);
        this.usuario = u;
    }
}

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta
Usar herencia solo por reutilizar código es un error de diseño común. Si la clase A solo necesita algunas funciones de B, pero no "es-un" B, la herencia introduce una carga innecesaria. Al heredar, te llevas todo el paquete: atributos que quizás no necesites y métodos que podrían no tener sentido en el nuevo contexto, ensuciando la interfaz de la clase.

La reutilización mediante herencia es rígida y se define en tiempo de compilación. Si solo buscas funcionalidad, la composición es más limpia, ya que permite delegar tareas a otro objeto sin comprometer la identidad de tu clase ni heredar comportamientos no deseados.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta
Este principio sugiere que es preferible construir clases complejas combinando objetos simples (composición) en lugar de crear jerarquías profundas. La composición es mucho más flexible, ya que las relaciones se pueden cambiar o configurar incluso en tiempo de ejecución, mientras que la herencia es estática y permanente.

Además, la composición facilita las pruebas unitarias y el mantenimiento. Es más fácil sustituir un objeto interno por otro que cumpla la misma función que intentar modificar el comportamiento de una superclase de la que dependen decenas de subclases, donde un pequeño cambio puede provocar errores en cascada por toda la jerarquía.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta
Se dice que la herencia rompe la encapsulación porque la subclase depende de los detalles de implementación de la superclase. Al tener acceso a miembros protected o confiar en cómo funcionan los métodos internos del padre, el "hijo" está demasiado acoplado. Si la superclase cambia su lógica interna, puede romper accidentalmente el funcionamiento de las subclases sin que estas hayan sido modificadas.

Esto crea una relación de "caja blanca", donde el desarrollador de la subclase necesita conocer las tripas de la superclase para extenderla correctamente. En cambio, la composición mantiene una relación de "caja negra", donde los objetos solo interactúan a través de sus interfaces públicas, respetando totalmente el principio de ocultación de información.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta

A continuación se comparan ambas alternativas. En la herencia, se asume que Estudiante y Trabajador comparten una esencia común. En la composición, se asume que ambos "tienen" un conjunto de datos personales, lo cual es más flexible si, por ejemplo, quisiéramos que un Estudiante pudiera convertirse en Trabajador dinámicamente.

Modelado por Herencia:

Java
public class Persona {
    protected String dni, nombre;
    public Persona(String dni, String nombre) { this.dni = dni; this.nombre = nombre; }
}
public class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) { super(dni, nombre); }
}
Modelado por Composición:

Java
public class DatosPersonales {
    private String dni, nombre;
    public DatosPersonales(String dni, String nombre) { this.dni = dni; this.nombre = nombre; }
}
public class Estudiante {
    private DatosPersonales datos;
    public Estudiante(DatosPersonales datos) { this.datos = datos; }
}