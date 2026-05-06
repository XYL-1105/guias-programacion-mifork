<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta

En el lenguaje C, la composición se logra mediante el anidamiento de estructuras. Se define una relación de "tiene-un" cuando un struct incluye como miembro otra instancia de un struct previamente definido. En este caso, la estructura Punto actúa como el bloque de construcción básico, proporcionando las coordenadas necesarias para que la estructura Linea pueda existir como una entidad de orden superior.

Para calcular la distancia entre puntos y la longitud de la línea, se hace uso de la biblioteca matemática estándar. La lógica se basa en el teorema de Pitágoras, tratando la diferencia de coordenadas como los catetos de un triángulo rectángulo. A continuación se presenta la implementación técnica:

C
#include <math.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto p1;
    Punto p2;
} Linea;

double calcularDistancia(Punto a, Punto b) {
    return sqrt(pow(b.x - a.x, 2) + pow(b.y - a.y, 2));
}

double calcularLongitud(Linea l) {
    // Se reutiliza la función de distancia pasando los puntos de la línea
    return calcularDistancia(l.p1, l.p2);
}

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta

La transición a Java permite encapsular tanto los datos como el comportamiento. Mediante el uso de modificadores de acceso (private) y la palabra reservada final, se garantiza la inmutabilidad de los objetos. Esto significa que, una vez instanciado un Punto o una Linea, sus valores internos no pueden ser alterados, superando la vulnerabilidad de los struct de C donde cualquier parte del programa podría modificar los campos si tiene acceso a la variable.

En este modelo, la clase Linea posee dos atributos de tipo Punto. La lógica de cálculo se integra como métodos de instancia, lo que permite una sintaxis más natural. La inmutabilidad es clave: al no proporcionar métodos "setter" y marcar los atributos como finales, se asegura que la relación de composición sea constante durante toda la vida del objeto.

Java
public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        return Math.sqrt(Math.pow(otro.x - this.x, 2) + Math.pow(otro.y - this.y, 2));
    }
}

public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double obtenerLongitud() {
        return p1.distanciaA(p2);
    }
}

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta

La multiplicidad define cuántas instancias de una clase pueden estar asociadas con una instancia de otra clase en una relación determinada. Es un concepto fundamental para entender los límites y las reglas de negocio de la estructura de datos. Se suele representar con rangos numéricos (por ejemplo, 1, 0..*, 1..n) y ayuda a determinar si una relación es obligatoria u opcional.

En el ejemplo proporcionado, la multiplicidad se analiza en ambos sentidos:

De Linea a Punto: La multiplicidad es 2. Una línea está compuesta exactamente por dos puntos (ni más, ni menos, según la definición del ejercicio).

De Punto a Linea: La multiplicidad es 0..* (o de cero a muchos). Un punto puede existir de forma independiente sin pertenecer a ninguna línea, o puede ser el extremo de una o múltiples líneas distintas.

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta

La distinción entre composición fuerte y débil radica en la dependencia del ciclo de vida de los objetos involucrados. En la composición fuerte (referida simplemente como composición), el objeto contenido no tiene sentido de existencia sin su contenedor. Si el objeto "Padre" se destruye, los objetos "Hijos" deben ser destruidos también, implicando una propiedad total y exclusiva.

Por el contrario, en la composición débil (técnicamente denominada agregación o asociación según el contexto), el ciclo de vida de los objetos es independiente. Los objetos contenidos pueden existir antes que el contenedor y seguir existiendo después de que este desaparezca. Es una relación de "pertenencia" pero no de "existencia vital".

En resumen, se utiliza el término "composición" para relaciones donde hay una coincidencia en el ciclo de vida y una propiedad fuerte. El término "agregación" se reserva para casos donde el contenedor solo mantiene una referencia a objetos que tienen su propia vida independiente fuera de dicha relación.

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta

Cuando una clase interactúa con otra de forma temporal, se habla de dependencia. A diferencia de la composición, donde una clase "tiene un" objeto como parte de sus atributos permanentes, la dependencia ocurre cuando una clase "usa un" objeto durante la ejecución de una tarea específica. Es la relación más débil y transitoria entre clases.

Se identifica una dependencia cuando la clase externa aparece únicamente en la firma de un método (parámetros o tipo de retorno) o como una variable local dentro de un bloque de código. Al no almacenarse como un atributo (campo) de la clase, la relación termina en cuanto el método finaliza su ejecución. Por tanto, no hay una estructura de datos persistente que una a ambos objetos.

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta

Para implementar una composición fuerte en Java, el objeto contenedor suele encargarse de la instanciación de sus partes. En el ejemplo de la línea, la clase LineaFuerte crea sus propios puntos internamente. De este modo, nadie más tiene acceso a esos puntos específicos y su vida está ligada exclusivamente a la de la línea.

En la composición débil (agregación), los puntos se reciben ya creados desde el exterior. La línea simplemente almacena una referencia a ellos. Si la línea se descarta, los puntos originales siguen siendo accesibles por quien los creó inicialmente, demostrando la independencia de sus ciclos de vida.

Java
// Composición Fuerte: La línea crea y posee sus puntos
public class LineaFuerte {
    private Punto p1;
    private Punto p2;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }
}

