<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta
Un puntero a una función en lenguajes como C o C++ es una variable que almacena la dirección de memoria donde residen las instrucciones de una función compilada, en lugar de almacenar un valor de datos estándar. Este mecanismo permite invocar funciones de manera dinámica en tiempo de ejecución, pasarlas como argumentos a otras funciones (conocidas como funciones callback) o almacenarlas en estructuras de datos, proporcionando gran flexibilidad en el diseño del software.

Para declarar un puntero a función, es necesario especificar la firma exacta de la función a la que apuntará, lo que incluye el tipo de valor de retorno y los tipos de los parámetros. Una vez asignada la dirección de la función objetivo al puntero, este se puede utilizar para realizar la llamada ejecutando el bloque de código correspondiente de forma transparente, como si se tratase del nombre original de la función.

A continuación, se ilustra este concepto mediante un ejemplo en C. Se define una función que recibe una cadena de caracteres, la modifica en el mismo espacio de memoria para convertirla a mayúsculas y retorna el puntero original. Posteriormente, se crea una variable puntero local que referencia a dicha función y se efectúa la llamada a través de él.

C
#include <stdio.h>
#include <ctype.h>

// Definición de la función
char* convertirMayusculas(char* cadena) {
    char* original = cadena;
    while (*cadena) {
        *cadena = toupper((unsigned char)*cadena);
        cadena++;
    }
    return original;
}

int main() {
    char texto[] = "hola mundo";
    
    // Declaración del puntero a la función y asignación
    char* (*aMayusculas)(char*) = convertirMayusculas;
    
    // Invocación a través del puntero
    char* resultado = aMayusculas(texto);
    
    printf("%s\n", resultado); // Imprime: HOLA MUNDO
    
    return 0;
}

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta
Una función lambda, también conocida como función anónima, es un bloque de código autónomo que no necesita un identificador o nombre formal para ser definido. En muchos lenguajes modernos, las expresiones lambda proporcionan una sintaxis muy concisa para instanciar funciones pequeñas y pasarlas directamente como argumentos o asignarlas a variables, simplificando drásticamente el código que tradicionalmente requeriría la declaración de funciones completas o clases anónimas.

En JavaScript, las funciones lambda se introdujeron de forma generalizada mediante las "arrow functions" (funciones flecha). Al carecer de tipado estricto, la declaración resulta extremadamente compacta. Se asigna directamente la expresión lógica a una constante o variable, la cual actúa a partir de ese momento como una referencia invocable a la función definida.

JavaScript
// Ejemplo en JavaScript
const aMayusculas = texto => texto.toUpperCase();

const resultado = aMayusculas("hola mundo");
console.log(resultado); // Imprime: HOLA MUNDO
En Java, dado que es un lenguaje fuertemente tipado y orientado a objetos, las funciones lambda se respaldan mediante interfaces. Utilizando el concepto de genericidad (visto en temas anteriores), se emplea la interfaz Function<T, R> para declarar una variable que reciba un tipo T (en este caso, un String) y retorne un tipo R (también String).

Java
import java.util.function.Function;

public class EjemploLambda {
    public static void main(String[] args) {
        // Ejemplo en Java
        Function<String, String> aMayusculas = texto -> texto.toUpperCase();
        
        String resultado = aMayusculas.apply("hola mundo");
        System.out.println(resultado); // Imprime: HOLA MUNDO
    }
}

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta
El paradigma funcional es un estilo de programación declarativo basado en el concepto matemático de la evaluación de funciones. A diferencia de la programación imperativa o la orientada a objetos convencional (que dependen de estados mutables y secuencias de instrucciones que cambian dicho estado paso a paso), la programación funcional enfatiza el uso de funciones puras, inmutabilidad de los datos y evita los efectos secundarios. El resultado de una función depende exclusivamente de los argumentos de entrada.

Se considera a lenguajes como Java (a partir de la versión 8) o C++ (a partir de C++11) como lenguajes multi-paradigma porque no obligan al desarrollador a utilizar un único enfoque. Mantienen toda la infraestructura de la programación orientada a objetos (clases, herencia, encapsulación), pero incorporan herramientas y constructos sintácticos que permiten aplicar técnicas del paradigma funcional. Esto facilita escribir partes del sistema orientadas a estados y otras partes basadas en transformaciones puras de datos.

