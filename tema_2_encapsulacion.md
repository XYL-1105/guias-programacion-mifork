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
La encapsulación y la ocultación de información en POO buscan separar claramente qué hace un objeto de cómo lo hace. Se pretende que los detalles internos de implementación queden protegidos y sólo se exponga al exterior aquello que es necesario para usar el objeto correctamente. De este modo, otros objetos interactúan con él a través de una interfaz controlada, sin depender de sus detalles internos.

La ocultación de información aporta varias ventajas: evita el uso indebido de los datos internos, facilita el mantenimiento del código, permite modificar la implementación sin afectar a quien usa la clase y ayuda a preservar la coherencia interna del objeto. Además, reduce el acoplamiento entre clases, lo que hace el sistema más robusto y flexible ante cambios.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta
La interfaz pública de una clase es el conjunto de métodos y atributos accesibles desde fuera de la clase. Es el “contrato” que la clase ofrece a otras partes del programa para poder interactuar con sus objetos. Define qué operaciones se pueden realizar sobre el objeto sin revelar cómo se implementan internamente.

Esta interfaz está directamente relacionada con la ocultación de información, ya que todo aquello que no forma parte de la interfaz pública permanece oculto. Así, se controla estrictamente el acceso al estado interno del objeto y se obliga a utilizar únicamente las operaciones previstas por el diseñador de la clase.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta
La interfaz pública de una clase debe diseñarse con cuidado porque es la parte que otros componentes del programa utilizarán. Una mala decisión en su diseño puede provocar que otras clases dependan de detalles que no deberían conocer o que el uso de la clase sea confuso o poco seguro.

Cambiar la interfaz pública no es sencillo, ya que puede obligar a modificar todas las partes del programa que la utilizan. Por ello, se considera que la interfaz pública debe ser estable y bien pensada desde el principio.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta
Las invariantes de clase son condiciones que siempre deben cumplirse para que el objeto esté en un estado válido. Por ejemplo, que un valor no pueda ser negativo o que dos atributos mantengan una relación concreta entre sí. Estas condiciones garantizan la coherencia interna del objeto durante toda su vida.

La ocultación de información ayuda a mantener estas invariantes porque impide que otras clases modifiquen directamente los atributos. Sólo los métodos de la propia clase pueden alterar el estado, asegurando que las invariantes se comprueben y se respeten en todo momento.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta
public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

La interfaz pública de la clase Punto está formada por su constructor y el método calcularDistanciaAOrigen. Los atributos x e y están ocultos. El modificador public indica que el elemento es accesible desde cualquier clase, mientras que private indica que sólo es accesible desde dentro de la propia clase.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta
En Java, los modificadores public y private pueden aplicarse a clases, atributos, métodos y constructores. Permiten controlar quién puede acceder a cada uno de estos elementos desde fuera de la clase.

Gracias a estos modificadores, se puede definir con precisión qué partes de la clase forman su interfaz pública y cuáles deben permanecer ocultas para proteger su estado interno.

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta
Además de la visibilidad pública y privada, existen otros niveles de visibilidad. En muchos lenguajes orientados a objetos aparece la visibilidad protegida, que permite el acceso desde clases derivadas, y visibilidades intermedias relacionadas con paquetes o módulos.

En Java, existen cuatro niveles: public, protected, visibilidad por defecto (package-private) y private. Otros lenguajes, como C++, también incluyen niveles similares, aunque con ligeras variaciones en su comportamiento.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta
Los miembros de instancia privados están ocultos para otras clases, pero no para otras instancias de la misma clase. Esto se debe a que el código que accede a esos miembros está dentro de la propia clase, que sí tiene permiso para acceder a los atributos privados de cualquier instancia.

public double calcularDistanciaAPunto(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}

En este ejemplo, aunque x e y son privados, se pueden usar sobre el objeto otro porque el método pertenece a la misma clase Punto.

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta
Los métodos getter y setter son métodos públicos que permiten leer y modificar, respectivamente, el valor de atributos privados. Son un mecanismo habitual para mantener la ocultación de información mientras se permite el acceso controlado a los datos.

Gracias a ellos, se puede añadir lógica adicional al acceso o modificación de los atributos, como validaciones, sin exponer directamente los datos internos.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta
Cuando se habla de que la ocultación de información mejora la “seguridad” del programa, no se refiere a la protección frente a ataques externos o hackers. Se refiere a la seguridad interna del diseño del programa.

Esto implica evitar usos incorrectos de los objetos, proteger su estado interno y garantizar que sólo se pueda modificar de formas previstas y controladas.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta
Un miembro de instancia pertenece a cada objeto concreto, mientras que un miembro de clase es compartido por todos los objetos de esa clase. En Java, estos últimos se indican con la palabra clave static.

