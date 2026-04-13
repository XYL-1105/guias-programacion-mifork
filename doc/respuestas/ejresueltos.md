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
Las cuatro características básicas de la programación orientada a objetos son encapsulamiento, abstracción, herencia y polimorfismo. Estas propiedades permiten organizar el programa alrededor de entidades llamadas objetos, que combinan datos y comportamiento, en lugar de separar funciones y estructuras como se hace habitualmente en C.

El encapsulamiento consiste en agrupar los datos y las operaciones que los manipulan dentro de una misma unidad (la clase), ocultando los detalles internos y exponiendo solo lo necesario mediante una interfaz pública. De esta forma, se evita el acceso directo e incontrolado a los datos, algo común en C con estructuras y variables globales.

La abstracción permite representar solo las características relevantes de un objeto para el problema que se desea resolver, ignorando los detalles innecesarios. Se trata de definir “qué hace” un objeto, sin centrarse inicialmente en “cómo lo hace”, facilitando así el diseño y la comprensión del sistema.

La herencia posibilita crear nuevas clases a partir de otras ya existentes, reutilizando su comportamiento. El polimorfismo, por su parte, permite que diferentes objetos respondan de forma distinta a una misma operación, haciendo posible que un mismo mensaje o método tenga comportamientos variados según el objeto que lo reciba.



## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta
Existen numerosos lenguajes de programación que permiten aplicar los principios de la programación orientada a objetos. Estos lenguajes incorporan mecanismos para definir clases, crear objetos y utilizar conceptos como encapsulamiento, herencia y polimorfismo.

Entre los más populares se encuentra Java, diseñado desde su origen con un enfoque completamente orientado a objetos. También destaca C++, que añade características orientadas a objetos al lenguaje C, permitiendo combinar programación estructurada y orientada a objetos.

Otro lenguaje muy utilizado es Python, que soporta plenamente la orientación a objetos con una sintaxis sencilla y flexible. Finalmente, C# es un lenguaje moderno desarrollado por Microsoft que también sigue el paradigma orientado a objetos de manera muy estricta y estructurada.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta
La programación estructurada es un paradigma anterior a la programación orientada a objetos que propone organizar el programa mediante estructuras de control claras y bien definidas, como secuencias, condicionales y bucles, evitando el uso indiscriminado de saltos como goto. Su objetivo principal es mejorar la legibilidad, el orden y el mantenimiento del código, dividiendo el programa en bloques lógicos que siguen un flujo de ejecución predecible.

En este enfoque, el programa se compone principalmente de funciones o procedimientos que operan sobre datos que suelen estar definidos fuera de ellas, como variables globales o estructuras compartidas. Es el estilo habitual en C, donde se separan claramente las funciones de los datos sobre los que trabajan, lo que puede dificultar el control del acceso a dichos datos.

La programación modular es una evolución natural de la programación estructurada que propone dividir el programa en módulos independientes, cada uno con una responsabilidad concreta. Cada módulo agrupa funciones y datos relacionados, reduciendo la dependencia entre distintas partes del programa y facilitando la reutilización.

Este enfoque busca que cada módulo pueda desarrollarse, probarse y mantenerse de forma aislada, comunicándose con otros módulos a través de interfaces bien definidas. Sin embargo, aunque agrupa funciones y datos, todavía no los integra en una única entidad como ocurre en la programación orientada a objetos con las clases.



## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta
Un objeto en programación orientada a objetos se define por su estado, su comportamiento y su identidad. Estos tres elementos permiten diferenciar claramente un objeto de otro y entender su papel dentro del programa.

El estado está formado por los datos que almacena el objeto, es decir, sus atributos o variables internas. El comportamiento viene dado por los métodos que puede ejecutar, que son las operaciones que modifican o consultan su estado. La identidad permite distinguir un objeto de otro, incluso aunque tengan los mismos valores en sus atributos.


## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta
Una clase es una plantilla o molde que define cómo serán los objetos: qué datos tendrán y qué operaciones podrán realizar. No contiene datos concretos, sino la definición de su estructura y comportamiento.

Una instancia es un objeto concreto creado a partir de una clase. Por tanto, una clase no es lo mismo que un objeto, sino que el objeto es el resultado de utilizar esa definición. Se pueden crear múltiples instancias de una misma clase, cada una con su propio estado.