La expresión "ciudadanos de primera clase" se refiere a cómo el lenguaje trata a las funciones. Si un lenguaje otorga a las funciones esta categoría, significa que se pueden utilizar exactamente igual que cualquier otro tipo de dato primitivo o un objeto. Es decir, una función puede almacenarse en una variable, pasarse como argumento a otra función, ser retornada como resultado y ser instanciada en tiempo de ejecución.

## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta
La sintaxis básica de una expresión lambda en Java se compone de tres partes fundamentales: una lista de parámetros separada por comas y encerrada entre paréntesis, el operador de flecha -> y, finalmente, el cuerpo de la función. El operador de flecha sirve para separar visual y lógicamente la entrada de los datos de la operación o transformación que se va a aplicar sobre ellos.

En cuanto a los parámetros, Java es capaz de inferir su tipo a partir del contexto en el que se define la lambda (por ejemplo, a partir de la firma de la interfaz funcional de destino), por lo que habitualmente se omite la declaración explícita de los tipos. Además, si la función recibe un único parámetro, los paréntesis iniciales pueden eliminarse por completo, haciendo la expresión aún más limpia.

El cuerpo de la función puede presentarse de dos formas. Si la lógica consta de una única instrucción, se puede redactar como una sola expresión sin llaves {} ni la palabra reservada return, ya que el compilador deduce el retorno implícito. Si, por el contrario, la lógica requiere múltiples líneas o variables temporales, es obligatorio delimitar el bloque con llaves y utilizar explícitamente return si se espera un valor de vuelta.

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta
Recibir una función como parámetro es una de las características clave de los lenguajes que soportan el paradigma funcional, dando lugar a lo que se conoce como funciones de orden superior (higher-order functions). Esto permite separar el algoritmo estructural (qué hacer con el dato) de la operación específica (cómo transformarlo), fomentando la reutilización del código en distintos escenarios sin necesidad de recurrir al polimorfismo clásico mediante herencia.

En JavaScript, dado su tipado dinámico, basta con definir el método transformar declarando un parámetro convencional que representará la función de transformación. Dentro de la implementación, este parámetro se invoca directamente pasándole la cadena original, tal como se ejecutaría cualquier función normal.

JavaScript
// Definición de la función de orden superior en JS
function transformar(texto, funcionTransformadora) {
    return funcionTransformadora(texto);
}

const aMayusculas = texto => texto.toUpperCase();
const resultado = transformar("hola mundo", aMayusculas);
console.log(resultado); // Imprime: HOLA MUNDO
En Java, se debe recurrir al uso de interfaces funcionales para establecer un contrato estricto de tipos. El método transformar se declarará requiriendo un objeto de tipo Function<String, String> como segundo parámetro. Para invocar la lógica que encapsula dicho parámetro en el interior del método, es necesario llamar al único método abstracto que define esa interfaz, que en el caso de Function es apply().

Java
import java.util.function.Function;

public class EjemploOrdenSuperior {
    
    // Método que recibe una función como parámetro en Java
    public static String transformar(String texto, Function<String, String> funcionTransformadora) {
        return funcionTransformadora.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = t -> t.toUpperCase();
        String resultado = transformar("hola mundo", aMayusculas);
        System.out.println(resultado); // Imprime: HOLA MUNDO
    }
}

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta
Una de las grandes ventajas de la sintaxis lambda es que evita la necesidad de declarar variables o métodos temporales para tareas de un solo uso. Es posible redactar la lógica de la función directamente en el espacio reservado para el argumento en la invocación del método. Esto se conoce como una definición anónima in-line (en línea), y permite leer la operación y su contexto de aplicación de forma unificada.

Cuando el compilador analiza una llamada a un método que espera una interfaz funcional (en Java) o un callback (en JavaScript), verifica que la expresión lambda redactada en el momento cumple con la firma esperada. Si los tipos coinciden, se instancia la función al vuelo y se transfiere su ejecución al interior del método destino.

A continuación, se demuestra esta técnica en Java, invocando al método previamente definido transformar. En lugar de pasar una variable preexistente, se define sobre la marcha una lambda que utiliza StringBuilder para invertir la cadena, logrando un código compacto e inmediato. En JavaScript, el enfoque es conceptualmente idéntico.

