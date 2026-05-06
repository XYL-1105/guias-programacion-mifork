<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta
En lenguajes orientados a objetos como Java, todas las clases derivan implícita o explícitamente de la clase base Object. Por tanto, si se define una estructura que internamente almacena los datos en un array de tipo Object, dicha estructura será capaz de recibir referencias de cualquier tipo de objeto sin generar errores de compilación iniciales. En el lenguaje C, esto se consigue tradicionalmente empleando punteros genéricos void*, los cuales pueden apuntar a cualquier dirección de memoria independientemente del tipo de dato subyacente.

Para ilustrar este concepto en Java, se puede diseñar una estructura contenedora muy simple. Al instanciar el array interno como un Object[], el método para añadir elementos aceptará números, cadenas de texto o cualquier instancia de una clase personalizada, proporcionando una solución universal de almacenamiento, aunque carente de especificidad en los tipos almacenados.

Java
public class ContenedorUniversal {
    private Object[] elementos;
    private int contador;

    public ContenedorUniversal(int capacidad) {
        this.elementos = new Object[capacidad];
        this.contador = 0;
    }

    public void agregar(Object elemento) {
        if (contador < elementos.length) {
            elementos[contador++] = elemento;
        }
    }

    public Object obtener(int indice) {
        return elementos[indice];
    }
}
Al utilizar esta clase, es posible introducir elementos variados llamando a agregar("Texto") o agregar(42). No obstante, el método de recuperación obtener() devolverá siempre una referencia genérica de tipo Object, independientemente de lo que se hubiera introducido originalmente, delegando en el programador la responsabilidad de transformarlo a su formato adecuado posteriormente.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta
La programación genérica es un paradigma de diseño de software en el que los algoritmos y las estructuras de datos se escriben en términos de tipos que se especificarán más adelante. Su objetivo principal es maximizar la reutilización del código, permitiendo que una misma implementación funcione de forma transparente y segura con multitud de tipos de datos distintos sin necesidad de duplicar el código para cada uno de ellos.

El ejemplo anterior, basado en el uso de la clase Object o punteros void*, se puede considerar como una forma rudimentaria o primitiva de alcanzar la reutilización de código que persigue la programación genérica. Permite crear un único contenedor lógico válido para múltiples naturalezas de datos, logrando evitar la duplicación de clases como ContenedorStrings o ContenedorEnteros.

Sin embargo, desde el punto de vista de los estándares modernos de ingeniería de software, no se considera verdadera programación genérica porque carece de una de sus características fundamentales: la seguridad de tipos estática. La verdadera programación genérica permite parametrizar los tipos manteniendo íntegra la capacidad del compilador de realizar verificaciones, mientras que el uso de Object rompe el chequeo de tipos estricto y oculta posibles incompatibilidades.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta
El principal problema de emplear raíces jerárquicas como Object (o abstracciones de memoria como void*) radica en la pérdida absoluta de información de tipos durante la fase de compilación. Cuando un dato concreto entra en la estructura, su tipo original se diluye; en consecuencia, al extraer el dato de la estructura, el compilador desconoce de qué se trata realmente, lo que obliga al desarrollo a realizar una conversión explícita o downcasting (por ejemplo, (String) contenedor.obtener(0)).

Esta obligación de realizar conversiones manuales es altamente propensa a errores humanos. Dado que el contenedor acepta cualquier cosa, nada impide que por descuido se almacene un número entero donde lógicamente se esperaba una cadena de texto. El compilador, confiando en que todo es un Object, validará la inserción y la posterior compilación del cast sin mostrar ninguna advertencia o error.

Finalmente, este diseño defectuoso traslada la detección de inconsistencias de tipos desde una etapa segura y temprana (tiempo de compilación) hacia una etapa crítica (tiempo de ejecución). Si se intenta convertir ese entero almacenado por error a una cadena de texto durante la ejecución del programa, el sistema abortará con un error irrecuperable (en Java se lanzará una excepción ClassCastException), provocando caídas inesperadas en el software final.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta
Los parámetros de tipo son la herramienta formal que proporcionan los lenguajes de programación modernos para implementar una programación genérica robusta y tipada. De manera similar a como un método o función utiliza parámetros convencionales para recibir diferentes valores, una clase o método genérico utiliza parámetros de tipo para recibir diferentes tipos de datos. Actúan como marcadores de posición abstractos durante la definición del código.