// Composición Débil (Agregación): La línea recibe puntos externos
public class LineaDebil {
    private Punto p1;
    private Punto p2;

    public LineaDebil(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }
}

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta

A diferencia de C++, donde el programador debe liberar la memoria manualmente (delete), en Java no existe una destrucción explícita de objetos por parte del usuario. El sistema cuenta con un mecanismo llamado Garbage Collector (Recolector de Basura). Este proceso identifica automáticamente los objetos que ya no son alcanzables por ninguna parte del programa y libera su memoria.

En la composición fuerte, cuando el objeto Linea deja de ser referenciado y es marcado para su eliminación, las referencias internas a los objetos Punto también desaparecen. Si esos puntos no están siendo usados por nadie más (lo cual es propio de la composición fuerte), quedan huérfanos de referencias. En el siguiente ciclo de ejecución del Garbage Collector, estos puntos serán destruidos junto con la línea de forma automática y transparente.

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta

En este diseño, se gestiona una lista de profesores y una referencia específica para el director. La integridad del sistema se mantiene mediante invariantes de clase: el director siempre debe estar presente en la lista general. Para ello, el constructor y los métodos de modificación validan las reglas de negocio lanzando excepciones si se intentan realizar operaciones prohibidas, como eliminar al profesor que ostenta el cargo de director.

Se oculta la implementación del array para proteger la encapsulación. El mundo exterior no sabe que se usa un array de tamaño 50; solo interactúa con métodos que permiten añadir, eliminar o consultar de forma controlada.

Java
public class Departamento {
    private Profesor[] profesores = new Profesor[50];
    private int numProfesores = 0;
    private Profesor director;

    public Departamento(Profesor directorActual) {
        if (directorActual == null) throw new IllegalArgumentException("Debe haber un director");
        this.director = directorActual;
        this.añadirProfesor(directorActual);
    }

    public void añadirProfesor(Profesor p) {
        if (numProfesores >= 50) throw new RuntimeException("Departamento lleno");
        profesores[numProfesores++] = p;
    }

    public void eliminarProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) throw new IndexOutOfBoundsException();
        if (profesores[pos] == director) throw new IllegalStateException("No se puede eliminar al director");
        
        // Desplazamiento para no dejar huecos
        for (int i = pos; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i+1];
        }
        profesores[--numProfesores] = null;
    }

    public void setDirector(Profesor nuevoDirector) {
        boolean encontrado = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevoDirector) {
                encontrado = true;
                break;
            }
        }
        if (!encontrado) throw new IllegalArgumentException("El director debe ser del departamento");
        this.director = nuevoDirector;
    }

    public int getTotal() { return numProfesores; }
    public Profesor getProfesor(int pos) { return profesores[pos]; }
}


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta

l sustituir el array primitivo por una interfaz List (como ArrayList), se elimina la necesidad de gestionar manualmente el tamaño, los desplazamientos al eliminar elementos y el contador de posiciones ocupadas. Métodos como add() y remove() simplifican drásticamente el código, reduciendo la posibilidad de errores de índice y permitiendo un crecimiento dinámico de la colección.

El problema de devolver la lista interna directamente (por ejemplo, return listaInterna;) es que se rompe la encapsulación. El cliente recibiría una referencia al objeto real y podría añadir o borrar profesores sin pasar por las validaciones de la clase Departamento (como la regla del director). Para resolverlo, se debe devolver una copia de la lista o, preferiblemente, una vista no modificable usando Collections.unmodifiableList(listaInterna).

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

La composición recursiva ocurre cuando una clase tiene un atributo de su propio tipo. Es una estructura potente para representar jerarquías o secuencias. En el caso de una Persona, cada instancia guarda una referencia a otra instancia que representa a su progenitora, permitiendo navegar por el árbol genealógico mediante el encadenamiento de objetos.

Además de las genealogías y las excepciones (donde una excepción envuelve a su causa), existen otros ejemplos clásicos como las estructuras de archivos (una carpeta contiene otras carpetas), los nodos de una lista enlazada o los árboles de decisión, donde cada nodo puede tener hijos que son, a su vez, nodos.

Java
public class Persona {
    private final String nombre;
    private final Persona madre; // Composición recursiva

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    public static void main(String[] args) {
        Persona abuela = new Persona("Ana", null);
        Persona madre = new Persona("Beatriz", abuela);
        Persona nieto = new Persona("Carlos", madre);
    }
}

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

Una relación es bidireccional cuando ambos objetos implicados mantienen una referencia mutua. Mientras que en una relación unidireccional el Departamento conoce a sus Profesor, en una bidireccional el Profesor también tiene un atributo que apunta al Departamento al que pertenece. Esto facilita la navegación en ambos sentidos del modelo de datos.

Para implementar esto en el ejemplo anterior, la clase Profesor debería incluir un atributo private Departamento departamento. El desafío técnico reside en mantener la consistencia: al añadir un profesor a un departamento, el código debe asegurar automáticamente que el atributo departamento del profesor se actualice para apuntar a ese contenedor. Esto suele requerir métodos de ayuda que gestionen ambos lados de la relación simultáneamente para evitar datos contradictorios.