Java
public class InvocacionInLine {
    // (Se asume la existencia del método estático transformar() del punto anterior)
    
    public static void main(String[] args) {
        // Se define y pasa la función lambda en la misma llamada
        String invertida = EjemploOrdenSuperior.transformar("hola", 
            s -> new StringBuilder(s).reverse().toString()
        );
        
        System.out.println(invertida); // Imprime: aloh
    }
}
JavaScript
// (Se asume la existencia de la función transformar() del punto anterior)

// Invocación in-line en JavaScript
const invertida = transformar("hola", s => s.split("").reverse().join(""));

console.log(invertida); // Imprime: aloh

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta
Un cierre, conocido comúnmente como closure, es un mecanismo por el cual una función lambda "captura" o recuerda el estado del entorno (ámbito léxico) en el que fue declarada. Esto significa que la función puede acceder a las variables locales del método que la envuelve, incluso si la función lambda se ejecuta más tarde o es devuelta y empleada fuera de ese contexto original. Esencialmente, el closure empaqueta en una única entidad el bloque de código junto con las referencias de los datos circundantes.

En Java, existe una restricción importante respecto a las variables capturadas por una lambda: deben ser efectivamente finales (effectively final). Esto implica que la variable local capturada no puede modificar su valor o referencia una vez ha sido inicializada, aunque no se utilice explícitamente la palabra reservada final. Esta regla existe para evitar problemas de concurrencia y mantener la coherencia temporal de los datos que la lambda almacena en su estado interno.

A continuación, se ilustra un closure en Java donde una expresión lambda accede a una variable local declarada antes de su propia definición. La lambda no solo procesa el argumento entrante, sino que se apoya en el entorno capturado (la variable prefijo) para conformar el resultado final.

Java
import java.util.function.Function;

public class EjemploClosure {
    
    public static String transformar(String texto, Function<String, String> funcionTransformadora) {
        return funcionTransformadora.apply(texto);
    }

    public static void main(String[] args) {
        // Variable local definida fuera de la lambda (efectivamente final)
        String prefijo = "El resultado es: ";
        
        // La lambda captura el 'prefijo' mediante un closure
        String resultado = transformar("Éxito total", s -> prefijo + s);
        
        System.out.println(resultado); // Imprime: El resultado es: Éxito total
    }
}

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta
La principal diferencia radica en el concepto de estado y contexto. Un puntero a función en lenguaje C almacena únicamente una dirección en memoria que apunta a una secuencia estática de instrucciones. El código en sí carece completamente de memoria sobre el lugar o el entorno donde el puntero fue asignado. Por tanto, para pasar un estado o contexto a la función apuntada en C, es imprescindible inyectarlo externamente a través de parámetros adicionales (habitualmente pasándole un puntero void* con la estructura de contexto).

Por el contrario, una función lambda provista de closures engloba no solo la porción de código a ejecutar, sino que encapsula una copia o referencia transparente de las variables del entorno léxico donde fue definida. Es una entidad con estado. La lambda agrupa comportamiento (instrucciones) y contexto (variables capturadas) de forma intrínseca, permitiendo que la firma de la función se mantenga limpia ya que los datos dependientes viajan automáticamente ocultos con la propia lambda.

Otra distinción crucial es de nivel arquitectónico. Los punteros en C son primitivas de muy bajo nivel gestionadas directamente sobre direcciones de memoria del proceso, lo que ofrece alto rendimiento pero bajo nivel de abstracción. Las expresiones lambda, particularmente en lenguajes como Java, son abstracciones de alto nivel que el compilador convierte internamente en instancias de objetos (implementando interfaces funcionales), delegando en el entorno de ejecución la gestión de la memoria, el ciclo de vida de los datos capturados y la validación estática de los tipos.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta
Otra capacidad inherente a las funciones como "ciudadanos de primera clase" es que un método puede tener como tipo de retorno una función. Esto permite diseñar componentes de software denominados "fábricas de funciones" o aplicar técnicas como la evaluación parcial (currying). El método recibe una configuración base y retorna una nueva función genérica configurada para operar repetidamente bajo esos parámetros iniciales.