A nivel sintáctico, estos parámetros se suelen representar entre signos de menor y mayor (como <T>, <E>, o <K, V>) y se declaran junto a la firma de la clase, interfaz o método. Al utilizarlos en el interior del bloque de código, reemplazan a los tipos concretos preexistentes (como int o String) y garantizan que, sea cual sea el tipo definitivo con el que se instancie la estructura, este se mantendrá coherente y constante en todas las variables y operaciones donde se haya referenciado dicho parámetro.

La introducción de los parámetros de tipo soluciona de raíz los problemas del chequeo manual. Al crear una instancia indicando un tipo concreto (por ejemplo, proporcionar un String para sustituir a <T>), el compilador memoriza y audita esa restricción. Bloqueará la introducción de datos incompatibles y adaptará automáticamente el tipo de retorno de las funciones de extracción, eliminando la necesidad de realizar conversiones de tipo manuales y previniendo los errores en tiempo de ejecución.

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta
Tanto en Java como en C++, el mecanismo de instanciación exige proporcionar entre corchetes angulares el tipo de dato definitivo que sustituirá a los parámetros de tipo declarados en la colección de la biblioteca estándar. Al realizar esto, se genera un contenedor especializado capaz de verificar su contenido de manera automática.

En Java, se emplea el sistema de Generics utilizando la interfaz List y su implementación ArrayList. Al instanciarla especificando el tipo <String>, el entorno garantiza que solo se admitan textos. Al recorrer los elementos mediante un bucle, cada iteración extrae directamente un objeto de tipo String, permitiendo invocar sobre él métodos de cadena (como toUpperCase()) de manera segura, sin conversiones adicionales.

Java
// Ejemplo en Java
import java.util.ArrayList;
import java.util.List;

public class EjemploJava {
    public static void main(String[] args) {
        List<String> listaDeCadenas = new ArrayList<>();
        listaDeCadenas.add("Hola");
        listaDeCadenas.add("Mundo");
        
        for (String elemento : listaDeCadenas) {
            // El elemento ya se extrae de forma segura como String
            System.out.println(elemento.toUpperCase());
        }
    }
}
En C++, el enfoque se materializa a través de las plantillas (Templates) de la biblioteca estándar (STL). El equivalente directo es std::vector, el cual se instancia indicando <std::string>. El funcionamiento externo resulta análogo al de Java: el compilador valida la inserción de las cadenas y, al recorrer el vector (comúnmente mediante referencias constantes para evitar copias), el sistema reconoce cada elemento estáticamente como su tipo original.

C++
// Ejemplo en C++
#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> vectorDeCadenas;
    vectorDeCadenas.push_back("Hola");
    vectorDeCadenas.push_back("Mundo");
    
    for (const std::string& elemento : vectorDeCadenas) {
        // Se asegura el tipo std::string en tiempo de compilación
        std::cout << elemento << std::endl;
    }
    return 0;
}

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta
Aunque la sintaxis de instanciación resulte similar en la superficie, los compiladores de Java y C++ aplican estrategias completamente distintas al procesar los parámetros de tipo. C++ genera código nuevo para cada tipo utilizado, mientras que Java reutiliza una única estructura lógica para todos los tipos posibles, demostrando que no funcionan de la misma manera a nivel interno.

En Java se utiliza el mecanismo denominado "Type Erasure" (borrado de tipos). Durante la compilación, el compilador utiliza la información de los genéricos (<T>) para realizar un análisis sintáctico estricto y comprobar que los tipos son correctos en todas las inserciones y asignaciones. Una vez validados, borra literalmente toda la información relacionada con el tipo genérico del código resultante (bytecode) y reemplaza el parámetro de tipo por la clase Object (o su límite superior). A la vez, inserta automáticamente todos los casts necesarios en los métodos de extracción. De este modo, en tiempo de ejecución solo existe una única clase compilada independiente del tipo parametrizado.

