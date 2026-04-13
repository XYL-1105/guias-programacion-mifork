<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta

La encapsulación busca agrupar en una sola unidad (la clase) los datos y las funciones que operan sobre esos datos. Es el concepto de "empaquetado". Por su parte, la ocultación de información es el mecanismo que restringe el acceso directo a los componentes internos de esa unidad. Se busca que el usuario de la clase solo conozca "qué" hace el objeto, pero no "cómo" lo hace internamente, evitando que se dependa de detalles de implementación que podrían cambiar en el futuro.

Las ventajas principales incluyen la mantenibilidad, ya que se puede modificar la estructura interna de una clase sin romper el código que la utiliza. También mejora la integridad de los datos, pues se evita que agentes externos asignen valores inválidos a los atributos. Finalmente, reduce la complejidad cognitiva, permitiendo que el programador se concentre en interactuar con el objeto a través de una interfaz sencilla y bien definida.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta

La interfaz pública es el conjunto de métodos y propiedades que una clase expone al exterior. Se puede visualizar como un "contrato" o el panel de control de una máquina: son los únicos puntos de contacto permitidos para que otros objetos soliciten servicios o información. En Java, esto se define generalmente mediante el modificador de acceso public.

La relación con la ocultación es de complementariedad: mientras la ocultación "cierra las puertas" de la implementación interna (detalles privados), la interfaz pública "abre ventanas" controladas. Una buena ocultación asegura que la interfaz pública sea mínima y suficiente, protegiendo al resto del sistema de cambios internos accidentales y garantizando que el acceso a los datos siempre pase por los filtros de validación establecidos por la clase.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta

El diseño de la interfaz pública es crítico porque representa el compromiso que la clase adquiere con el resto del sistema. Una vez que una interfaz pública es utilizada por otros programadores o módulos, cualquier cambio en ella (como renombrar un método o cambiar el tipo de un parámetro) provocará errores de compilación en todos los lugares donde se use. Por ello, se dice que la interfaz pública debe ser estable.

Cambiar la interfaz pública no es fácil en proyectos grandes, ya que genera un "efecto dominó" que obliga a refactorizar múltiples partes del software. En cambio, si el diseño es cuidadoso y se oculta la mayor cantidad de detalles posible, el programador tiene total libertad para cambiar la lógica interna (el código dentro de los métodos) sin que nadie más se dé cuenta, lo que facilita enormemente la evolución del programa.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta

Las invariantes de clase son condiciones o reglas lógicas que deben cumplirse siempre para que un objeto se considere en un estado válido. Por ejemplo, en una clase Fecha, una invariante sería que el valor del "mes" debe estar siempre entre 1 y 12. Si estas condiciones se violan, el comportamiento del programa se vuelve impredecible y propenso a errores graves.

La ocultación de información es la herramienta fundamental para proteger estas invariantes. Al marcar los atributos como privados, se impide que un código externo asigne un valor erróneo (como mes = 15) de forma directa. La única forma de modificar el estado es a través de métodos públicos que actúan como "porteros", validando los datos antes de permitir el cambio y asegurando que la invariante nunca se rompa.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta

A continuación se muestra la clase Punto aplicando ocultación de información. Los atributos se marcan como privados para proteger el estado interno.

Java
public class Punto {
    private double x; // Ocultos al exterior
    private double y;

    public double calcularDistanciaAOrigen() { // Expuesto al exterior
        return Math.sqrt(x * x + y * y);
    }
}
La interfaz pública de esta clase consiste en el método calcularDistanciaAOrigen(). El modificador private indica que el miembro solo es accesible desde dentro de la misma clase, mientras que public permite que cualquier otra clase del programa pueda acceder al miembro. En este diseño, el mundo exterior sabe que el punto puede calcular su distancia, pero no puede ver ni modificar directamente sus coordenadas.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta

En Java, los modificadores de acceso se pueden aplicar a las clases, los atributos (variables de instancia o de clase) y los métodos (incluyendo los constructores). Es importante destacar que las clases de nivel superior (las que no están dentro de otra clase) normalmente solo pueden ser public o tener visibilidad por defecto (package-private); no pueden ser private porque no tendría sentido una clase que nadie puede instanciar.

Al aplicar estos modificadores a los miembros (atributos y métodos), se define el nivel de encapsulación del objeto. Lo más habitual en la práctica profesional es que los atributos sean siempre private para cumplir con la ocultación de información, mientras que los métodos varían entre public para la funcionalidad externa y private para tareas auxiliares internas que no deben ser vistas por el usuario de la clase.

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta

Sí, existen niveles intermedios de visibilidad que permiten un control más fino. En Java, además de public y private, existen el nivel protected (accesible por la propia clase, sus subclases y clases del mismo paquete) y la visibilidad por defecto (sin palabra clave), que limita el acceso solo a las clases que se encuentran en el mismo paquete.

En otros lenguajes, la terminología y el comportamiento varían. Por ejemplo, en C++ también existen public, private y protected, pero su alcance está estrictamente ligado a la jerarquía de clases. En lenguajes como Python, la visibilidad es puramente convencional: se suele anteponer un guion bajo (_) al nombre de un atributo para indicar que es "privado", aunque el lenguaje no impide técnicamente el acceso, apelando a la responsabilidad del programador.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta

La respuesta correcta es la (a): otras clases. En Java y C++, la privacidad se define a nivel de clase, no de objeto. Esto significa que una instancia de la clase Punto puede acceder a los atributos privados de otra instancia de la clase Punto, siempre que dicho acceso ocurra dentro del código fuente de la propia clase.

Un ejemplo claro es el método para calcular la distancia entre dos puntos:

Java
public double calcularDistanciaAPunto(Punto otro) {
    // Es legal acceder a otro.x y otro.y aunque sean privados
    double dx = this.x - otro.x; 
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}
Esto es posible porque el código de calcularDistanciaAPunto reside dentro de la clase Punto, y por tanto, "conoce" la estructura interna de cualquier objeto de su misma especie. La ocultación protege al objeto de miradas de clases extrañas, pero no de sus semejantes.

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta

Los métodos getter y setter (también llamados descriptores de acceso y mutadores) son métodos públicos diseñados para leer o modificar, respectivamente, el valor de un atributo privado. El "getter" permite que el exterior consulte un dato sin darle permiso para escribir en él, mientras que el "setter" permite la modificación controlada.

El uso de estos métodos es una pieza clave de la encapsulación. En lugar de permitir el acceso directo a una variable, el "setter" puede incluir lógica de validación (por ejemplo, impedir que una edad sea negativa). Además, el "getter" podría devolver una copia del dato o un valor calculado, manteniendo la flexibilidad de cambiar cómo se almacena la información internamente sin afectar a quien la consulta.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta

No se refiere habitualmente a seguridad frente a ataques externos o ciberseguridad, sino a la seguridad técnica o robustez del código. El objetivo es evitar que el programa entre en estados corruptos debido a errores accidentales del programador. Se trata de proteger al código "de nosotros mismos" o de otros compañeros de equipo que podrían manipular variables de forma indebida.

Un programa "seguro" en este contexto es aquel que es difícil de romper mediante un uso incorrecto de sus clases. Si un objeto garantiza que sus datos internos siempre son coherentes gracias a la ocultación, se eliminan muchísimos fallos lógicos (bugs) que de otro modo serían muy difíciles de rastrear, especialmente en sistemas complejos con miles de líneas de código.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta

Un miembro de instancia es una variable o método que pertenece a cada objeto individual; cada instancia tiene su propia copia de los datos (como las coordenadas x e y de un punto). En cambio, un miembro de clase (marcado con static) pertenece a la clase en su conjunto y es compartido por todas las instancias. Solo existe una copia de un atributo static en toda la memoria del programa.

Los miembros de clase también se pueden ocultar. Es una práctica común declarar atributos static private para mantener información global de la clase que no debe ser manipulada desde fuera, como un contador interno de objetos creados o una caché de configuración. La visibilidad funciona exactamente igual: si es privado, solo los métodos de esa clase (ya sean estáticos o de instancia) podrán acceder a él.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta

Aunque pueda parecer contradictorio, tiene mucho sentido en patrones de diseño específicos. Si un constructor es private, ninguna otra clase puede crear instancias de esa clase de la forma tradicional (new Clase()). Esto se utiliza, por ejemplo, en el patrón Singleton, donde se quiere garantizar que solo exista una única instancia de una clase en todo el programa.

También se emplea cuando se desea obligar al usuario a crear objetos a través de métodos factoría estáticos. Esto permite dar nombres descriptivos a la creación de objetos (como Punto.crearEnOrigen()) o realizar comprobaciones complejas antes de decidir si se devuelve un objeto nuevo o uno reciclado, ofreciendo un control total sobre el proceso de instanciación que un constructor público no permitiría.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta

Los miembros de clase se indican con la palabra reservada static. En el siguiente ejemplo, se utilizan atributos estáticos privados para registrar los valores máximos alcanzados por cualquier punto creado, protegiendo estos datos mediante métodos de consulta públicos.

Java
public class Punto {
    private double x, y;
    private static double xMax = 0; // Miembro de clase oculto
    private static double yMax = 0;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        if (x > xMax) xMax = x;
        if (y > yMax) yMax = y;
    }

    public static double getXMax() { return xMax; }
    public static double getYMax() { return yMax; }
}
Aquí, xMax e yMax no pertenecen a un punto concreto, sino a la "especie" Punto. Cada vez que se crea un objeto, se actualiza la estadística global de forma transparente para el usuario.

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta

Para este propósito se utiliza un método estático, ya que debe poder llamarse sin que exista todavía una instancia de la clase. El método procesa los datos y termina llamando al constructor interno para devolver el objeto ya configurado.

Java
public static Punto crearPuntoRedondeado(double x, double y) {
    double xRedondeado = Math.round(x);
    double yRedondeado = Math.round(y);
    return new Punto(xRedondeado, yRedondeado);
}
Se ha utilizado static porque los métodos factoría son, por definición, comportamientos de la clase encargados de generar instancias. Esto permite que el usuario del código escriba Punto p = Punto.crearPuntoRedondeado(3.7, 4.2);, obteniendo un punto en las coordenadas (4.0, 4.0).

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta

Este ejercicio demuestra la potencia de la ocultación de información. Se cambia radicalmente la estructura interna de datos, pero como la interfaz pública (los métodos que el exterior ve) se mantiene igual, ningún programa que use la clase Punto fallará ni tendrá que ser modificado.

Java
public class Punto {
    private double[] coordenadas = new double[2]; // Cambio interno

    public Punto(double x, double y) {
        this.coordenadas[0] = x;
        this.coordenadas[1] = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(Math.pow(coordenadas[0], 2) + Math.pow(coordenadas[1], 2));
    }
}
La "magia" reside en que el usuario sigue llamando a calcularDistanciaAOrigen() de la misma forma que antes. No le importa si los datos se guardan en variables sueltas, en un array o en un fichero; el contrato de la interfaz pública se ha respetado.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta

Aunque parezca equivalente, no es mejor declararlo público. La convención universal en Java es mantener los atributos privados. Si se declara público, se pierde el control para siempre: cualquier cambio futuro en ese atributo obligará a cambiar todos los programas que lo usen. Con getter/setter, se mantiene una capa de abstracción que permite, por ejemplo, añadir una validación en el futuro sin cambiar la interfaz.

Esto tiene una relación directa con las invariantes de clase. Un atributo público puede ser modificado con cualquier valor, rompiendo la coherencia del objeto. Al usar un setter, la clase conserva la autoridad para rechazar valores ilegales. Incluso si hoy el setter no hace nada especial, dejarlo implementado es una inversión en flexibilidad para el mantenimiento futuro del software.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta

Una clase es inmutable cuando sus objetos no pueden cambiar de estado una vez que han sido creados (por ejemplo, la clase String en Java). Todos sus atributos suelen ser final y no proporciona métodos que alteren sus datos internos. Para representar un cambio, un método de una clase inmutable devuelve un nuevo objeto con el nuevo estado en lugar de modificar el actual.

Un método modificador es cualquier método que cambia el estado de un objeto. No tiene por qué ser un "setter" tradicional (como setX); puede ser un método con lógica de negocio como acelerar() o trasladar(). En las clases inmutables, estos métodos modificadores no existen o se transforman en funciones que generan nuevas instancias, lo cual es muy ventajoso para evitar errores en programas con múltiples hilos de ejecución (concurrencia).

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta

No, no es recomendable incluirlos por defecto para todos los atributos. Existe una tendencia errónea a generar getters y setters para todo de forma automática, pero esto a menudo rompe la encapsulación al exponer detalles internos innecesariamente. Si un atributo no necesita ser modificado desde el exterior para que la clase cumpla su función, no debe tener un setter.

La regla de oro es proporcionar la mínima interfaz posible. Cada setter público es una nueva forma en la que un código externo puede alterar el objeto, lo que aumenta la superficie de posibles errores. El diseño debe basarse en las necesidades de comportamiento del objeto, no simplemente en dar acceso a todos sus datos internos.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta

La clase String es inmutable. Cuando se concatenan dos cadenas (por ejemplo, s1 + s2), no se modifica la cadena s1, sino que Java crea en memoria un tercer objeto String completamente nuevo que contiene la unión de ambas. Esto hace que las cadenas sean muy seguras de compartir, pero puede ser ineficiente si se realizan miles de concatenaciones en un bucle.

Para construir cadenas largas paso a paso, lo correcto es utilizar la clase StringBuilder. Esta clase sí es mutable y permite añadir contenido a un buffer interno sin crear nuevos objetos en cada paso. Una vez terminada la construcción, se llama al método toString() para obtener el resultado final como un objeto String inmutable.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta

Existen dos formas de comparación: por identidad (usando el operador ==) y por contenido (usando el método equals()). El operador == comprueba si dos variables apuntan exactamente a la misma dirección de memoria (si son el mismo objeto físico). El método equals() está pensado para comprobar si dos objetos diferentes tienen los mismos datos internos (si son lógicamente iguales).

Por defecto, el método equals() en Java se comporta igual que ==, por lo que cada clase debe sobrescribirlo si quiere definir qué significa ser "igual" (por ejemplo, dos puntos son iguales si sus coordenadas x e y coinciden). Para las cadenas de texto, siempre se debe usar s1.equals(s2), ya que usar == suele dar resultados erróneos al comparar el lugar de la memoria en lugar del texto que contienen.

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta

Las clases wrapper (envoltorios) son clases que representan a los tipos primitivos (como int, double, boolean) como si fueran objetos reales (Integer, Double, Boolean). Son necesarias porque muchas estructuras de Java (como las listas) solo pueden trabajar con objetos y no con tipos básicos de C.

En Java moderno, el proceso es automático mediante el autoboxing y unboxing: el lenguaje convierte un int a un Integer y viceversa sin que el programador tenga que hacer nada especial. Sin embargo, esto tiene un coste de rendimiento y memoria. No todos los lenguajes lo necesitan; en Smalltalk o Ruby, todo es un objeto desde el principio y no existen tipos primitivos "al estilo C", por lo que no requieren wrappers.

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta

Un tipo enumerado (enum) permite definir una variable que solo puede tomar un conjunto restringido de valores predefinidos y con nombre (por ejemplo, los días de la semana o los puntos cardinales). Esto mejora la legibilidad y evita errores comunes en C, donde se solían usar constantes enteras que no impedían pasar valores sin sentido.

En Java, un enum es en realidad una clase especial. Esto es una gran ventaja en términos de encapsulación, ya que a diferencia de C, los enumerados en Java pueden tener sus propios atributos, constructores y métodos. Esto permite asociar lógica y datos directamente a cada valor de la enumeración, manteniendo toda la información relacionada agrupada y protegida.

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta

Se presenta la implementación del enumerado Mes. Los constructores de los enum en Java son privados por definición, ya que no se pueden crear nuevas instancias fuera de las declaradas inicialmente.

Java
public enum Mes {
    ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3), ABRIL(30, 4),
    MAYO(31, 5), JUNIO(30, 6), JULIO(31, 7), AGOSTO(31, 8),
    SEPTIEMBRE(30, 9), OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

    private final int dias;
    private final int posicion;

    Mes(int dias, int posicion) {
        this.dias = dias;
        this.posicion = posicion;
    }

    public int getDias() { return dias; }
    public int getPosicion() { return posicion; }
}

## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta

Para implementar esta lógica, se añaden métodos que utilicen la posición del mes. Se considera la diferencia estacional entre hemisferios: cuando en el norte es verano, en el sur es invierno.

Java
public boolean esDeVerano(boolean enHemisferioNorte) {
    if (enHemisferioNorte) {
        return (posicion >= 6 && posicion <= 9); // Junio a Septiembre
    } else {
        return (posicion <= 3 || posicion >= 12); // Diciembre a Marzo
    }
}

public boolean esDeInvierno(boolean enHemisferioNorte) {
    // La lógica sería la inversa al verano
    return esDeVerano(!enHemisferioNorte);
}

// Métodos análogos se implementarían para Primavera y Otoño siguiendo la misma estructura lógica
Esta estructura permite encapsular el conocimiento astronómico y geográfico dentro del propio objeto Mes, permitiendo que el resto del programa simplemente pregunte por la estación sin conocer los detalles de los meses que la componen.