El uso de un closure resulta vital en esta mecánica. Cuando se invoca crearDescuento(porcentaje), la lambda generada en su interior utiliza el argumento local porcentaje proporcionado en ese instante. Una vez que el método crearDescuento finaliza su ejecución, su contexto debería desaparecer, pero debido al closure, la lambda devuelta retiene el valor específico de porcentaje blindado en su interior, posibilitando su uso futuro cada vez que alguien aplique la función a un precio determinado.

A continuación, se demuestra en Java la creación del generador de funciones. Se crean dos instancias independientes (descuento del 10% y del 50%), comprobando que cada función lambda ha conservado su propio estado configurado mediante el closure, para posteriormente aplicar las funciones sobre un importe base.

Java
import java.util.function.Function;

public class GeneradorFunciones {

    // Método que devuelve una función lambda
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        // Se captura la variable 'porcentaje' en el closure
        return precio -> precio - (precio * (porcentaje / 100.0));
    }

    public static void main(String[] args) {
        // Se crean dos funciones diferentes almacenando su propio contexto
        Function<Double, Double> descuentoDiez = crearDescuento(10.0);
        Function<Double, Double> descuentoMitad = crearDescuento(50.0);

        double precioBase = 200.0;

        // Se aplican las funciones
        System.out.println("Precio con 10%: " + descuentoDiez.apply(precioBase));  // Imprime: 180.0
        System.out.println("Precio a mitad: " + descuentoMitad.apply(precioBase)); // Imprime: 100.0
    }
}

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta
Una interfaz funcional es una construcción específica de Java diseñada para actuar como tipo base formal de las expresiones lambda y referencias a métodos. A diferencia de lenguajes de tipado dinámico donde la lambda es una entidad autónoma, en Java el compilador necesita asociar una lambda a un tipo concreto y declarado en el sistema. Una interfaz funcional proporciona esa compatibilidad, permitiendo tratar la lambda de manera tipada y polimórfica.

El requisito principal e indispensable para que una interfaz se considere "funcional" es que contenga un y solo un método abstracto (lo que se conoce por sus siglas en inglés como SAM, Single Abstract Method). Este único método abstracto define la firma estricta (argumentos y tipo de retorno) a la cual la expresión lambda deberá ajustarse obligatoriamente al momento de asignarse a dicha interfaz.

De manera opcional, y como buena práctica de desarrollo, las interfaces funcionales suelen ir acompañadas de la anotación @FunctionalInterface. Esta anotación no otorga capacidades nuevas, pero fuerza al compilador a realizar una comprobación estricta: emitirá un error de compilación inmediato si un desarrollador intenta añadir un segundo método abstracto en el futuro, previniendo la ruptura accidental del contrato funcional. Cabe destacar que la presencia de métodos default o métodos estáticos dentro de la interfaz no viola la regla, ya que no son métodos abstractos.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta
Para definir una interfaz funcional personalizada en Java, se redacta una interfaz convencional pero limitando su contenido abstracto a un solo método. El uso de esta interfaz permite dotar de un significado semántico a la operación, mejorando la legibilidad del código de negocio al reemplazar identificadores genéricos por un nombre descriptivo como Transformador.

El siguiente código muestra la declaración formal de dicha interfaz. Incorpora la anotación de seguridad pertinente y define el contrato de un método que toma un objeto String como entrada y devuelve otro String transformado. A partir de esta definición, cualquier método del sistema podrá recibir un Transformador como parámetro, siendo compatible al instante con cualquier expresión lambda equivalente.

Java
@FunctionalInterface
public interface Transformador {
    // Único método abstracto (Single Abstract Method)
    String transformar(String texto);
}

// Ejemplo conceptual de uso:
// Transformador aMayusculas = str -> str.toUpperCase();

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta
Las interfaces funcionales alcanzan su máxima utilidad cuando se combinan con la programación genérica. Aplicando parámetros de tipo de la misma forma que se estudió para las clases genéricas, una interfaz funcional puede abstraerse no solo de la lógica del algoritmo, sino de los tipos de datos exactos con los que opera. Esto permite que una única interfaz sirva para definir transformaciones de cadenas de texto, conversiones matemáticas o procesamientos de entidades complejas sin necesidad de duplicar el código del contrato.

