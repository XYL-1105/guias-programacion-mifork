<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta

En el lenguaje C, al no existir un mecanismo nativo de excepciones, el control de errores recae en el diseño de la firma de la función. El objetivo es comunicar al llamador que algo ha fallado sin detener la ejecución de forma abrupta. Existen principalmente dos estrategias para lograr esto: el uso de valores de retorno especiales o el uso de parámetros de salida (punteros).

La primera opción consiste en reservar un valor del dominio de la función para indicar error. Si una raíz cuadrada devuelve un float, y sabemos que el resultado siempre será positivo, se puede devolver -1.0 para indicar una entrada inválida. La segunda opción es devolver un código de estado (como un int o un enum) y entregar el resultado real a través de un puntero pasado por argumento.

C
// Opción 1: Valor de retorno especial
float raiz_opcion1(float n) {
    if (n < 0) return -1.0; // Valor "centinela" para error
    return sqrt(n);
}

// Opción 2: Código de estado y puntero (estilo sistema operativo)
int raiz_opcion2(float n, float *resultado) {
    if (n < 0) return -1; // Error: código negativo
    *resultado = sqrt(n);
    return 0; // Éxito
}

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta

Una excepción es un evento que ocurre durante la ejecución de un programa y que interrumpe el flujo normal de las instrucciones. Se trata de un mecanismo estructurado para señalar que se ha producido una situación anómala (un error, una falta de recursos, una entrada inválida) que el código actual no sabe o no debe resolver. Es una forma de decir: "ha ocurrido algo inesperado, que alguien con más contexto se ocupe de ello".

El objetivo principal de un programador al usar excepciones es separar la lógica de negocio (el "camino feliz" del código) de la lógica de gestión de errores. Al delegar la responsabilidad del error a un nivel superior, se evita ensuciar las funciones con constantes comprobaciones de if (error), logrando un código más limpio, legible y robusto.

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta

En Java, la gestión se realiza mediante el lanzamiento de un objeto que representa el error. El método no necesita reservar valores especiales de retorno; simplemente "dispara" la excepción si las condiciones no se cumplen. El flujo de control salta inmediatamente fuera del método hacia el bloque de captura más cercano.

En el siguiente ejemplo, se observa cómo la clase Calculadora se encarga de la lógica matemática, mientras que el main asume la responsabilidad de interactuar con el usuario y gestionar el posible fallo mediante una estructura try-catch.

Java
public class Calculadora {
    public double raiz(double n) {
        if (n < 0) {
            throw new IllegalArgumentException("No se puede calcular la raíz de un negativo");
        }
        return Math.sqrt(n);
    }
}

public class Test {
    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        try {
            System.out.println(calc.raiz(-5));
        } catch (IllegalArgumentException e) {
            System.out.println("Error detectado: " + e.getMessage());
        }
    }
}

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta

Lanzar una excepción es el acto de crear el objeto de error y entregarlo al entorno de ejecución mediante la palabra throw. Capturar (o controlar) es rodear el código susceptible de fallo con un bloque try y definir una respuesta en un bloque catch. Si una función lanza una excepción y no hay un catch en ella, la excepción se propaga, subiendo automáticamente al nivel de la función que la llamó.

Durante la propagación, las funciones en la pila de llamadas se interrumpen inmediatamente; ninguna línea de código posterior al punto de error se ejecuta. La función "muere" prematuramente y se elimina de la pila. Si ninguna función en la jerarquía captura la excepción, esta llega al sistema operativo, provocando la terminación del programa. En el ejemplo de la raíz, si raiz falla, el control sale de raiz, vuelve al main y busca el catch; si no lo encuentra, el programa finaliza.

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta

La gran ventaja frente a C es que la propagación es automática e inevitable. En C, si una función de bajo nivel devuelve un código de error, el programador está obligado a comprobar manualmente ese retorno en cada nivel intermedio de la pila de llamadas para seguir pasando el error hacia arriba. Un solo olvido en un if intermedio puede hacer que el programa ignore el fallo y continúe operando con datos corruptos.