Los miembros de clase también pueden declararse como private o public, por lo que igualmente pueden beneficiarse de la ocultación de información.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta
Tiene sentido que un constructor sea privado cuando se quiere controlar cómo se crean los objetos. Esto ocurre, por ejemplo, en el patrón Singleton o cuando se desea forzar el uso de métodos factoría.

De esta forma, se impide la creación directa de objetos y se obliga a utilizar métodos específicos que pueden aplicar ciertas reglas antes de construir la instancia.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta
public class Punto {
    private double x;
    private double y;

    private static double maxX = Double.MIN_VALUE;
    private static double maxY = Double.MIN_VALUE;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public static double getMaxX() {
        return maxX;
    }

    public static double getMaxY() {
        return maxY;
    }
}

Los miembros de clase se indican con static y permiten almacenar información común a todos los objetos creados.

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta
public static Punto crearPuntoRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}

Sí, se ha utilizado static, ya que el método no depende de ninguna instancia concreta.

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta
public class Punto {
    private double[] coord = new double[2];

    public Punto(double x, double y) {
        coord[0] = x;
        coord[1] = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coord[0] * coord[0] + coord[1] * coord[1]);
    }
}

Se ha cambiado la implementación interna sin modificar la interfaz pública, demostrando la utilidad de la encapsulación.

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta

Aunque un atributo tenga getter y setter públicos, no es recomendable declararlo público. Mantenerlo privado permite cambiar su implementación interna en el futuro sin afectar al resto del código.

La convención más habitual es que los atributos sean privados. Esto está directamente relacionado con las invariantes de clase, ya que permite controlar y validar cualquier modificación del estado interno.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta
Una clase es inmutable cuando, una vez creado el objeto, su estado no puede cambiar. Un método modificador es aquel que altera el estado del objeto, y no siempre coincide exactamente con un setter.

Las clases inmutables tienen ventajas como mayor seguridad, facilidad de uso en entornos concurrentes y menor posibilidad de errores relacionados con cambios inesperados de estado.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta
No es recomendable incluir setters siempre por convención. Exponer setters puede permitir modificaciones que no tienen sentido para el objeto y romper sus invariantes.

Se deben incluir únicamente cuando tenga sentido que el estado del objeto pueda cambiar de esa forma controlada.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta
La clase String en Java es inmutable. Al concatenar dos cadenas, se crea un nuevo objeto String en lugar de modificar el existente.

Si se van a realizar muchas concatenaciones para construir una cadena larga, se recomienda utilizar StringBuilder, que es mutable y más eficiente en ese caso.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta
En POO, los objetos pueden compararse por identidad o por contenido. La identidad comprueba si son exactamente el mismo objeto en memoria, mientras que el contenido compara sus valores internos.

En Java, el método equals permite comparar el contenido. Por defecto, compara la identidad. Para comparar cadenas, se debe usar equals, no el operador ==.

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta
Las clases wrapper envuelven tipos primitivos dentro de objetos, como Integer para int. Este proceso puede hacerse automáticamente en Java mediante autoboxing.

Permiten tratar valores primitivos como objetos, lo que facilita su uso en colecciones y métodos genéricos. No todos los lenguajes orientados a objetos necesitan wrappers, ya que algunos no distinguen entre primitivos y objetos.

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta
Un tipo enumerado es un conjunto finito y fijo de valores posibles. En Java, un enumerado es una clase especial que define instancias predefinidas.

Los enumerados permiten encapsular comportamiento y datos asociados a cada valor, mejorando la claridad y la seguridad del diseño.

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta
public enum Mes {
    ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3), ABRIL(30, 4),
    MAYO(31, 5), JUNIO(30, 6), JULIO(31, 7), AGOSTO(31, 8),
    SEPTIEMBRE(30, 9), OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

    private int dias;
    private int ordinal;

    private Mes(int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }

    public int getDias() {
        return dias;
    }

    public int getOrdinal() {
        return ordinal;
    }
}



## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta

public boolean esDePrimavera(boolean esHemisferioNorte) {
    int m = this.ordinal;
    if (esHemisferioNorte) {
        return m >= 3 && m <= 5;
    } else {
        return m >= 9 && m <= 11;
    }
}

public boolean esDeVerano(boolean esHemisferioNorte) {
    int m = this.ordinal;
    if (esHemisferioNorte) {
        return m >= 6 && m <= 8;
    } else {
        return m == 12 || m <= 2;
    }
}

public boolean esDeOtoño(boolean esHemisferioNorte) {
    int m = this.ordinal;
    if (esHemisferioNorte) {
        return m >= 9 && m <= 11;
    } else {
        return m >= 3 && m <= 5;
    }
}

public boolean esDeInvierno(boolean esHemisferioNorte) {
    int m = this.ordinal;
    if (esHemisferioNorte) {
        return m == 12 || m <= 2;
    } else {
        return m >= 6 && m <= 8;
    }
}