Al introducir un tipo origen genérico (convencionalmente <T>) y un tipo de retorno genérico (convencionalmente <R>), la interfaz establece una vinculación lógica durante la compilación. El usuario decidirá los tipos exactos en el instante en el que declare la variable o invoque el método, garantizando que el compilador realice la verificación de tipos adecuada al aplicar la expresión lambda.

A continuación, se define la interfaz Transformador<T, R> generalizada y se procede a instanciarla para un caso numérico. La lambda proporcionada toma el argumento Double, utiliza las utilidades matemáticas predefinidas para realizar un redondeo formal, y asume el moldeo (cast) necesario para devolver el tipo de salida especificado, Integer.

Java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T entrada);
}

public class EjemploTransformadorGenerico {
    public static void main(String[] args) {
        // Se define un transformador explícito de Double a Integer
        Transformador<Double, Integer> redondeador = numeroDouble -> (int) Math.round(numeroDouble);
        
        Integer resultado = redondeador.transformar(5.7);
        System.out.println(resultado); // Imprime: 6
    }
}

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta
Para evitar que todos los desarrolladores definan sus propias versiones idénticas de interfaces funcionales genéricas (como se hizo con Transformador), Java proporciona en el paquete java.util.function una biblioteca estándar completa y estandarizada. Estas interfaces abarcan los patrones operativos más habituales, fomentando la homogeneidad y la interoperabilidad en las colecciones y las llamadas a la API (Application Programming Interface).

Las interfaces genéricas fundamentales se pueden clasificar en cuatro grandes grupos principales basados en la naturaleza de su entrada y su salida:

Function<T, R>: Representa una transformación pura. Recibe un argumento de tipo T y devuelve un resultado de tipo R. Su método es apply().

Consumer<T>: Representa una acción final o efecto secundario. Recibe un argumento de tipo T pero no devuelve ningún resultado (void). Su método es accept().

Supplier<T>: Representa una provisión de datos. No recibe ningún argumento de entrada, pero devuelve un valor de tipo T. Su método es get().

Predicate<T>: Representa una condición lógica o filtro. Recibe un argumento de tipo T y devuelve siempre un valor booleano primitivo (boolean). Su método es test().

Adicionalmente, debido a que la genericidad en Java no admite tipos primitivos de manera eficiente sin forzar autoboxing (por ejemplo, usar Integer en lugar de int), el paquete ofrece decenas de variantes especializadas de estas interfaces diseñadas directamente para tipos primitivos básicos, mejorando considerablemente el rendimiento del software. Ejemplos de estas variantes son IntFunction<R>, DoubleConsumer, o IntToDoubleFunction.

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta
La introducción del paradigma funcional modificó significativamente la forma de recorrer colecciones en el lenguaje, promoviendo el paso de la iteración externa (mediante los tradicionales bucles for y bloques de iteradores expuestos) a la iteración interna. En la iteración interna, la estructura de datos toma el control de los saltos e índices internamente y simplemente solicita al programador el comportamiento concreto que debe aplicar individualmente sobre cada elemento.

Para lograr esto, las colecciones como List incorporaron el método forEach(). Este método actúa como receptor y espera un objeto del tipo de la interfaz predefinida Consumer<T>. Como se vio anteriormente, un Consumer permite inyectar el código que realizará una acción sin devolver ningún resultado. Esto proporciona una sintaxis altamente expresiva en la que el enfoque se desplaza hacia la declaración de las reglas de negocio sobre el dato, delegando la mecánica de la iteración.

En el siguiente bloque se observa la instanciación de una lista de enteros y su posterior recorrido funcional. Para cada elemento proporcionado por la lista a la expresión lambda, se aplica una condición de filtrado estándar e imprime la confirmación solicitada en caso de evaluación afirmativa.

Java
import java.util.Arrays;
import java.util.List;