En Java, el error "viaja" por sí solo. No hace falta escribir código en las capas intermedias para que el error llegue desde la base hasta la interfaz de usuario. Esto garantiza que un error nunca pase desapercibido: o se gestiona explícitamente en algún punto, o el programa se detiene. Esto simplifica enormemente el diseño de aplicaciones con muchas capas de profundidad.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta

En Java y otros lenguajes orientados a objetos, las excepciones son instancias de clases. Esto significa que pueden tener atributos y métodos, permitiendo encapsular mucha más información que un simple número de error de C. Al ser objetos, se organizan en una jerarquía de herencia, lo que permite capturar grupos de errores relacionados (por ejemplo, capturar cualquier error de entrada/salida capturando la clase base IOException).

Esta estructura permite crear excepciones personalizadas simplemente heredando de clases existentes. Esto es vital para la encapsulación: una biblioteca de gestión de nóminas puede lanzar una SalarioNegativoException que contenga el empleado afectado y el valor erróneo. El programador recibe un objeto rico en contexto, no solo un código genérico.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta

A diferencia de C, donde un error suele ser un simple int, un objeto excepción en Java porta información crucial de forma nativa. La más importante es el Stack Trace (traza de la pila), que es una lista detallada de todas las funciones y números de línea que se estaban ejecutando en el momento preciso del error. Esto permite localizar el origen del fallo al instante.

Además, el objeto lleva un mensaje descriptivo (String) que explica la causa humana del error y, opcionalmente, una referencia a otra excepción (causa original), lo que permite rastrear errores encadenados. Toda esta información está encapsulada dentro del objeto y se entrega automáticamente al manejador catch.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta

Sí, es posible y recomendable tener varios bloques catch asociados a un único try. Esto permite dar respuestas diferenciadas según el tipo de error que ocurra. Por ejemplo, en una operación de red, podríamos querer reintentar la conexión si falla el servidor, pero mostrar un mensaje de error crítico si el formato de los datos es incorrecto.

Es fundamental destacar que solo se ejecuta un bloque catch: el primero que coincida con el tipo de la excepción lanzada (o una de sus superclases). Por ello, el orden es crítico: se deben colocar primero las excepciones más específicas (subclases) y al final las más generales, para evitar que un catch genérico "se coma" a los demás.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta

El bloque finally garantiza la ejecución de un fragmento de código independientemente de si se produjo una excepción o no. Su uso principal es la liberación de recursos críticos (cerrar ficheros, liberar sockets, cerrar conexiones a bases de datos) que no deben quedar abiertos aunque el programa falle y se interrumpa el flujo normal.

Se coloca después de los bloques catch (o directamente tras el try si no hay catch). Aunque la excepción se propague hacia arriba, el código dentro de finally se ejecuta antes de que el control abandone la función actual.

Java
try {
    abrirFichero();
    leerDatos();
} catch (IOException e) {
    System.out.println("Error al leer");
} finally {
    cerrarFichero(); // Se ejecutará siempre, haya error o no
}

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta

El bloque finally es extremadamente persistente. Se puede usar perfectamente sin bloques catch (estructura try-finally), lo cual es útil cuando no queremos manejar el error en esa función pero sí asegurar que los recursos se liberen antes de que el error se propague hacia el llamador.

Incluso si hay una instrucción return dentro del bloque try o de un catch, Java garantiza que el código de finally se ejecute antes de que el método devuelva el control. Es una de las pocas garantías absolutas del lenguaje respecto al orden de ejecución en situaciones de salida forzada.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta

Las excepciones controladas (checked) son aquellas que el compilador te obliga a gestionar o declarar (heredan de Exception). Representan situaciones externas que el programa debe prever (un fichero que no existe). Las no controladas (unchecked) heredan de RuntimeException y representan errores de programación (división por cero, puntero nulo).

Usar controladas (Checked) cuando:

El error es recuperable.

Es un fallo externo previsible (red, disco).

Se quiere obligar al programador a pensar en el fallo.

Situaciones de API pública donde el fallo es parte del contrato.

Usar no controladas (Unchecked) cuando:

El error es un "bug" del programador (violación de precondiciones).

No hay forma lógica de recuperarse en ese punto.

Se quiere evitar ensuciar el código con firmas de métodos largas.

Errores de lógica interna (índice de array fuera de rango).

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta

La palabra clave throws se utiliza en la firma de un método para declarar que dicho método puede lanzar ciertas excepciones controladas sin capturarlas internamente. Es una advertencia para quien llame al método: "ten cuidado, este método puede fallar de esta manera y tú serás el responsable de gestionar el error".

Es la alternativa a try-catch cuando la función actual no tiene suficiente información o responsabilidad para resolver el problema. En lugar de atrapar el error, lo "pasa" hacia arriba formalmente, cumpliendo con las reglas del compilador de Java para las excepciones controladas.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta

En este ejemplo, el método procesar delega el manejo del error del fichero al nivel superior, pero asume la responsabilidad de cerrar el recurso pase lo que pase.

Java
public void procesarFichero() throws FileNotFoundException {
    Scanner sc = null;
    try {
        sc = new Scanner(new File("datos.txt"));
        // Lógica de lectura
    } finally {
        if (sc != null) sc.close(); // Limpieza obligatoria
    }
}

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta

Es legal poner excepciones no controladas (como RuntimeException) en la cláusula throws, pero es opcional y no tiene efecto sobre el compilador. El llamador no estará obligado a poner un try-catch.

Su único sentido es el de documentación. Al incluirlo en la firma, se está comunicando a otros programadores que existe un riesgo específico de ese error, permitiéndoles capturarlo si lo consideran oportuno para mejorar la robustez de su código, aunque el lenguaje no les fuerce a ello.

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta

Se recomienda usar excepciones controladas para fallos de los que el usuario puede recuperarse (ej. reintentar una contraseña) y no controladas para errores de lógica (ej. pasar un parámetro nulo). No todos los lenguajes tienen esta distinción. C++, C# o Python solo tienen excepciones "no controladas" (en el sentido de que nada te obliga a capturarlas).

En los lenguajes que solo tienen una opción, la tendencia moderna es el estilo de las excepciones no controladas. Se considera que obligar al programador a capturar errores en cada paso genera código repetitivo y que, en la mayoría de los casos reales, el manejador acaba simplemente registrando el error y deteniendo la tarea, algo que la propagación automática ya hace sin esfuerzo extra.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta

Es muy común lanzar excepciones dentro de un catch. Se puede relanzar la misma excepción capturada si queremos que el error siga subiendo pero necesitamos realizar una acción intermedia (como guardar un log). También se puede lanzar una excepción nueva para realizar una traducción de errores (pasar de un error técnico de base de datos a un error de negocio entendible).

Java
// Caso 1: Relanzar
catch (IOException e) {
    log.error("Fallo de disco");
    throw e; // Sigue propagándose
}

// Caso 2: Nueva excepción
catch (SQLException e) {
    throw new ErrorDeAccesoException("No se pudo validar el usuario");
}

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

La técnica de la "causa" consiste en envolver una excepción original dentro de una nueva. Esto permite mantener la abstracción (el usuario ve un error de "Fallo en Registro") sin perder la información técnica de bajo nivel que lo provocó (una "Conexión rechazada" en el puerto 3306).

Cuando estas excepciones se imprimen (Stack Trace), Java muestra la nueva excepción y añade una sección "Caused by:" con los detalles de la original. Esto es fundamental para depurar sistemas complejos donde un error en una capa profunda provoca una cascada de fallos hacia arriba.

Java
try {
    conectarDB();
} catch (SQLException e) {
    // Encapsulamos la causa original 'e'
    throw new MiExcepcionPersonalizada("Error de negocio", e);
}