Por el contrario, C++ basa su funcionamiento en la "Instanciación de plantillas" (Template instantiation). Cuando el compilador detecta que se está usando un vector con el tipo std::string, toma el código fuente genérico original y genera físicamente una copia completa y nueva de la clase adaptada de forma explícita para tratar con std::string. Si después se instancia un vector numérico (int), el compilador creará una segunda clase paralela adaptada para enteros. Esto significa que en el ejecutable final coexistirán tantas versiones distintas del código como tipos diferentes se hayan parametrizado, lo que otorga mayor rendimiento pero aumenta el tamaño del archivo resultante.

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta
Para diseñar una clase que sea capaz de alojar dos valores de naturalezas completamente independientes, es necesario declarar la clase con dos parámetros de tipo distintos en su cabecera (convencionalmente nombrados como <T, U> o <K, V>). Estos parámetros se utilizarán a lo largo del cuerpo de la clase para definir los atributos internos, la firma del constructor que recibe los valores, y los tipos devueltos por los métodos observadores o getters.

El uso de múltiples parámetros de tipo resulta fundamental cuando se necesita agrupar datos relacionados pero heterogéneos sin tener que forzarlos a pertenecer a una misma jerarquía de herencia. Esta estructura es especialmente valiosa en lenguajes como Java que no admiten el retorno múltiple de valores nativo en sus métodos, ya que una instancia de un Par genérico actúa como una envoltura de transporte fuertemente tipada.

A continuación, se define la clase genérica Par con sus componentes requeridos, y se incluye un ejemplo que invoca un método calculador. Dicho método utiliza el Par para empaquetar de forma conjunta y tipada (en este caso <Double, Double>) la media y la desviación de una muestra analizada.

Java
// Definición de la clase con dos parámetros de tipo
public class Par<T, U> {
    private final T primero;
    private final U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() {
        return primero;
    }

    public U getSegundo() {
        return segundo;
    }
}

// Ejemplo de uso en el contexto de una operación de cálculo
public class CalculadoraEstadistica {
    
    public Par<Double, Double> obtenerMediaYDesviacion(double[] datos) {
        // Se asumen cálculos abstractos para ilustrar el ejemplo
        double mediaCalculada = 5.5; 
        double desviacionCalculada = 1.2;
        
        // Se empaquetan los valores instanciando el tipo Par
        return new Par<>(mediaCalculada, desviacionCalculada);
    }
    
    public void mostrarResultados() {
        double[] misDatos = {4.0, 5.0, 6.0, 7.0};
        Par<Double, Double> resultado = obtenerMediaYDesviacion(misDatos);
        
        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación: " + resultado.getSegundo());
    }
}

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta
Los métodos genéricos permiten aplicar el alcance de los parámetros de tipo de manera local, restringiendo su ciclo de vida únicamente a la invocación y ejecución de un método concreto. Su sintaxis requiere declarar el parámetro genérico (por ejemplo, <T>) inmediatamente antes del tipo de retorno del método. Esta técnica es ideal para operaciones utilitarias donde el comportamiento lógico del algoritmo no depende de datos del estado de una instancia de clase.

Si se implementara una función para devolver aleatoriamente uno de dos objetos utilizando el tipo raíz Object, se presentarían graves fallos en la solidez del sistema. Por un lado, permitiría la inserción desparejada (por ejemplo, pasar un String y un Integer como argumentos), careciendo de comprobación de homogeneidad. Por otro lado, como el método devolvería forzosamente un tipo genérico Object, obligaría al código llamador a realizar siempre un downcasting peligroso (ej. (String) objetoDevuelto) para restaurar la semántica original del dato.

Al definir este comportamiento como un método genérico vinculado a un tipo <T>, se subsanan estas dos problemáticas en tiempo de compilación. El compilador exigirá de manera automática que los dos argumentos introducidos pertenezcan al mismo tipo subyacente de T (evitando desajustes o fallos de diseño). Asimismo, el tipo devuelto adaptará su firma estática al tipo concreto provisto en la invocación, permitiendo recibir el resultado ya tipado sin necesidad de aplicar downcasting en absoluto.

Java
import java.util.Random;

public class UtilidadesSeleccion {
    