No todos los lenguajes orientados a objetos utilizan obligatoriamente el concepto de clase. Algunos lenguajes, como Java o C#, sí se basan completamente en clases, mientras que otros, como JavaScript, permiten un modelo basado en prototipos.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta
En muchos lenguajes orientados a objetos, los objetos se almacenan en la zona de memoria conocida como montículo (heap), mientras que las variables que contienen referencias a esos objetos suelen estar en la pila (stack). Esto permite que los objetos sigan existiendo mientras haya referencias hacia ellos.

No todos los lenguajes gestionan la memoria de la misma forma. En C++ el programador controla manualmente la reserva y liberación de memoria, mientras que en Java este proceso está automatizado.

La recolección de basura (garbage collection) es el mecanismo mediante el cual el sistema libera automáticamente la memoria ocupada por objetos que ya no tienen referencias activas, evitando fugas de memoria sin intervención del programador.

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta
Un método es una función asociada a una clase que define una operación que pueden realizar sus objetos. A diferencia de las funciones en C, los métodos operan sobre el estado interno del objeto al que pertenecen.

La sobrecarga de métodos consiste en definir varios métodos con el mismo nombre dentro de una clase, pero con distinta lista de parámetros (número o tipo). El compilador decide cuál ejecutar según los argumentos utilizados en la llamada.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta
class Punto {
double x;
double y;

double calculaDistanciaAOrigen() {
    return Math.sqrt(x * x + y * y);
}
}

public class Main {
public static void main(String[] args) {
Punto p = new Punto();
p.x = 3;
p.y = 4;

    System.out.println(p.calculaDistanciaAOrigen());
}
}


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta
El punto de entrada de un programa en Java es el método public static void main(String[] args). Es el primer método que la máquina virtual ejecuta al iniciar el programa.

La palabra clave static indica que el método pertenece a la clase y no a una instancia concreta. No solo se usa en main, también puede aplicarse a atributos y otros métodos.

Combinado con final, indica que el valor no podrá modificarse tras su inicialización, siendo habitual en constantes.



## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta
Para compilar un programa en Java se utiliza javac NombreArchivo.java, lo que genera un archivo .class. Para ejecutarlo, se usa java NombreClase.

Java es un lenguaje compilado a un formato intermedio llamado byte-code. Este código no es ejecutado directamente por el sistema operativo.

La máquina virtual de Java (JVM) interpreta este byte-code, permitiendo que el mismo programa funcione en distintos sistemas sin recompilar.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta
La palabra clave new crea un objeto en memoria y llama a su constructor. Un constructor es un método especial que inicializa el objeto al crearse.

class Empleado {
String dni;
String nombre;
String apellidos;

Empleado(String dni, String nombre, String apellidos) {
    this.dni = dni;
    this.nombre = nombre;
    this.apellidos = apellidos;
}
}

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta
La referencia this apunta al propio objeto que está ejecutando el método. Permite diferenciar entre atributos y parámetros con el mismo nombre.

No todos los lenguajes usan el mismo nombre, pero el concepto es similar.

double calculaDistanciaAOrigen() {
return Math.sqrt(this.x * this.x + this.y * this.y);
}

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta
double distanciaA(Punto otro) {
double dx = this.x - otro.x;
double dy = this.y - otro.y;
return Math.sqrt(dx * dx + dy * dy);
}



## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta
En Java, los objetos se pasan por valor de la referencia. Esto significa que se copia la referencia, pero ambos apuntan al mismo objeto.

Si se modifica el estado del objeto dentro del método, los cambios se reflejan fuera. Sin embargo, si se cambia la referencia, no afecta al original.

En el caso de tipos primitivos como int, se pasa una copia del valor, por lo que los cambios no afectan al exterior.



## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta
toString() es un método que devuelve una representación en texto del objeto. Existe en muchos lenguajes con nombres similares.

public String toString() {
return "(" + x + ", " + y + ")";
}

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

### Respuesta
Una clase se parece a un struct en cuanto a que ambos agrupan datos. Sin embargo, el struct no integra comportamiento ni control de acceso.

Para ser como una clase, un struct necesitaría asociar funciones y mecanismos de encapsulamiento.



## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
typedef struct {
double x;
double y;
} Punto;

double calculaDistanciaAOrigen(Punto *p) {
return sqrt(p->x * p->x + p->y * p->y);
}

Aquí, el puntero p actúa como el equivalente a this, pasándose explícitamente a la función.