public class IteracionFuncional {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-2, 5, -8, 10, 0, 3);
        
        // Uso expresivo de forEach con una expresión lambda actuando como Consumer<Integer>
        numeros.forEach(numero -> {
            if (numero > 0) {
                System.out.println("El número " + numero + " es positivo.");
            }
        });
    }
}

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta
El uso de los comodines o wildcards en las firmas de interfaces funcionales, como en Consumer<? super T>, está destinado a habilitar la flexibilidad del polimorfismo contravariante y covariante estudiado en genericidad. Si el método solo admitiera estrictamente Consumer<T>, forzaría a que la lambda declarara exactamente el mismo tipo que contiene la lista. Al emplear <? super T>, la función garantiza que es capaz de "consumir" de manera segura cualquier elemento de tipo T, dado que acepta una lambda diseñada para un tipo igual o incluso para cualquier clase superior de la que T herede (por ejemplo, procesar una lista de Integer usando un filtro genérico escrito para tipo base Number).

Este principio de asignación en Java se sintetiza en el acrónimo PECS: Producer Extends, Consumer Super. Esta norma dicta cómo utilizar los wildcards. Si una estructura se dedica a producir datos para la lectura, se debe usar ? extends T para permitir cualquier descendiente compatible. Si la estructura o método, por el contrario, debe consumir elementos como entrada (por ejemplo, para añadir a una lista o procesarlos en un Consumer), se debe usar ? super T para permitir cualquier implementación ascendente y genérica que abarque al tipo origen.

Al aplicar esta regla maestra (PECS) para mejorar y hacer verdaderamente robusto el método transformar(T entrada, Function<...>) en un entorno genérico, se devela su forma final. El argumento de tipo Function actúa simultáneamente como Consumidor de la entrada y como Productor del resultado. Por ende, para que el método tolere la mayor compatibilidad jerárquica posible, su parámetro funcional se define bajo las siglas PECS aplicando la contravarianza a la entrada y covarianza al retorno: Function<? super T, ? extends R>.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta
Una referencia a un método es una sintaxis que permite extraer la dirección o la lógica subyacente de un método existente en una clase u objeto para usarlo directamente como si fuese una función lambda o puntero. Se emplea habitualmente cuando el contenido de la expresión lambda no aporta ninguna lógica nueva y se limita simplemente a reenviar los parámetros de entrada hacia un método predefinido.

En JavaScript, los métodos son de facto propiedades que contienen funciones, por lo que se pueden referenciar y extraer asignándolas a una variable. Sin embargo, al extraer el método, este pierde su contexto lógico y la vinculación a su objeto original (la referencia del puntero interno this se corrompe). Para obtener la referencia de manera operativa, es forzoso asociarla de nuevo al objeto en memoria original empleando la función nativa bind(), consolidando el contexto para la futura ejecución.

JavaScript
// Ejemplo en JavaScript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }
    saludar() {
        console.log("Hola, soy " + this.nombre);
    }
}

const personaJS = new Persona("Ana");

// Extracción de la referencia asegurando el contexto del objeto
const referenciaSaludarJS = personaJS.saludar.bind(personaJS);

// Invocación a través de la referencia
referenciaSaludarJS(); // Imprime: Hola, soy Ana
En Java, este mecanismo se abstrae mediante la sintaxis del operador de doble par de puntos ::. Para almacenar la referencia a nivel de variable local, el compilador exige establecer una interfaz funcional que coincida con la firma del método interceptado. Dado que el método saludar no retorna nada ni pide argumentos, la interfaz predeterminada compatible es Runnable (que especifica un método run() vacío).

Java
public class EjemploReferenciaMetodo {
    static class Persona {
        String nombre;
        Persona(String nombre) { this.nombre = nombre; }
        
        public void saludar() {
            System.out.println("Hola, soy " + this.nombre);
        }
    }

    public static void main(String[] args) {
        Persona personaJava = new Persona("Carlos");
        
        // Referencia directa al método del objeto instanciado usando ::
        Runnable referenciaSaludarJava = personaJava::saludar;
        
        // Invocación ejecutando la interfaz compatible
        referenciaSaludarJava.run(); // Imprime: Hola, soy Carlos
    }
}

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta
El lenguaje Java clasifica las referencias a métodos en cuatro tipos diferenciados, según su naturaleza y la manera en que el sistema trata o invoca al objeto receptor de la llamada subyacente. La elección de una u otra sintaxis depende de si la acción implica instanciación de objetos, llamadas a la clase de forma global, o si se basa en objetos concretos pasados o prefijados.