    // Método definido sin genéricos (Propenso a fallos y exige casting)
    public Object seleccionaUnoInseguro(Object a, Object b) {
        return new Random().nextBoolean() ? a : b;
    }

    // Método definido con genéricos (Seguro y fuerte en tipado)
    public <T> T seleccionaUnoSeguro(T a, T b) {
        return new Random().nextBoolean() ? a : b;
    }

    public void demostracion() {
        // (i) Evitar downcasting y (ii) forzar igualdad de tipos:
        
        // Con el método seguro, el compilador infiere que T es String.
        // No es necesario castear a (String) el resultado.
        String ganadorSeguro = seleccionaUnoSeguro("Coche", "Moto"); 
        
        // El compilador lanza un error en la siguiente línea impidiendo su compilación,
        // garantizando que ambos parámetros pertenezcan al mismo tipo concreto.
        // Integer falloCompilacion = seleccionaUnoSeguro(10, "Texto"); 
        
        // Por el contrario, el método inseguro compila pero es deficiente:
        String ganadorInseguro = (String) seleccionaUnoInseguro("Coche", "Moto"); // Exige cast
        Object mezclaAceptada = seleccionaUnoInseguro("Coche", 20); // Compila erróneamente el desajuste lógico
    }
}

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta
Efectivamente, es posible imponer límites a los parámetros de tipo para evitar que puedan ser sustituidos por cualquier clase del sistema. Esta característica se denomina "parámetros de tipo acotados" (bounded type parameters) y en Java se implementa empleando la palabra clave extends en la definición genérica (por ejemplo, <T extends Number>). Con ello se indica al compilador que el tipo T será desconocido, pero con la garantía absoluta de que será un descendiente o implementador del límite establecido, permitiendo operar con los métodos de dicha clase base.

A la hora de definir una abstracción numérica bidimensional, se podría implementar una primera solución puramente orientada a objetos polimórficos, sin genéricos, donde los atributos x e y se declaren explícitamente de la clase abstracta Number. Esto permite alojar reales o enteros con libertad y efectuar cálculos de distancias basándose en las abstracciones, pero pierde toda trazabilidad estricta del tipo introducido.

Por el contrario, la segunda solución involucra el uso de un parámetro genérico limitado <T extends Number>. Esta estrategia mantiene las mismas propiedades polimórficas numéricas, pero refuerza de forma estricta el chequeo de tipos interno de la clase en el propio momento de la compilación, unificando la naturaleza general que compone al objeto. Respecto al proceso de "Type Erasure", como en este caso el parámetro genérico T cuenta con un límite declarado explícitamente (Number), el compilador reemplaza todas las instancias de T por Number en el código compilado resultante, en lugar del convencional Object.

Java
// Solución 1: Sin genéricos, polimorfismo puro con Number
public class PuntoBasico {
    private Number x;
    private Number y;

    public PuntoBasico(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() { return x; }
    public Number getY() { return y; }

    public double calcularDistanciaA(PuntoBasico otro) {
        double dx = this.x.doubleValue() - otro.getX().doubleValue();
        double dy = this.y.doubleValue() - otro.getY().doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}

// Solución 2: Con Genéricos acotados reforzando el chequeo (T extends Number)
public class Punto<T extends Number> {
    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() { return x; }
    public T getY() { return y; }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = this.x.doubleValue() - otro.getX().doubleValue();
        double dy = this.y.doubleValue() - otro.getY().doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta
La principal distinción teórica y práctica entre ambas soluciones reside en el nivel de tolerancia en la mezcla de datos internos. En la implementación basada únicamente en el tipo polimórfico abstracto Number, no existe ninguna vinculación lógica entre la coordenada x y la coordenada y. Por tanto, esta solución permisiva autoriza sin problema instanciar un punto cuyas coordenadas sean un conjunto cruzado de tipos distintos, como construir un punto con una componente entera (Integer) y la otra real (Double).

Al introducir genéricos de la forma Punto<T extends Number>, se establece un pacto de homogeneidad interna inquebrantable. El parámetro T se evalúa e infiere una única vez para toda la clase durante el momento de su instanciación. Debido a esto, si se declara y reserva la variable como un Punto<Integer>, el compilador bloqueará radicalmente cualquier intento de inyectar un valor real en la segunda componente. Ambas coordenadas estarán obligadas a ser, exactamente y de manera solidaria, del mismo tipo preciso especificado, incrementando notablemente la fiabilidad estructural de la información contenida.

Respecto a los métodos de extracción como getX(), las implicaciones son análogas y definitorias. La variante sin genéricos siempre devuelve la firma abstracta superior Number, por lo que se ignora estáticamente cuál es su forma original, obligando a emplear downcasting para operar con capacidades exclusivas de los hijos. Por el contrario, la variante genérica devuelve como valor de retorno con total precisión el tipo estricto particular (el tipo inferido para T). Si se creó un Punto<Double>, getX() regresará directamente con la garantía y firma plena de ser un tipo Double a ojos del compilador.

## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### Respuesta
Para que una interfaz permita exigir a sus subclases que reciban parámetros adaptados a su propia naturaleza y evitar la sobrecarga insegura originada por un tipo base genérico, la interfaz misma debe declarar un parámetro de tipo. Dicho parámetro simbolizará a la clase hija particular y se pasará de forma referencial a sí mismo (conocido como self-referential generic type o F-bound polymorphism) al momento de implementar el contrato.

Al modificar la interfaz Punto transformándola en Punto<T>, se altera la firma impuesta al método que calcula la distancia de la forma distanciaA(Punto p) hacia un método mucho más robusto de la forma distanciaA(T p). El programador entonces estipula al definir la clase Punto2D que esta está obligada a implementar el contrato Punto<Punto2D>. Esto automáticamente obliga a sobrescribir el método distanciaA recibiendo exclusivamente otra referencia segura de tipo Punto2D.

El resultado directo de este refinamiento es la erradicación total del polimorfismo dinámico difuso dentro de la lógica del método. Al garantizar que el argumento recibido ya representa sin ambigüedades la clase concreta que nos concierne, todo requerimiento del operador instanceof es superfluo. Asimismo, cualquier intento de efectuar una transformación estática (downcasting) manual queda excluido, asegurando así que un punto tridimensional no podrá de ninguna manera colisionar ni compilar bajo interacciones de distancias bidimensionales, dejando la clase limpia y robusta frente a errores.

Java
// Se añade un parámetro de tipo T para vincularlo a la clase concreta
public interface Punto<T> { 
    public double distanciaA(T p); 
} 

// Punto2D implementa el contrato asociando el tipo T explícitamente a su propia clase Punto2D
public class Punto2D implements Punto<Punto2D> { 
    private final double x, y; 
    
    public Punto2D(double x, double y) { 
        this.x = x; 
        this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto2D p) { 
        // El parámetro 'p' ahora está estrictamente tipado como Punto2D desde compilación.
        // Se evita cualquier validación con instanceof y su subsiguiente downcasting manual.
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2)); 
    } 
} 

// Punto3D actúa en consecuencia bajo su propia lógica y dominio dimensional
public class Punto3D implements Punto<Punto3D> { 
    private final double x, y, z;
    
    public Punto3D(double x, double y, double z) {
        this.x = x; this.y = y; this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2) + Math.pow(z - p.z, 2));
    }
}

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta
En el sistema de tipado, que String herede de Object no implica en absoluto que un contenedor genérico comparta la misma jerarquía. En Java, una estructura como List<String> no es un subtipo válido de List<Object>. Sin embargo, debido al diseño original del lenguaje previo a los genéricos, un arreglo convencional String[] sí es considerado de manera legal como un subtipo válido compatible con un Object[].

La justificación formal a esta disparidad se fundamenta en los riesgos inherentes de la asignación polimórfica en memoria modificable. Si se permite asignar un array de tipo String[] a una variable base declarada como Object[], el compilador posibilitará ingresar cualquier elemento en el arreglo (como un entero Integer amparándose en que un número es un Objeto). Al ejecutar el programa, la Máquina Virtual reconocerá que la estructura física reservada correspondía originariamente a textos, originando de manera abrupta un fallo fatal al intentar almacenar el entero (fenómeno catalogado como una ArrayStoreException). Para sellar esta vulnerabilidad desde su concepción en la compilación, los creadores de Java determinaron por diseño bloquear estas jerarquías en las listas genéricas recientes impidiendo que compilen.