Referencia a un método estático: El método de destino pertenece a la clase y no requiere un objeto. Se expresa como Clase::metodoEstatico. Ejemplo: la conversión numérica donde Function<String, Integer> se puede escribir como Integer::parseInt.

Referencia a un constructor: Funciona como un método de provisión, devolviendo una nueva instancia al ser invocado. Se expresa empleando la palabra reservada new mediante la sintaxis Clase::new. Ejemplo: un Supplier<List<String>> referenciado de forma abreviada como ArrayList::new.

Referencia a un método de instancia de un objeto específico: Esta modalidad enlaza la ejecución con un objeto que ya existe y que reside en la memoria, tal como se ilustró en el ejemplo de Persona anterior. La estructura general es instanciaConcreta::metodo. Ejemplo: si se posee un objeto listado (miLista), se extrae como miLista::size.

Referencia a un método de instancia sobre cualquier instancia (o parámetro entrante): Es el caso de uso más complejo conceptualmente. Se aplica cuando el propio objeto que ejecutará el método se inyectará como primer parámetro durante la llamada a la interfaz funcional. La estructura apela globalmente a la clase Clase::metodoInstancia. Ejemplo: el método universal de impresión para convertir a cadena donde una lista de objetos se pasa a String::toLowerCase, aplicando el método en cada instancia recibida sobre la marcha.

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta
El ordenamiento de colecciones complejas es un área donde las capacidades funcionales destacan en cuanto a expresividad y reducción de código repetitivo frente al estilo tradicional de la orientación a objetos, que obligaba a crear clases implementando interfaces para actuar como objetos-estrategia. Collections.sort() espera un Comparator<T>, el cual es, a efectos prácticos, una interfaz funcional cuyo único método abstracto (compare(T o1, T o2)) toma dos parámetros y retorna un valor numérico dictando la superioridad de uno frente al otro.

En la primera aproximación lógica, se desarrolla explícitamente el cuerpo de la función lambda proporcionando la implementación del contrato funcional. Se comparan primero numéricamente las edades; si dicha comparación establece desigualdad, se devuelve dicho dictamen numérico. Si el resultado evidencia la misma edad para ambos sujetos, se delega el criterio de desempate al ordenamiento alfanumérico predefinido del método compareTo propio de las cadenas de caracteres.

La segunda estrategia muestra el máximo nivel declarativo y de fluidez soportado por la API de Java moderna. La interfaz Comparator incorpora una serie de métodos estáticos y métodos por defecto (default methods) de diseño funcional. Estos admiten referencias a métodos (o pequeñas lambdas extractoras de datos) como Persona::getEdad o Persona::getNombre, encadenando internamente las lógicas lógicas jerárquicas de desempate mediante utilidades de la librería como comparingInt() seguidas de un elocuente método thenComparing(), minimizando enormemente el riesgo de error humano.

Java
import java.util.*;

public class OrdenamientoFuncional {
    static class Persona {
        String nombre;
        int edad;
        Persona(String nombre, int edad) { this.nombre = nombre; this.edad = edad; }
        public String getNombre() { return nombre; }
        public int getEdad() { return edad; }
        @Override public String toString() { return nombre + ":" + edad; }
    }

    public static void main(String[] args) {
        List<Persona> lista1 = Arrays.asList(
            new Persona("Zoe", 30), new Persona("Ana", 25), new Persona("Luis", 30));
        
        List<Persona> lista2 = new ArrayList<>(lista1); // Copia para el segundo método

        // VERSIÓN 1: Expresión lambda construyendo el comparador manualmente
        Collections.sort(lista1, (p1, p2) -> {
            int comparacionEdad = Integer.compare(p1.getEdad(), p2.getEdad());
            if (comparacionEdad != 0) {
                return comparacionEdad;
            }
            return p1.getNombre().compareTo(p2.getNombre());
        });

        // VERSIÓN 2: Enfoque expresivo utilizando las utilidades funcionales de Comparator
        Collections.sort(lista2, Comparator
            .comparingInt(Persona::getEdad)
            .thenComparing(Persona::getNombre)
        );

        System.out.println("Lambda manual: " + lista1); // Imprime: [Ana:25, Luis:30, Zoe:30]
        System.out.println("API Comparator: " + lista2); // Imprime: [Ana:25, Luis:30, Zoe:30]
    }
}