Desde un nivel académico y matemático, estos patrones de compatibilidad polimórfica en estructuras definen el concepto de la varianza. Se enuncia que un tipo parametrizado posee carácter covariante si conserva exactamente el mismo esquema o dirección jerárquica que engloban sus tipos internos correspondientes (caso de los Arrays, un conjunto de Cadenas desciende de un conjunto de Objetos). Por contra, adopta un marco invariante cuando, sin importar cuál sea la herencia que presenten las variables base de sus parámetros, sus clases parametrizadas resultan estrictamente disjuntas y mutuamente incompatibles (como las clases y colecciones Genéricas clásicas). Finalmente, adquiere condición de contravariante ante el excepcional caso donde las prioridades genealógicas se invierten plenamente (una familia estructurada de padres opera sobre funciones diseñadas específicamente para sus hijos).

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta
Un wildcard o "comodín" de tipo (indicado expresamente mediante el carácter del símbolo de interrogación ?) es un recurso sintáctico de flexibilización provisto por Java que se emplea en las instancias genéricas para representar un parámetro de familia desconocida temporalmente. Su objetivo es desactivar selectivamente la invarianza rígida explicada en la sección anterior para favorecer un uso polimórfico de colecciones como variables de métodos temporales, permitiendo encauzar y asegurar formalmente si un programa va exclusivamente a extraer o si su finalidad va explícitamente a incorporar elementos.

La instrucción de límite superior representada con el comando List<? extends T> implementa una estrategia segura de covarianza y certifica de manera categórica que cualquier familia que componga el arreglo será como mínimo de estirpe hija o heredera directa de T. Su utilidad principal se reserva en exclusiva para instancias productoras de información; es decir, estructuras destinadas a operaciones de lectura debido a que, al poder leer sin riesgo como objetos tipo base T, el sistema se protege anulando o limitando paralelamente toda la capacidad sintáctica de insertar ningún otro elemento nuevo.

Opuestamente, la instrucción que asienta un límite inferior y se articula como List<? super T> forja un enlace de tipo contravariante, estableciendo la premisa general de que el contenido pertenece como mínimo a la estirpe genérica T, y pudiendo ser incluso una superclase padre superior. Se reserva íntegramente como una estructura de consumición de datos, apta para operaciones masivas de agregación o incorporación a las colecciones al garantizar que estas tendrán el volumen de capacidad base para asumir sin conflicto a los pequeños hijos de T, pagando el precio estricto de bloquear en tiempo de compilación toda lectura determinista detallada de sus constituyentes de regreso.

Java
import java.util.List;

public class GestionWildcards {

    // (i) Uso de <? extends T> para LEER de forma covariante (Productor de datos)
    // Se acepta List<Integer>, List<Double>, List<Number>, etc.
    public double calcularSumaTotal(List<? extends Number> listaNumerica) {
        double totalAcumulado = 0.0;
        for (Number elemento : listaNumerica) {
            // Permite lectura estricta y segura reconociéndolo como Number.
            totalAcumulado += elemento.doubleValue(); 
        }
        // El compilador prohíbe añadir elementos aquí para proteger la memoria original.
        // listaNumerica.add(5); // PROHIBIDO: Error de compilación
        return totalAcumulado;
    }

    // (ii) Uso de <? super T> para ESCRIBIR de forma contravariante (Consumidor de datos)
    // Se acepta List<Integer>, List<Number> o List<Object>.
    public void agregarBloqueDeEnteros(List<? super Integer> listaDestino) {
        // Al garantizar que la base soporta como mínimo a un Integer, 
        // se faculta la inserción masiva segura.
        listaDestino.add(100);
        listaDestino.add(200);
        listaDestino.add(300);
        
        // El compilador impide su lectura tipada directa, ya que no se sabe si 
        // el destino era en realidad un contenedor genérico de Objetos puros.
        // Integer intentoDeLectura = listaDestino.get(0); // PROHIBIDO sin casting manual
    }
}