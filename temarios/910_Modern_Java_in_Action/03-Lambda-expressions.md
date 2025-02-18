# 3. Lambda expressions

Este capítulo cubre

* Lambdas en pocas palabras
* Dónde y cómo utilizar lambdas
* El patrón de ejecución
* Interfaces funcionales, inferencia de tipos.
* Referencias de métodos
* Componiendo lambdas

En el capítulo anterior, vio que pasar código con parametrización de comportamiento es útil para hacer frente a cambios frecuentes de requisitos en su código. Le permite definir un bloque de código que representa un comportamiento y luego transmitirlo. Puede decidir ejecutar ese bloque de código cuando ocurre un determinado evento (por ejemplo, un clic en un botón) o en ciertos puntos de un algoritmo (por ejemplo, un predicado como "solo manzanas que pesen más de 150 g" en el algoritmo de filtrado o la operación de comparación personalizada en la clasificación). En general, con este concepto puedes escribir código que sea más flexible y reutilizable.

Pero viste que usar clases anónimas para representar diferentes comportamientos no es satisfactorio. Es detallado, lo que no anima a los programadores a utilizar la parametrización del comportamiento en la práctica. En este capítulo, le enseñaremos acerca de una nueva característica en Java 8 que aborda este problema: expresiones lambda. Le permiten representar un comportamiento o un código de acceso de forma concisa. Por ahora puedes pensar en las expresiones lambda como funciones anónimas, métodos sin nombres declarados, pero que también pueden pasarse como argumentos a un método como puedes hacerlo con una clase anónima.

Le mostraremos cómo construirlos, dónde usarlos y cómo puede hacer que su código sea más conciso al usarlos. También explicamos algunas novedades como la inferencia de tipos y nuevas interfaces importantes disponibles en la API de Java 8. Finalmente, presentamos referencias a métodos, una nueva característica útil que va de la mano con las expresiones lambda.

Este capítulo está organizado de tal manera que le enseñe paso a paso cómo escribir código más conciso y flexible. Al final de este capítulo, reunimos todos los conceptos enseñados en un ejemplo concreto; Tomamos el ejemplo de clasificación que se muestra en el capítulo 2 y lo mejoramos gradualmente utilizando expresiones lambda y referencias de métodos para hacerlo más conciso y legible. Este capítulo es importante en sí mismo y también porque utilizará lambdas ampliamente a lo largo del libro.

## 3.1. Lambdas en pocas palabras

Una expresión lambda puede entenderse como una representación concisa de una función anónima que se puede transmitir. No tiene nombre, pero tiene una lista de parámetros, un cuerpo, un tipo de retorno y posiblemente también una lista de excepciones que se pueden generar. Ésa es una gran definición; analicémoslo:

* **Anonymous - Anónimo**: decimos anónimo porque no tiene un nombre explícito como lo tendría normalmente un método; ¡Menos para escribir y pensar!
* **Function - Función**: decimos función porque una lambda no está asociada con una clase en particular como lo está un método. Pero al igual que un método, una lambda tiene una lista de parámetros, un cuerpo, un tipo de retorno y una posible lista de excepciones que se pueden generar.
* **Passed around - Pasado**: una expresión lambda se puede pasar como argumento a un método o almacenarse en una variable.
* **Concise - Conciso** : no es necesario escribir mucho texto repetitivo como lo hace para las clases anónimas.
 
Si se pregunta de dónde viene el término lambda, proviene de un sistema desarrollado en el mundo académico llamado cálculo lambda , que se utiliza para describir cálculos.

¿Por qué debería importarle las expresiones lambda? Viste en el capítulo anterior que pasar código actualmente es tedioso y detallado en Java. Bueno, ¡buenas noticias! Lambdas soluciona este problema; te permiten pasar código de forma concisa. Lambdas técnicamente no te permite hacer nada que no pudieras hacer antes de Java 8. ¡Pero ya no tienes que escribir código torpe usando clases anónimas para beneficiarte de la parametrización del comportamiento! Las expresiones Lambda lo alentarán a adoptar el estilo de parametrización de comportamiento que describimos en el capítulo anterior. El resultado neto es que su código será más claro y flexible. Por ejemplo, utilizando una expresión lambda puedes crear un objeto **`Comparator`** personalizado de una forma más concisa.

Antes:

```java
Comparator<Apple> byWeight = new Comparator<Apple>() {
    public int compare(Apple a1, Apple a2){
        return a1.getWeight().compareTo(a2.getWeight());
    }
};
```

Después (con expresiones lambda):

```java
Comparator<Apple> byWeight =
    (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
```

¡Debes admitir que el código parece más claro! No te preocupes si todas las partes de la expresión lambda aún no tienen sentido; Te explicaremos todas las piezas pronto. Por ahora, tenga en cuenta que literalmente está pasando solo el código necesario para comparar dos manzanas usando su peso. Parece que estás pasando el cuerpo del método compare. Pronto aprenderá que puede simplificar su código aún más. En la siguiente sección explicaremos exactamente dónde y cómo se pueden utilizar expresiones lambda.

La lambda que acabamos de mostrar tiene tres partes, como se muestra en la figura 3.1 :

**Figura 3.1.Una expresión lambda se compone de parámetros, una flecha y un cuerpo.**

![image](https://github.com/adolfodelarosades/Java/assets/23094588/bdb476e3-bfd4-45fa-82e4-056baf574628)

* **Una lista de parámetros**: en este caso refleja los parámetros delcomparemétodo de aComparator—twoApples.
* **Una flecha**: la flecha->separa la lista de parámetros del cuerpo de la lambda.
* **El body del lambda**: compara dosApples usando sus pesos. La expresión se considera el valor de retorno de lambda.

Para ilustrar más, la siguiente lista muestra cinco ejemplos de expresiones lambda válidas en Java 8.

**Listado 3.1.Expresiones lambda válidas en Java 8**

```java
(String s) -> s.length()                                             1
(Apple a) -> a.getWeight() > 150                                     2
(int x, int y) -> {
    System.out.println("Result:");
    System.out.println(x + y);
}                                                                    3
() -> 42                                                             4
(Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight())     5
```

* **1 Toma un parámetro de tipo `String` y devuelve un `int`. No tiene declaración de devolución ya que la devolución está implícita.**
* **2 Toma un parámetro de tipo `Apple` y devuelve un valor `booleano` (si la manzana pesa más de 150 g).**
* **3 Toma dos parámetros de tipo `int` y no devuelve ningún valor (retorno `void`). Su cuerpo contiene dos declaraciones.**
* **4 No toma ningún parámetro y devuelve el `int 42`**
* **5 Toma dos parámetros de tipo `Apple` y devuelve un `int` que representa la comparación de sus pesos.**

Los diseñadores del lenguaje Java eligieron esta sintaxis porque fue bien recibida en otros lenguajes, como C# y Scala. JavaScript tiene una sintaxis similar. La sintaxis básica de una lambda es (denominada lambda de estilo de expresión )

```java
(parameters) -> expression
```

o (tenga en cuenta las llaves para las declaraciones, esta lambda a menudo se denomina lambda de ***block-style - estilo bloque***)

```java
parameters) -> { statements; }
    List<Apple> result = new ArrayList<>();
    for (Apple apple: inventory) {
        if ( 
            result.add(apple);
        }
    }
    return result;
}
```

Como puede ver, las expresiones lambda siguen una sintaxis simple. Completar el cuestionario 3.1 debería permitirle saber si comprende el patrón.

**Prueba 3.1: sintaxis Lambda**

Según las reglas de sintaxis que se acaban de mostrar, ¿cuáles de las siguientes no son expresiones lambda válidas?


1. **`() -> {}`**
2. **`() -> "Raoul"`**
3. **`() -> { return "Mario"; }`**
4. **`(Integer i) -> return "Alan" + i;`**
5. **`(String s) -> { "Iron Man"; }`**


**Respuesta:**

4 y 5 son lambdas no válidas; el resto son válidos. Detalles:

1. Esta lambda no tiene parámetros y devuelve **`void`**. Es similar a un método con un cuerpo vacío: **`public void run() { }`**. Dato curioso: a esto se le suele llamar ***burger lambda***. Míralo desde un lado y verás que tiene forma de hamburguesa con dos bollos.
2. Esta lambda no tiene parámetros y devuelve a **`String`** como expresión.
3. Esta lambda no tiene parámetros y devuelve un **`String`**(usando una declaración de retorno explícita, dentro de un bloque).
4. **`return`** una declaración de flujo de control. Para que esta lambda sea válida, se requieren llaves de la siguiente manera: **`(Integer i) -> { return "Alan" + i; }`**.
5. "Iron Man" es una expresión, no una declaración. Para que esta lambda sea válida, puede eliminar las llaves y el punto y coma de la siguiente manera: **`(String s) -> "Iron Man"`**. O si lo prefiere, puede utilizar una declaración de devolución explícita de la siguiente manera: **`(String s) -> { return "Iron Man"; }`**.
   
**Tabla 3.1** Proporciona una lista de lambdas de ejemplo con ejemplos de casos de uso.

![image](https://github.com/adolfodelarosades/Java/assets/23094588/4f735643-acef-46fa-bd71-8115970aea49)


## 3.2. Dónde y cómo utilizar lambdas

Quizás ahora te preguntes dónde puedes usar expresiones lambda. En el ejemplo anterior, asignaste una lambda a una variable de tipo **`Comparator<Apple>`**. También puedes usar otra lambda con el método **`filter`** que implementaste en el capítulo anterior:

```java
List<Apple> greenApples =
        filter(inventory, (Apple a) -> GREEN.equals(a.getColor()));
```

¿Dónde exactamente puedes usar lambdas? Puede utilizar una expresión lambda en el contexto de una interfaz funcional. En el código que se muestra aquí, puede pasar un lambda como segundo argumento del método **`filter`** porque espera un objeto de tipo **`Predicate<T>`**, que es una interfaz funcional. No te preocupes si esto suena abstracto; Ahora explicaremos en detalle qué significa esto y qué es una interfaz funcional.

### 3.2.1. Interfaz funcional

¿Recuerda la interfaz **`Predicate<T>`** que creó en el capítulo 2 para poder parametrizar el comportamiento del método **`filter`**? ¡Es una interfaz funcional! ¿Por qué? Porque **`Predicate`** especifica solo un método abstracto:

```java
public interface Predicate<T> {
    boolean test (T t);
}
```

En pocas palabras, ***una interfaz funcional es una interfaz que especifica exactamente un método abstracto***. Ya conoce varias otras interfaces funcionales en la API de Java, como **`Comparator`** y **`Runnable`**, que exploramos en el capítulo 2:

```java
public interface Comparator<T> {                           1
    int compare(T o1, T o2);
}
public interface Runnable {                                2
    void run();
}
public interface ActionListener extends EventListener {    3
    void actionPerformed(ActionEvent e);
}
public interface Callable<V> {                             4
    V call() throws Exception;
}
public interface PrivilegedAction<T> {                     5
    T run();
}
```

* **1 `java.util.Comparador`**
* **2 `java.lang.Runnable`**
* **3 `java.awt.event.ActionListener`**
* **4 `java.util.concurrent.Invocable`**
* **5 `java.seguridad.Acción Privilegiada`**

<hr>

**Nota**

Verá en el capítulo 13 que las interfaces ahora también pueden tener métodos predeterminados (un método con un cuerpo que proporciona alguna implementación predeterminada para un método en caso de que no lo implemente una clase). Una interfaz sigue siendo funcional si tiene muchos métodos predeterminados, siempre y cuando especifique sólo un método abstracto .

<hr>

Para comprobar su comprensión, el cuestionario 3.2 debería permitirle saber si comprende el concepto de interfaz funcional.

**Prueba 3.2: Interfaz funcional**

¿Cuáles de estas interfaces son interfaces funcionales?

```java
public interface Adder {
    int add(int a, int b);
}
public interface SmartAdder extends Adder {
    int add(double a, double b);
}
public interface Nothing {
}
```

**Respuesta:**

Sólo **`Adder`** es una interfaz funcional.

**`SmartAdder`** no es una interfaz funcional porque especifica dos métodos abstractos llamados **`add`**(uno se hereda de **`Adder`**).

**`Nothing`** no es una interfaz funcional porque no declara ningún método abstracto.

¿Qué se puede hacer con las interfaces funcionales? Las expresiones Lambda le permiten proporcionar la implementación del método abstracto de una interfaz funcional directamente en línea y tratar la expresión completa como una instancia de una interfaz funcional (más técnicamente hablando, una instancia de una implementación concreta de la interfaz funcional). Puede lograr lo mismo con una clase interna anónima, aunque es más complicado: proporciona una implementación y crea una instancia directamente en línea. El siguiente código es válido porque **`Runnable`** es una interfaz funcional que define solo un método abstracto **`run`**:

```java
Runnable r1 = () -> System.out.println("Hello World 1");         1
Runnable r2 = new Runnable() {                                   2
    public void run() {
        System.out.println("Hello World 2");
    }
};
public static void process(Runnable r) {
    r.run();
}
process(r1);                                                     3
process(r2);                                                     4
process(() -> System.out.println("Hello World 3"));              5
```

* **1 Utiliza una lambda**
* **2 Utiliza una clase anónima**
* **3 impresiones “Hello World 1”**
* **4 impresiones “Hello World 2”**
* **5 Imprime “Hello World 3” con una lambda pasada directamente**

### 3.2.2. Function descriptor

La firma del método abstracto de la interfaz funcional describe la firma de la expresión lambda. A este método abstracto lo llamamos descriptor de función. Por ejemplo, la interfaz **`Runnable`** puede verse como la firma de una función que no acepta nada ni devuelve nada (**`void`**) porque solo tiene un método abstracto llamado **`run`**, que no acepta nada ni devuelve nada (**`void`**). [ 1 ]

<hr>

**1**

*Algunos lenguajes como Scala proporcionan anotaciones de tipo explícitas en su sistema de tipos para describir el tipo de una función (llamadas tipos de función). Java reutiliza los tipos nominales existentes proporcionados por interfaces funcionales y los asigna a una forma de tipos de funciones detrás de escena.*

<hr>

Usamos una notación especial a lo largo de este capítulo para describir las firmas de lambdas y las interfaces funcionales. La notación **`() -> void`** representa una función con una lista vacía de parámetros que devuelven **`void`**. Esto es exactamente lo que la interfaz **`Runnable`** representa. Como otro ejemplo, **`(Apple, Apple) -> int`** denota una función que toma dos **`Apple`**s como parámetros y devuelve un **`int`**. Proporcionaremos más información sobre los descriptores de funciones en la sección 3.4 y en la tabla 3.2 más adelante en el capítulo.

Quizás ya se esté preguntando cómo se verifica el tipo de expresiones lambda. Detallamos cómo el compilador verifica si una lambda es válida en un contexto determinado en la sección 3.5 . Por ahora, basta con entender que se puede asignar una expresión lambda a una variable o se pasa a un método que espera una interfaz funcional como argumento, siempre que la expresión lambda tenga la misma firma que el método abstracto de la interfaz funcional. Por ejemplo, en nuestro ejemplo anterior, podría pasar una lambda directamente al método **`process`** de la siguiente manera:

```java
public void process(Runnable r) {
    r.run();
}
process(() -> System.out.println("This is awesome!!"));
```

Este código, cuando se ejecute, imprimirá "**`This is awesome!!`**" La expresión lambda **`() -> System.out.println("This is awesome!!")`** no toma parámetros y devuelve **`void`**. Esta es exactamente la firma del método **`run`** definido en la interfaz **`Runnable`**.

#### Lambdas e invocación del método `void`

Aunque esto pueda parecer extraño, la siguiente expresión lambda es válida:

```java
process(() -> System.out.println("This is awesome"));
```

Después de todo, **`System.out.println returns void`** ¡así que claramente esto no es una expresión! ¿Por qué no tenemos que encerrar el cuerpo con llaves como ésta?

```java
process(() -> { System.out.println("This is awesome"); });
```

Resulta que existe una regla especial para la invocación de un método void definida en la Java Language Specification. No es necesario encerrar una única invocación de método void entre llaves.

Quizás se pregunte: "¿Por qué podemos pasar una lambda sólo donde se espera una interfaz funcional?" Los diseñadores del lenguaje consideraron enfoques alternativos como agregar tipos de funciones (un poco como la notación especial que introdujimos para describir la firma de las expresiones lambda; revisaremos este tema en los capítulos 20 y 21) a Java. Pero eligieron este método porque encaja de forma natural sin aumentar la complejidad del lenguaje. Además, la mayoría de los programadores de Java ya están familiarizados con la idea de una interfaz con un único método abstracto (por ejemplo, para el manejo de eventos). Sin embargo, la razón más importante es que las interfaces funcionales ya se usaban ampliamente antes de Java 8. Esto significa que proporcionan una buena ruta de migración para usar expresiones lambda. De hecho, si ha estado usando interfaces funcionales como **`Comparatore`** y **`Runnable`** incluso sus propias interfaces que definen solo un método abstracto, ahora puede usar expresiones lambda sin cambiar sus API. Pruebe el cuestionario 3.3 para evaluar sus conocimientos sobre dónde se pueden utilizar lambdas.

**Prueba 3.3: ¿Dónde se pueden utilizar lambdas?**

¿Cuáles de los siguientes son usos válidos de las expresiones lambda?

1. 
```java
execute(() -> {});
public void execute(Runnable r) {
  r.run();
}
```

2.
```java
public Callable<String> fetch() {
  return () -> "Tricky example ;-)";
}
```

3.
```java
Predicate<Apple> p = (Apple a) -> a.getWeight();
```

**Respuesta:**

Sólo 1 y 2 son válidos.

El primer ejemplo es válido porque la lambda **`() -> {}`** tiene la firma **`() -> void`**, que coincide con la firma del método abstracto **`run`** definido en **`Runnable`**. Tenga en cuenta que ejecutar este código no hará nada porque el cuerpo de la lambda está vacío.

El segundo ejemplo también es válido. De hecho, el tipo de retorno del método **`fetch`** es **`Callable<String>. Callable<String>`** define un método con la firma **`() -> String`** cuando **`T`** se reemplaza con **`String`**. Debido a que lambda **`() -> "Tricky example ;-)"`**  tiene la firma **`() -> String`**, se puede usar en este contexto.

El tercer ejemplo no es válido porque la expresión lambda **`(Apple a) -> a.getWeight()`** tiene la firma **`(Apple) -> Integer`**, que es diferente de la firma del método **`test`** definido en **`Predicate<Apple>: (Apple) -> boolean`**.

#### ¿Qué pasa con `@FunctionalInterface`?

Si explora la nueva API de Java, notará que las interfaces funcionales generalmente están anotadas con **`@FunctionalInterface`**. (Mostramos una lista extensa en la sección 3.4, donde exploramos cómo usar las interfaces funcionales en profundidad). Esta anotación se usa para indicar que la interfaz es pretende ser una interfaz funcional y, por lo tanto, es útil para la documentación. Además, el compilador devolverá un error significativo si define una interfaz mediante la anotación **`@FunctionalInterface`** y no es una interfaz funcional. Por ejemplo, un mensaje de error podría ser **"Multiple non-overriding abstract methods found in interface Foo"** para indicar que hay más de un método abstracto disponible. Tenga en cuenta que la anotación **`@FunctionalInterface`** no es obligatoria, pero es una buena práctica utilizarla cuando una interfaz está diseñada para ese propósito. Puede considerarlo como la notación **`@Override`** que indica que se anula un método.

## 3.3. Poniendo lambdas en práctica: el patrón de ejecución

Veamos un ejemplo de cómo las lambdas, junto con la parametrización del comportamiento, se pueden utilizar en la práctica para hacer que su código sea más flexible y conciso. Un patrón recurrente en el procesamiento de recursos (por ejemplo, al tratar con archivos o bases de datos) es abrir un recurso, procesarlo y luego cerrarlo. Las fases de configuración y limpieza son siempre similares y rodean el código importante que realiza el procesamiento. Esto se denomina patrón de ejecución, como se ilustra en la figura 3.2. Por ejemplo, en el siguiente código, las líneas resaltadas muestran el código repetitivo necesario para leer una línea de un archivo (tenga en cuenta también que utiliza la instrucción **`try-with-resources`** de Java 7, que ya simplifica el código, porque no tiene para cerrar el recurso explícitamente):

**Figura 3.2.Las tareas A y B están rodeadas por un código repetitivo responsable de la preparación/limpieza.**

![image](https://github.com/adolfodelarosades/Java/assets/23094588/5abfc3fd-e060-4b87-9efa-c401383aefd4)


```java
public String processFile() throws IOException {
try (BufferedReader br =
             new BufferedReader(new FileReader("data.txt"))) {
        return br.readLine();                                     1
    }
}
```

**1 Esta es la línea que hace un trabajo útil.**

### 3.3.1. Paso 1: Recuerde la parametrización del comportamiento

Este código actual es limitado. Puede leer sólo la primera línea del archivo. ¿Qué sucede si en su lugar desea devolver las dos primeras líneas o incluso la palabra utilizada con más frecuencia? Idealmente, le gustaría reutilizar el código para realizar la configuración y la limpieza e indicarle al método **`processFile`** que realice diferentes acciones en el archivo. ¿Te suena esto familiar? Sí, es necesario parametrizar el comportamiento de **`processFile`**. Necesita una forma de pasarle el comportamiento **`processFile`** para que pueda ejecutar diferentes comportamientos usando un archivo **`BufferedReader`**.

El comportamiento de aprobación es exactamente para lo que sirven las lambdas. ¿Cómo debería verse el nuevo método **`processFile`** si desea leer dos líneas a la vez? Necesitas una lambda que toma a **`BufferedReader`** y devuelve a **`String`**. Por ejemplo, aquí se explica cómo imprimir dos líneas de un **`BufferedReader`**:

```java
String result
    = processFile((BufferedReader br) -> br.readLine() + br.readLine());
```

### 3.3.2. Paso 2: utilice una interfaz funcional para pasar comportamientos

Explicamos anteriormente que las lambdas solo se pueden usar en el contexto de una interfaz funcional. Debe crear uno que coincida con la firma **`BufferedReader -> String`** y que pueda generar un archivo **`IOException`**. Llamemos a esta interfaz **`BufferedReaderProcessor`**:

```java
@FunctionalInterface
public interface BufferedReaderProcessor {
    String process(BufferedReader b) throws IOException;
}
```

Ahora puede utilizar esta interfaz como argumento de su nuevo método **`processFile`**:

```java
public String processFile(BufferedReaderProcessor p) throws IOException {
   ...
}
```

### 3.3.3. Paso 3: ¡Ejecuta un comportamiento!

Cualquier lambda del formulario **`BufferedReader -> String`** se puede pasar como argumento, porque coincide con la firma del método **`process`** definido en la interfaz **`Buffered-ReaderProcessor`**. Ahora solo necesita una forma de ejecutar el código representado por lambda dentro del cuerpo de **`processFile`**. Recuerde, las expresiones lambda le permiten proporcionar la implementación del método abstracto de una interfaz funcional directamente en línea y tratan la expresión completa como una instancia de una interfaz funcional. Por lo tanto, puedes llamar al método **`process`** en el objeto  resultante **`BufferedReaderProcessor`** dentro del cuerpo para realizar el procesamiento **`processFile `**: 

```java
public String processFile(BufferedReaderProcessor p) throws IOException {
    try (BufferedReader br =
                   new BufferedReader(new FileReader("data.txt"))) {
        return p.process(br);                                          1
    }
}
```

**1 Procesa el objeto BufferedReader**

### 3.3.4. Paso 4: Pasar lambdas

Ahora puede reutilizar el método **`processFile`** y procesar archivos de diferentes maneras pasando diferentes lambdas.

A continuación se muestra el procesamiento de una línea:

```java
String oneLine =
    processFile((BufferedReader br) -> br.readLine());
```

A continuación se muestra el procesamiento de dos líneas:

```java
String twoLines =
    processFile((BufferedReader br) -> br.readLine() + br.readLine());
```

La Figura 3.3 resume los cuatro pasos dados para hacer el método **`processFile`** más flexible.

**Figura 3.3. Proceso de cuatro pasos para aplicar el patrón de ejecución circular**

![image](https://github.com/adolfodelarosades/Java/assets/23094588/745769d3-39f9-42f7-8907-2da5ab49ec7a)


Hemos mostrado cómo puede utilizar interfaces funcionales para pasar lambdas. Pero tenías que definir tus propias interfaces. En la siguiente sección, exploramos nuevas interfaces que se agregaron a Java 8 y que puede reutilizar para pasar múltiples lambdas diferentes.

## 3.4. Usando interfaces funcionales

Como aprendiste en la sección 3.2.1, una interfaz funcional especifica exactamente un método abstracto. Las interfaces funcionales son útiles porque la firma del método abstracto puede describir la firma de una expresión lambda. La firma del método abstracto de una interfaz funcional se llama ***function descriptor - descriptor de función***. Para utilizar diferentes expresiones lambda, necesita un conjunto de interfaces funcionales que puedan describir descriptores de funciones comunes. Varias interfaces funcionales ya están disponibles en la API de Java, como **`Comparable`**, **`Runnable`** y **`Callable`**, que vio en la sección 3.2.

Los diseñadores de la biblioteca Java para Java 8 le han ayudado introduciendo varias interfaces funcionales nuevas dentro del paquete **`java.util.function`**. Describiremos las interfaces **`Predicate`**, **`Consumer`** y **`Function`** a continuación. Una lista más completa está disponible en la tabla 3.2 al final de esta sección.

### 3.4.1. Predicate

La interfaz **`java.util.function.Predicate<T>`** define un método abstracto llamado **`test`** que acepta un objeto de tipo genérico **`T`** y devuelve un archivo **`boolean`**. Es exactamente el mismo que creaste anteriormente, ¡pero está disponible de fábrica! Es posible que desee utilizar esta interfaz cuando necesite representar una expresión booleana que utilice un objeto de tipo **`T`**. Por ejemplo, puede definir una lambda que acepte objetos **`String`**, como se muestra en la siguiente lista.

**Listado 3.2. Trabajando con un `Predicate`**

```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}
public <T> List<T> filter(List<T> list, Predicate<T> p) {
    List<T> results = new ArrayList<>();
    for(T t: list) {
        if(p.test(t)) {
            results.add(t);
        }
    }
    return results;
}
Predicate<String> nonEmptyStringPredicate = (String s) -> !s.isEmpty();
List<String> nonEmpty = filter(listOfStrings, nonEmptyStringPredicate);
```

Si busca la especificación Javadoc de la interfaz **`Predicate`**, puede observar métodos adicionales como **`and`** y **`or`**. No te preocupes por ellos por ahora. Volveremos a esto en la sección 3.8 .

### 3.4.2. Consumer

La interfaz **`java.util.function.Consumer<T>`** define un método abstracto llamado **`accept`** que toma un objeto de tipo genérico **`T`** y no devuelve ningún resultado (**`void`**). Puede utilizar esta interfaz cuando necesite acceder a un objeto de tipo **`T`** y realizar algunas operaciones sobre el mismo. Por ejemplo, puede usarlo para crear un método **`forEach`**, que toma una lista **`Integers`** y aplica una operación en cada elemento de esa lista. En el siguiente listado, utilizará este método **`forEach`** combinado con una lambda para imprimir todos los elementos de la lista.

**Listado 3.3. Trabajando con un `Consumer`**

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
public <T> void forEach(List<T> list, Consumer<T> c) {
    for(T t: list) {
        c.accept(t);
    }
}
forEach(
         Arrays.asList(1,2,3,4,5),
        (Integer i) -> System.out.println(i)         1
       );
```

**1 La lambda es la implementación del método de aceptación del Consumer.**

### 3.4.3. Function

La interfaz **`java.util.function.Function<T, R>`** define un método abstracto llamado **`apply`** que toma un objeto de tipo genérico **`T`** como entrada y devuelve un objeto de tipo genérico **`R`**. Puede utilizar esta interfaz cuando necesite definir una lambda que asigne información de un objeto de entrada a una salida (por ejemplo, extraer el peso de una manzana o asignar una cadena a su longitud). En el listado que sigue, mostramos cómo puede usarlo para crear un método **`map`** para transformar una lista de **`Strings`** en una lista de **`Integers`** que contiene la longitud de cada **`String`**.

**Listado 3.4. Trabajando con un `Function`**

```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}
public <T, R> List<R> map(List<T> list, Function<T, R> f) {
    List<R> result = new ArrayList<>();
    for(T t: list) {
        result.add(f.apply(t));
    }
    return result;
}
// [7, 2, 6]
List<Integer> l = map(
                       Arrays.asList("lambdas", "in", "action"),
                       (String s) -> s.length()                     1
               );
```

**1 Implementa el método de aplicación de `Function`.**

#### Especializaciones Primitive

Describimos tres interfaces funcionales que son genéricas: **`Predicate<T>`**, **`Consumer<T>`** y **`Function<T, R>`**. También hay interfaces funcionales que están especializadas en ciertos tipos.

Para actualizar un poco: cada tipo de Java es un tipo de referencia (por ejemplo, **`Byte`**, **`Integer`**, **`Object`**, **`List`**) o un tipo primitivo (por ejemplo, **`int`**, **`double`**, **`byte`**, **`char`**). Pero los parámetros genéricos (por ejemplo, **`T`** en **`Consumer<T>`**) solo pueden vincularse a tipos de referencia. Esto se debe a cómo se implementan internamente los genéricos. [ 2 ] Como resultado, en Java existe un mecanismo para convertir un tipo primitivo en un tipo de referencia correspondiente. Este mecanismo se llama ***boxing***. El enfoque opuesto (convertir un tipo de referencia en un tipo primitivo correspondiente) se llama ***unboxing***. Java también tiene un mecanismo de **`autoboxing`** para facilitar la tarea a los programadores: las operaciones de ***boxing*** y ***unboxing*** se realizan automáticamente. Por ejemplo, esta es la razón por la que el siguiente código es válido (un **`int`** se encuadra en un **`Integer`** ): 

<hr>

**2**

*Algunos otros lenguajes, como C#, no tienen esta restricción. Otros lenguajes, como Scala, sólo tienen tipos de referencia. Revisaremos esta cuestión en el capítulo 20.*

<hr>

```java
List<Integer> list = new ArrayList<>();
for (int i = 300; i < 400; i++){
    list.add(i);
}
```

Pero esto tiene un coste de rendimiento. Los valores encuadrados envuelven los tipos primitivos y se almacenan en el montón. Por lo tanto, los valores encuadrados utilizan más memoria y requieren búsquedas de memoria adicionales para recuperar el valor primitivo ajustado.

Java 8 también agregó una versión especializada de las interfaces funcionales que describimos anteriormente para evitar operaciones de autoboxing cuando las entradas o salidas son primitivas. Por ejemplo, en el siguiente código, usar un **`Predicate<Integer>`** evita una operación boxing del valor **`1000`**, mientras que usar un **` Predicate<Integer>`** encuadraría (would box) el argumento **`1000 `** de un objeto **`Integer`**:

```java
public interface IntPredicate {
    boolean test(int t);
}
IntPredicate evenNumbers = (int i) -> i % 2 == 0;
evenNumbers.test(1000);                                        1
Predicate<Integer> oddNumbers = (Integer i) -> i % 2 != 0;
oddNumbers.test(1000);                                         2
```

* **1 True (no boxing)**
* **2 False (boxing)**

En general, el tipo primitivo apropiado precede a los nombres de las interfaces funcionales que tienen una especialización para el parámetro de tipo de entrada (por ejemplo, **`DoublePredicate`**, **`IntConsumer`**, **`LongBinaryOperator`**, **`IntFunction`** etc.). La interfaz **`Function`** también tiene variantes para el parámetro de tipo de salida: **`ToIntFunction<T>`**, **`IntToDoubleFunction`**, etc.

La **Tabla 3.2** resume las interfaces funcionales más utilizadas disponibles en la API de Java y sus descriptores de funciones, junto con sus especializaciones primitivas. Tenga en cuenta que estos son solo un kit inicial y que siempre puede crear el suyo propio si es necesario (el cuestionario 3.7 inventa **`TriFunction`** para este propósito). Crear sus propias interfaces también puede ser útil cuando un nombre de dominio específico ayude con la comprensión y el mantenimiento del programa. Recuerde, la notación **`(T, U) -> R`** muestra cómo pensar en un descriptor de función. El lado izquierdo de la flecha es una lista que representa los tipos de argumentos y el lado derecho representa los tipos de resultados. En este caso, representa una función con dos argumentos de tipo genérico respectivamente **`T`** y **`U`** que tiene un tipo de retorno de **`R`**.

**Tabla 3.2. Interfaces funcionales comunes agregadas en Java 8**

![image](https://github.com/adolfodelarosades/Java/assets/23094588/a7f816d2-ff77-4b2d-b2a4-6ac08854a227)
![image](https://github.com/adolfodelarosades/Java/assets/23094588/ca88a4f8-1cfd-42a1-aa99-66f9f45acc20)

Ahora ha visto muchas interfaces funcionales que se pueden usar para describir la firma de varias expresiones lambda. Para comprobar su comprensión hasta ahora, realice la prueba 3.4.

**Prueba 3.4: Functional interface - Interfaces funcionales**

¿Qué interfaces funcionales usaría para los siguientes descriptores de funciones (firmas de expresión lambda)? Encontrarás la mayoría de las respuestas en la tabla 3.2 . Como ejercicio adicional, cree expresiones lambda válidas que pueda utilizar con estas interfaces funcionales.

1. **`T -> R`**
2. **`(int, int) -> int`**
3. **`T -> void`**
4. **`() -> T`**
5. **`(T, U) -> R`**

**Respuestas:**

1. **`Function<T, R>`** es un buen candidato. Normalmente se utiliza para convertir un objeto de tipo **`T`** en un objeto de tipo **`R`** (por ejemplo, **`Function<Apple, Integer>`** para extraer el peso de una manzana).
2. **`IntBinaryOperator`** Tiene un único método abstracto llamado **`applyAsInt`** representación de un descriptor de función **`(int, int) -> int`**.
3. **`Consumer<T>`** Tiene un único método abstracto llamado **`accept`** representación de un descriptor de función **`T -> void`**.
4. **`Supplier<T>`** Tiene un único método abstracto llamado **`get`** representación de un descriptor de función **`() -> T`**.
5. **`BiFunction<T, U, R>`** Tiene un único método abstracto llamado **`apply`** representación de un descriptor de función **`(T, U) -> R`**.

Para resumir la discusión sobre interfaces funcionales y lambdas, la tabla 3.3 proporciona un resumen de casos de uso, ejemplos de lambdas e interfaces funcionales que se pueden usar.

**Tabla 3.3. Ejemplos de lambdas con interfaces funcionales.**

![image](https://github.com/adolfodelarosades/Java/assets/23094588/e844a1a9-08dd-4f9a-8d0e-9a5856b1c912)

#### ¿Qué pasa con las excepciones, lambdas y las interfaces funcionales?

Tenga en cuenta que ninguna de las interfaces funcionales permite que se lance una excepción marcada. Tiene dos opciones si necesita el cuerpo de una expresión lambda para generar una excepción: definir su propia interfaz funcional que declara la excepción marcada o envolver el cuerpo lambda con un bloque **`try/catch`**.

Por ejemplo, en la sección 3.3 introdujimos una nueva interfaz funcional **`Buffered-Reader-Processor`** que declaraba explícitamente **`IOException`**:

```java
@FunctionalInterface
public interface BufferedReaderProcessor {
    String process(BufferedReader b) throws IOException;
}
BufferedReaderProcessor p = (BufferedReader br) -> br.readLine();
```

Pero es posible que esté utilizando una API que espera una interfaz funcional **`Function<T, R>`** y no hay opción para crear la suya propia. Verá en el próximo capítulo que la **API Streams** hace un uso intensivo de las interfaces funcionales de la tabla 3.2. En este caso, puede detectar explícitamente la excepción marcada:

```java
Function<BufferedReader, String> f =
  (BufferedReader b) -> {
    try {
      return b.readLine();
    }
    catch(IOException e) {
      throw new RuntimeException(e);
    }
  };
```

Ahora ha visto cómo crear lambdas y dónde y cómo usarlas. A continuación, explicaremos algunos detalles más avanzados: cómo el compilador verifica el tipo de lambdas y las reglas que debe tener en cuenta, como las lambdas que hacen referencia a variables locales dentro de su cuerpo y las lambdas compatibles con void. No es necesario comprender completamente la siguiente sección de inmediato y es posible que desee volver a ella más tarde y pasar a la sección 3.6 sobre referencias de métodos.

## 3.5. Comprobación de tipos, inferencia de tipos y restricciones

Cuando mencionamos por primera vez las expresiones lambda, dijimos que le permiten generar una instancia de una interfaz funcional. No obstante, una expresión lambda en sí misma no contiene información sobre qué interfaz funcional está implementando. Para tener una comprensión más formal de las expresiones lambda, debes saber cuál es el tipo de lambda.

### 3.5.1. Type checking - Comprobación de tipo

El tipo de lambda se deduce del contexto en el que se utiliza la lambda. El tipo esperado para la expresión lambda dentro del contexto (por ejemplo, un parámetro de método al que se pasa o una variable local a la que está asignada) se denomina tipo de destino. Veamos un ejemplo para ver qué sucede detrás de escena cuando usa una expresión lambda. La Figura 3.4 resume el proceso de verificación de tipo para el siguiente código:

**Figura 3.4. Deconstruyendo el proceso de verificación de tipos de una expresión lambda**

![image](https://github.com/adolfodelarosades/Java/assets/23094588/b9a6f9c2-4b5b-4bbb-a9fe-c43e671e20f0)


```java
List<Apple> heavierThan150g =
        filter(inventory, (Apple apple) -> apple.getWeight() > 150);
```

El proceso de verificación de tipo se deconstruye de la siguiente manera:

* Primero, busca la declaración del método **`filter`**.
* En segundo lugar, espera, como segundo parámetro formal, un objeto de tipo **`Predicate<Apple>`**(el target type - tipo objetivo).
* En tercer lugar, **`Predicate<Apple>`** es una interfaz funcional que define un único método abstracto llamado **`test`**.
* Cuarto, el método **`test`** describe un descriptor de función que acepta un **`Apple`** y devuelve un **`boolean`**.
* Finalmente, cualquier argumento del método **`filter`** debe cumplir con este requisito.

El código es válido porque la expresión lambda que estamos pasando también toma un parámetro **`Apple`** y devuelve un **`boolean`**. Tenga en cuenta que si la expresión lambda arrojara una excepción, entonces la cláusula **`throws`** declarada del método abstracto también tendría que coincidir.

### 3.5.2. Misma lambda, diferentes interfaces funcionales

Debido a la idea de tipificación de destino, la misma expresión lambda se puede asociar con diferentes interfaces funcionales si tienen una firma de método abstracto compatible. Por ejemplo, ambas interfaces **`Callable`** y **`PrivilegedAction`** las descritas anteriormente representan funciones que no aceptan nada y devuelven un tipo genérico **`T`**. Por tanto, son válidas las dos asignaciones siguientes:

```java
Callable<Integer> c = () -> 42;
PrivilegedAction<Integer> p = () -> 42;
```

En este caso, la primera asignación tiene un tipo de destino **`Callable<Integer>`** y la segunda asignación tiene un tipo de destino **`PrivilegedAction<Integer>`**.

En la tabla 3.3 mostramos un ejemplo similar; la misma lambda se puede utilizar con múltiples interfaces funcionales diferentes:

```java
Comparator<Apple> c1 =
  (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
ToIntBiFunction<Apple, Apple> c2 =
  (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
BiFunction<Apple, Apple, Integer> c3 =
  (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());
```

#### Diamond operator

Aquellos de ustedes que estén familiarizados con la evolución de Java recordarán que Java 7 ya había introducido la idea de que los tipos se infieren a partir del contexto con inferencia genérica usando el operador diamante (**`<>`**) (esta idea se puede encontrar incluso antes con métodos genéricos). Una expresión de instancia de clase dada puede aparecer en dos o más contextos diferentes, y el argumento de tipo apropiado se inferirá como se ejemplifica aquí:

```java
List<String> listOfStrings = new ArrayList<>();
List<Integer> listOfIntegers = new ArrayList<>();
```

#### Regla especial void-compatibility

Si una lambda tiene una expresión de declaración como su body, es compatible con un descriptor de función que devuelve **`void`**(siempre que la lista de parámetros también sea compatible). Por ejemplo, las dos líneas siguientes son legales aunque el método **`add`** de una **`List`** devuelva un **`boolean`** y no **`void`** como se esperaba en el **` Consumer context (T -> void)`**:

```java
// Predicate has a boolean return
Predicate<String> p = (String s) -> list.add(s);
// Consumer has a void return
Consumer<String> b = (String s) -> list.add(s);
```

A estas alturas ya debería tener una buena comprensión de cuándo y dónde se le permite usar expresiones lambda. Pueden obtener su tipo de destino a partir de un contexto de asignación, un contexto de invocación de método (parámetros y retorno) y un contexto de conversión. Para comprobar sus conocimientos, pruebe el cuestionario 3.5.

**Prueba 3.5: Comprobación de tipos: ¿por qué no se compila el siguiente código?**

¿Cómo podrías solucionar el problema?

```java
Object o = () -> { System.out.println("Tricky example"); };
```

**Respuesta:**

El contexto de la expresión lambda es **`Object`**(el target type - tipo de destino). Pero **`Object`** no es una interfaz funcional. Para solucionar este problema, puede cambiar el tipo de destino a **`Runnable`**, que representa un descriptor de función **`() -> void`**:

```java
Runnable r = () -> { System.out.println("Tricky example"); };
```

También puede solucionar el problema convirtiendo la expresión lambda en **`Runnable`**, que proporciona explícitamente un target type - tipo de destino.

```java
Object o = (Runnable) () -> { System.out.println("Tricky example"); };
```

Esta técnica puede resultar útil en el contexto de la sobrecarga con un método que toma dos interfaces funcionales diferentes que tienen el mismo descriptor de función. Puede convertir la lambda para eliminar explícitamente qué firma de método se debe seleccionar.

Por ejemplo, la llamada **`execute(() -> {})`** usando el método **`execute`**, como se muestra a continuación, sería ambigua, porque ambos **`Runnable`** y **`Action`** tienen el mismo function descriptor - descriptor de función:

```java
public void execute(Runnable runnable) {
    runnable.run();
}
public void execute(Action<T> action) {
    action.act();
}
@FunctionalInterface
interface Action {
    void act();
}
```

Pero puedes eliminar explícitamente la ambigüedad de la llamada usando una expresión de conversión: **`execute ((Action) () -> {});`**

Ha visto cómo se puede usar el tipo de destino para verificar si una lambda se puede usar en un contexto particular. También se puede utilizar para hacer algo ligeramente diferente: inferir los tipos de parámetros de una lambda.

### 3.5.3. Inferencia de tipos

Puede simplificar su código un paso más. El compilador de Java deduce qué interfaz funcional asociar con una expresión lambda a partir de su contexto circundante (el tipo de destino), lo que significa que también puede deducir una firma apropiada para la lambda porque el descriptor de función está disponible a través del tipo de destino. El beneficio es que el compilador tiene acceso a los tipos de parámetros de una expresión lambda y se pueden omitir en la sintaxis lambda. El compilador de Java infiere los tipos de parámetros de una lambda como se muestra aquí: [ 3 ]

<hr>

**3**

*Tenga en cuenta que cuando una lambda tiene un único parámetro cuyo tipo se infiere, los paréntesis que rodean el nombre del parámetro también se pueden omitir.*

<hr>

```java
List<Apple> greenApples =
        filter(inventory, apple -> GREEN.equals(apple.getColor()));     1
```

**1 No hay tipo explícito en el parámetro apple.**

Los beneficios de la legibilidad del código son más notorios con expresiones lambda que tienen varios parámetros. Por ejemplo, aquí se explica cómo crear un objeto **`Comparator`**:

```java
Comparator<Apple> c =
  (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight());      1
Comparator<Apple> c =
  (a1, a2) -> a1.getWeight().compareTo(a2.getWeight());                  2
```

* **1 Sin inferencia de tipos**
* **2 Con inferencia de tipos**

Tenga en cuenta que a veces es más legible incluir los tipos explícitamente y, a veces, es más legible excluirlos. No existe una regla sobre cuál es mejor; Los desarrolladores deben tomar sus propias decisiones sobre qué hace que su código sea más legible.

### 3.5.4. Usando variables locales

Todas las expresiones lambda que hemos mostrado hasta ahora utilizaron sólo sus argumentos dentro de su cuerpo. Pero las expresiones lambda también pueden usar ***free variables - variables libres*** (variables que no son parámetros y están definidas en un ámbito externo) como pueden hacerlo las clases anónimas. Se llaman ***capturing lambdas - capturar lambdas***. Por ejemplo, la siguiente lambda captura la variable por **`portNumber`**:

```java
int portNumber = 1337;
Runnable r = () -> System.out.println(portNumber);
```

Sin embargo, hay un pequeño giro. Existen algunas restricciones sobre lo que puede hacer con estas variables. Las lambdas pueden capturar (hacer referencia en sus bodies) variables de instancia y variables estáticas sin restricciones. Pero cuando se capturan variables locales, deben declararse explícitamente **`final`** o ser efectivas **`final`**. Las expresiones lambda pueden capturar variables locales que se asignan solo una vez. (Nota: capturar una variable de instancia puede verse como capturar la final local variable **`this`**). Por ejemplo, el siguiente código no se compila porque la variable **`portNumber`** se asigna dos veces:

```java
int portNumber = 1337;
Runnable r = () -> System.out.println(portNumber);         1
portNumber = 31337;
```

* **1 Error: la variable local portNumber no es final o efectivamente final.**

#### Restricciones a las variables locales.

Quizás se pregunte por qué las variables locales tienen estas restricciones. Primero, hay una diferencia clave en cómo se implementan las variables locales y de instancia detrás de escena. Las variables de instancia se almacenan en el **heap**, mientras que las variables locales viven en el **stack**. Si un lambda pudiera acceder a la variable local directamente y el lambda se usara en un thread - hilo, entonces el thread - hilo que usa el lambda podría intentar acceder a la variable después del thread - hilo que asignó la variable la había desasignado(then the thread using the lambda could try to access the variable after the thread that allocated the variable had deallocated it). Por lo tanto, Java implementa el acceso a una variable local libre como acceso a una copia de la misma, en lugar de acceso a la variable original. Esto no hace ninguna diferencia si la variable local se asigna solo una vez; de ahí la restricción.

En segundo lugar, esta restricción también desalienta los típicos patrones de programación imperativo (que, como explicamos en capítulos posteriores, impiden la paralelización fácil) que mutan una variable externa.

#### Closure

Es posible que haya oído hablar del término ***closure*** y se pregunte si las lambdas cumplen con la definición de closure (que no debe confundirse con el lenguaje de programación **Clojure**). Para decirlo científicamente, un closure es una instancia de una función que puede hacer referencia a variables no locales de esa función sin restricciones. Por ejemplo, un closure podría pasarse como argumento a otra función. También podría *acceder* y *modificar* variables definidas fuera de su alcance. Ahora, las lambdas y las clases anónimas de Java 8 hacen algo similar a los closure: se pueden pasar como argumento a métodos y pueden acceder a variables fuera de su alcance. Pero tienen una restricción: no pueden modificar el contenido de las variables locales de un método en el que está definida la lambda. Esas variables tienen que ser implícitamente final. Ayuda pensar que las lambdas cierran sobre *valores* en lugar de *variables*. Como se explicó anteriormente, esta restricción existe porque las variables locales viven en la pila - stack y están implícitamente confinadas al subproceso en el que se encuentran. Permitir la captura de variables locales mutables abre nuevas posibilidades no seguras para subprocesos, que no son deseables (las variables de instancia están bien porque viven en en el heap, que se comparte entre subprocesos).

Ahora describiremos otra gran característica que se introdujo en el código Java 8: ***referencias de métodos***. Piense en ellos como versiones abreviadas de determinadas lambdas.

## 3.6. Method references - Referencias de métodos

Las referencias a métodos le permiten reutilizar definiciones de métodos existentes y pasarlas como lambdas. En algunos casos, parecen más legibles y se sienten más naturales que usar expresiones lambda. Aquí está nuestro ejemplo de clasificación escrito con una referencia de método y un poco de ayuda de la API de Java 8 actualizada (exploramos este ejemplo con más detalle en la sección 3.7 ).

Antes:

```java
inventory.sort((Apple a1, Apple a2)
a1.getWeight().compareTo(a2.getWeight()));
```

Después (usando una referencia de método y **`java.util.Comparator.comparing`**):

```java
inventory.sort(comparing(Apple::getWeight));          1
```

* **1 Su primera referencia de método**

No se preocupe por la nueva sintaxis y cómo funcionan las cosas. ¡Aprenderás eso en las próximas secciones!

### 3.6.1. En una palabra

¿Por qué debería importarle las referencias de métodos? Las referencias a métodos pueden verse como una abreviatura de lambdas que llaman solo a un método específico. La idea básica es que si una lambda representa "llamar a este método directamente", es mejor referirse al método por su nombre en lugar de por una descripción de cómo llamarlo. De hecho, una referencia de método le permite crear una expresión lambda a partir de la implementación de un método existente. Pero al hacer referencia explícita al nombre de un método, su código puede obtener una mejor legibilidad. ¿Como funciona? Cuando necesita una referencia de método, la referencia de destino se coloca antes del delimitador **`::`** y el nombre del método se proporciona después. Por ejemplo, **`Apple::getWeight`** es una referencia de método al método **`getWeight`** definido en la clase **`Apple`**. (Recuerde que no se necesitan corchetes después **`getWeight`** porque no lo está llamando en este momento, simplemente está citando su nombre). Esta referencia de método es una abreviatura de la expresión lambda **`(Apple apple) -> apple.getWeight()`**. La Tabla 3.4 ofrece algunos ejemplos más de posibles referencias a métodos en Java 8.

**Tabla 3.4. Ejemplos de lambdas y equivalentes de referencia de métodos**

![image](https://github.com/adolfodelarosades/Java/assets/23094588/e27a4b98-5356-4dea-a3ea-caf9188e4b09)


Puede pensar en las referencias a métodos como azúcar sintáctico para lambdas que se refieren solo a un método porque escribe menos para expresar lo mismo.

#### Receta para construir referencias de métodos.

Hay tres tipos principales de referencias de métodos:

1. Una referencia de método a un método estático (por ejemplo, el método **`parseInt`** de **`Integer`**, escrito **`Integer::parseInt`**)
2. Una referencia de método a un método de instancia de un tipo arbitrario (por ejemplo, el método **`length`** de a **`String`**, escrito **`String::length`**)
3. Una referencia de método a un *método de instancia de un objeto o expresión existente* (por ejemplo, supongamos que tiene una variable local **`expensiveTransaction`** que contiene un objeto de tipo **`Transaction`**, que admite un método de instancia **`getValue`**; puede escribir **`expensiveTransaction::getValue`**)

El segundo y tercer tipo de referencias a métodos pueden resultar un poco abrumadores al principio. La idea con el segundo tipo de referencias a métodos, como **`String::length`**, es que estás haciendo referencia a un método para un objeto que se proporcionará como uno de los parámetros de lambda. Por ejemplo, la expresión lambda **`(String s) -> s.toUpperCase()`** se puede reescribir como **`String::toUpperCase`**. Pero el tercer tipo de referencia de método se refiere a una situación en la que llamas a un método en una lambda a un objeto externo que ya existe. Por ejemplo, la expresión lambda **`() -> expensiveTransaction.getValue()`** se puede reescribir como **`expensiveTransaction::getValue`**. Este tercer tipo de referencia de método es particularmente útil cuando necesita pasar un método definido como ayudante privado. Por ejemplo, digamos que definiste un método auxiliar **`isValidName`**:

```java
private boolean isValidName(String string) {
    return Character.isUpperCase(string.charAt(0));
}
```

Ahora puede pasar este método en el contexto de una **`Predicate<String>`** referencia de método:

```java
filter(words, this::isValidName)
```

Para ayudarle a digerir este nuevo conocimiento, las reglas abreviadas para refactorizar una expresión lambda a una referencia de método equivalente siguen recetas simples, como se muestra en la figura 3.5.

**Figura 3.5. Recetas para construir referencias de métodos para tres tipos diferentes de expresiones lambda**

![image](https://github.com/adolfodelarosades/Java/assets/23094588/0d281bfc-63d0-4e6e-813a-e0c5b6bc4b3d)


Tenga en cuenta que también existen formas especiales de referencias de métodos para constructores, constructores de matrices y superllamadas. Apliquemos referencias de métodos en un ejemplo concreto. Supongamos que desea ordenar una **`List`** de strings, ignorando las diferencias entre mayúsculas y minúsculas. El método **`sort`** en una **`List`** espera un **`Comparator`** como parámetro. Viste anteriormente que **`Comparator`** describe un descriptor de función con la firma **`(T, T) -> int`**. Puede definir una expresión lambda que utilice el método **`compareToIgnoreCase`** en la clase **`String`** de la siguiente manera (tenga en cuenta que **`compareToIgnoreCase`** está predefinida en la clase **`String`**):

```java
List<String> str = Arrays.asList("a","b","A","B");
str.sort((s1, s2) -> s1.compareToIgnoreCase(s2));
```

La expresión lambda tiene una firma compatible con el descriptor de función de **`Comparator`**. Utilizando las recetas descritas anteriormente, el ejemplo también se puede escribir utilizando una referencia de método; Esto da como resultado un código más conciso, como sigue:

```java
List<String> str = Arrays.asList("a","b","A","B");
str.sort(String::compareToIgnoreCase);
```

Tenga en cuenta que el compilador pasa por un proceso de verificación de tipos similar al de las expresiones lambda para determinar si una referencia de método es válida con una interfaz funcional determinada. La firma de la referencia del método debe coincidir con el tipo de contexto.

Para comprobar su comprensión de las referencias de métodos, ¡pruebe el cuestionario 3.6!

**Prueba 3.6: Method references - Referencias de métodos**

¿Cuáles son las referencias de métodos equivalentes para las siguientes expresiones lambda?

**1.**

```java
ToIntFunction<String> stringToInt =
               (String s) -> Integer.parseInt(s);
```

**2.**

```java
BiPredicate<List<String>, String> contains =
               (list, element) -> list.contains(element);
```

**3.**

```java
Predicate<String> startsWithNumber =
               (String string) -> this .starts-WithNumber(string);
```

**Respuestas:**

1. Esta expresión lambda reenvía su argumento al método estático **`parseInt`** de **`Integer`**. Este método toma un **`String`** para parsear y retorna un **`int`**. Como resultado, la lambda se puede reescribir usando la receta ***1*** de la figura 3.5 (expresiones lambda que llaman a un método estático) de la siguiente manera:

```java
ToIntFunction<String> stringToInt = Integer::parseInt;
```

2. Esta lambda usa su primer argumento para llamar al método **`contains`**. Como el primer argumento es de tipo **`List`**, puedes usar la receta ***2*** de la figura 3.5 de la siguiente manera:

```java
BiPredicate<List<String>, String> contains = List::contains;
```

Esto se debe a que el target type describe un descriptor de función **`(List<String>, String) -> boolean, and List::contains`** se puede descomprimir en ese descriptor de función.

Esta lambda de estilo expresión invoca un método auxiliar privado. Puede utilizar la receta ***3*** de la figura 3.5 de la siguiente manera:

```java
Predicate<String> startsWithNumber = this::startsWithNumber
```
Solo hemos mostrado cómo reutilizar implementaciones de métodos existentes y crear referencias de métodos. Pero puedes hacer algo similar con los constructores de una clase.

### 3.6.2. Constructor references - Referencias de constructores

Puede crear una referencia a un constructor existente utilizando su nombre y la palabra clave **`new`** de la siguiente manera: **`ClassName::new`**. Funciona de manera similar a una referencia a un método estático. Por ejemplo, supongamos que hay un constructor sin argumentos. Esto encaja con la firma **`() -> Apple`** de **`Supplier`**; puedes hacer lo siguiente:

```java
Supplier<Apple> c1 = Apple::new;          1
Apple a1 = c1.get();                      2
```

* **1 Constructor reference al constructor predeterminado `Apple()`**
* **2Llamar al método get del proveedor produce una nueva `Apple`.**

que es equivalente a

```java
Supplier<Apple> c1 = () -> new Apple();         1
Apple a1 = c1.get();                            2
```

* **1 Expresión Lambda para crear un `Apple` usando el constructor default**
* **2 Llamar al método `get` del proveedor produce una nueva `Apple`**

Si tiene un constructor con firma **`Apple(Integer weight)`**, se ajusta a la firma de la interfaz **`Function`**, entonces puede hacer esto

```java
Function<Integer, Apple> c2 = Apple::new;        1
Apple a2 = c2.apply(110);                        2
```

* **1 Constructor reference - Referencia del constructor a `Apple (Integer weight)`**
* **2 Llamar al método `apply` de la función con un weight(peso) determinado produce una `Apple`**

que es equivalente a

```java
Function<Integer, Apple> c2 = (weight) -> new Apple(weight);     1
Apple a2 = c2.apply(110);                                        2
```
                                        
* **1 Expresión Lambda para crear una `Apple` con un weight(peso) determinado**
* **2 Llamar al método `apply` de la función con un weight(peso) determinado produce un nuevo objeto `Apple`**

En el siguiente código, cada elemento de a **`List`** de **`Integers`** se pasa al constructor de **`Apple`** usando un método **`map`** similar al que definimos anteriormente, lo que da como resultado un **`List`** de apples con varios weights(pesos):

```java
List<Integer> weights = Arrays.asList(7, 3, 4, 10);
List<Apple> apples = map(weights, Apple::new);                            1
public List<Apple> map(List<Integer> list, Function<Integer, Apple> f) {
    List<Apple> result = new ArrayList<>();
    for(Integer i: list) {
        result.add(f.apply(i));
    }
    return result;
}
```

* **1 Pasar una referencia de constructor al método `map`**

Si tiene un constructor de dos argumentos, **`Apple (Color color, Integer weight)`** se ajusta a la firma de la interfaz **`BiFunction`**, por lo que puede hacer esto:

```java
BiFunction<Color, Integer, Apple> c3 = Apple::new;      1
Apple a3 = c3.apply(GREEN, 110);                        2
```

* **1 Constructor reference - Referencia del constructor a `Apple (Color color, Integer weight)`**
* **2 El método `apply` de `BiFunction` con un color y weight(peso) determinados produce un nuevo objeto `Apple`.**

que es equivalente a

```java
BiFunction<String, Integer, Apple> c3 =
    (color, weight) -> new Apple(color, weight);        1
Apple a3 = c3.apply(GREEN, 110);                        2
```

* **1 Expresión Lambda para crear una `Apple` con un color y weight(peso) determinados**
* **2 El método `apply` de `BiFunction` con un color y weight(peso) determinados produce un nuevo objeto `Apple`.**

La capacidad de hacer referencia a un constructor sin crear una instancia del mismo permite aplicaciones interesantes. Por ejemplo, puede utilizar a **`Map`** para asociar constructores con un valor string. Luego puedes crear un método **`giveMeFruit`** que, dados a **`String`** y a **`Integer`**, pueda crear diferentes tipos de frutas con diferentes pesos, de la siguiente manera:

```java
static Map<String, Function<Integer, Fruit>> map = new HashMap<>();
static {
    map.put("apple", Apple::new);
    map.put("orange", Orange::new);
    // etc...
}
public static Fruit giveMeFruit(String fruit, Integer weight){
    return map.get(fruit.toLowerCase())                              1
              .apply(weight);                                        2
}
```

* **1 Obtenga una función `<Integer, Fruit>` desde el `map`**
* **2 El método `apply` de la función con un parámetro `Integer weight` crea la fruta solicitada.**
  
Para comprobar su comprensión de las referencias de métodos y constructores, pruebe la prueba 3.7.

**Prueba 3.7: Constructor references - Referencias de constructores**

Viste cómo transformar constructores de cero, uno y dos argumentos en referencias de constructores. ¿Qué necesitaría hacer para utilizar una referencia de constructor para un constructor de tres argumentos como **`RGB(int, int, int)`**?

**Respuesta:**

Viste que la sintaxis para una referencia de constructor es **`ClassName::new`**, por lo que en este caso es **`RGB::new`**. Pero necesita una interfaz funcional que coincida con la firma de esa referencia del constructor. Como no hay ninguna en el conjunto inicial de interfaz funcional, puedes crear la tuya propia:

```java
public interface TriFunction<T, U, V, R> {
    R apply(T t, U u, V v);
}
```

Y ahora puedes usar la referencia del constructor de la siguiente manera:

```java
TriFunction<Integer, Integer, Integer, RGB> colorFactory = RGB::new;
```

Hemos analizado mucha información nueva: lambdas, interfaces funcionales y referencias de métodos. Lo pondremos todo en práctica en la siguiente sección.

## 3.7. Poner en práctica lambdas y referencias de métodos

Para concluir este capítulo y nuestra discusión sobre lambdas, continuaremos con nuestro problema inicial de ordenar una lista de **`Apples`** con diferentes estrategias de ordenamiento. Y mostraremos cómo se puede evolucionar progresivamente una solución ingenua hacia una solución concisa, utilizando todos los conceptos y características explicados hasta ahora en el libro: parametrización de comportamiento, clases anónimas, expresiones lambda y referencias de métodos. La solución final en la que trabajaremos es la siguiente (tenga en cuenta que todo el código fuente está disponible en el sitio web del libro: www.manning.com/books/modern-java-in-action ):

```java
inventory.sort(comparing(Apple::getWeight));
```

### 3.7.1. Paso 1: Pass code - Código de acceso

Tienes suerte; La API de Java 8 ya le proporciona un método **`sort`** disponible para **`List`** que no tenga que implementarlo. ¡La parte más difícil ya está hecha! Pero, ¿cómo se puede pasar una estrategia de pedido al método **`sort`**? Bueno, el método **`sort`** tiene la siguiente firma:

```java
void sort(Comparator<? super E> c)
```

¡Espera un objeto **`Comparator`** como argumento para comparar dos **`Apple`**s! Así es como puedes pasar diferentes estrategias en Java: deben estar envueltas en un objeto. Decimos que el comportamiento de **`sort`** está parametrizado : su comportamiento será diferente según las diferentes estrategias de ordenación que se le pasen.

Su primera solución se ve así:

```java
public class AppleComparator implements Comparator<Apple> {
        public int compare(Apple a1, Apple a2){
                return a1.getWeight().compareTo(a2.getWeight());
        }
}
inventory.sort(new AppleComparator());
```

### 3.7.2. Paso 2: Usa una clase anónima

En lugar de implementar **`Comparator`** con el propósito de crear una instancia una vez, vio que podía usar una clase anónima para mejorar su solución:

```java
inventory.sort(new Comparator<Apple>() {
    public int compare(Apple a1, Apple a2){
        return a1.getWeight().compareTo(a2.getWeight());
    }
});
```

### 3.7.3. Paso 3: Use expresiones lambda

Pero su solución actual sigue siendo detallada. Java 8 introdujo expresiones lambda, que proporcionan una sintaxis ligera para lograr el mismo objetivo: ***passing code pasar código***. Viste que se puede usar una expresión lambda donde se espera una ***functional interface - interfaz funcional***. **Como recordatorio, una interfaz funcional es una interfaz que define solo un método abstracto**. La firma del método abstracto (llamado ***function descriptor - descriptor de función***) puede describir la firma de una expresión lambda. En este caso, **`Comparator`** representa un descriptor de función **`(T, T) -> int`**. Debido a que estás usando **`Apple`**s, representa más específicamente **`(Apple, Apple) -> int`**. Por lo tanto, su nueva solución mejorada tiene el siguiente aspecto:

```java
inventory.sort((Apple a1, Apple a2)
                -> a1.getWeight().compareTo(a2.getWeight())
);
```

Explicamos que el compilador de Java podría ***inferir los tipos*** de parámetros de una expresión lambda utilizando el contexto en el que aparece la lambda. Por lo tanto, puede reescribir su solución de la siguiente manera:

```java
inventory.sort((a1, a2) -> a1.getWeight().compareTo(a2.getWeight()));
```

¿Puedes hacer que tu código sea aún más legible? **`Comparator`** incluye un método auxiliar estático(static helper) llamado **`comparing`** que extrae **`Function`** una **`Comparable`** key y produce un objeto **`Comparator`** (explicamos por qué las interfaces pueden tener métodos estáticos en el capítulo 13). Se puede utilizar de la siguiente manera (tenga en cuenta que ahora pasa una lambda con un solo argumento; la lambda especifica cómo extraer la clave para comparar desde un **`Apple`**):

```java
Comparator<Apple> c = Comparator.comparing((Apple a) -> a.getWeight());
```

Ahora puedes reescribir tu solución en una forma un poco más compacta:

```java
import static java.util.Comparator.comparing;
inventory.sort(comparing(apple -> apple.getWeight()));
```

### 3.7.4. Paso 4: Utilice method references - referencias de métodos

Explicamos que las referencias a métodos son azúcar sintáctico para expresiones lambda que reenvían sus argumentos. Puede utilizar una referencia de método para hacer que su código sea un poco menos detallado (suponiendo una importación estática de **`java.util.Comparator.comparing`**):

```java
inventory.sort(comparing(Apple::getWeight));
```

¡Felicitaciones, esta es su solución final! ¿Por qué es mejor que el código anterior a Java 8? No es sólo porque es más corto; También es obvio lo que significa. El código se lee como el enunciado del problema "clasificar el inventario comparando el peso de las manzanas".

## 3.8. Métodos útiles para componer expresiones lambda.

Varias interfaces funcionales en la API de Java 8 contienen métodos convenientes. Específicamente, muchas interfaces funcionales como **`Comparator`**, **`Function`** y **`Predicate`** que se utilizan para pasar expresiones lambda proporcionan métodos que permiten la composición. ¿Qué quiere decir esto? En la práctica, significa que puedes combinar varias expresiones lambda simples para construir otras más complicadas. Por ejemplo, puede combinar dos predicados en un predicado más grande que realice una operación **`or`** entre los dos predicados. Además, también puede componer funciones de modo que el resultado de una se convierta en la entrada de otra función. Quizás te preguntes cómo es posible que existan métodos adicionales en una interfaz funcional. (¡Después de todo, esto va en contra de la definición de una interfaz funcional!) El truco es que los métodos que introduciremos se llaman métodos predeterminados (no son métodos abstractos). Los explicamos detalladamente en el capítulo 13. Por ahora, confíe en nosotros y lea el capítulo 13 más adelante, cuando quiera saber más sobre los métodos predeterminados y lo que puede hacer con ellos.

### 3.8.1. Componer Comparators

Has visto que puedes usar el método estático **`Comparator.comparing`** para devolver un **`Comparator`** basado en un **`Function`** que extrae una clave para comparar de la siguiente manera:

```java
Comparator<Apple> c = Comparator.comparing(Apple::getWeight);
```

#### Reversed order - Orden invertido

¿Qué pasaría si quisieras clasificar las manzanas disminuyendo de peso? No es necesario crear una instancia diferente de un **`Comparator`**. La interfaz incluye un método predeterminado **`reversed`** que invierte el orden de un comparador determinado. Puedes modificar el ejemplo anterior para ordenar las manzanas disminuyendo el peso reutilizando el inicial **`Comparator`**:

```java
inventory.sort(comparing(Apple::getWeight).reversed());         1
```

* **1 Ordena por peso decreciente**

#### Chaining Comparators - Comparators de encadenamiento

Todo esto está bien, pero ¿qué pasa si encuentras dos manzanas que tienen el mismo peso? ¿Qué manzana debería tener prioridad en la lista ordenada? Es posible que desee brindar un segundo **`Comparator`** - para refinar aún más la comparación. Por ejemplo, después de comparar dos manzanas según su peso, es posible que desees ordenarlas por país de origen. El método **`thenComparing`** te permite hacer eso. Toma una función como parámetro (como el método **`comparing`**) y proporciona una segunda **`Comparator`** si dos objetos se consideran iguales usando el inicial **`Comparator`**. Puedes resolver el problema elegantemente nuevamente de la siguiente manera:

```java
inventory.sort(comparing(Apple::getWeight)
         .reversed()                                  1
         .thenComparing(Apple::getCountry));          2
```

* **1 Ordena por peso decreciente**
* **2 Clasifica mejor por país cuando dos manzanas tienen el mismo peso**

### 3.8.2.Componer Predicates

La interfaz **`Predicate`** incluye tres métodos que le permiten reutilizar un **`Predicate`** existente para crear otros más complicados: **`negate`**, **`and`** y **`or`**. Por ejemplo, puedes usar el método **`negate`** para devolver la negación de a **`Predicate`**, como una manzana que no es roja:

```java
Predicate<Apple> notRedApple = redApple.negate();           1
```      

* **1 Produce la negación del existente `Predicate` del objeto `redApple`**

Es posible que desees combinar dos lambdas para decir que una manzana es roja y pesada con el método **`and`**:

```java
Predicate<Apple> redAndHeavyApple =
    redApple.and(apple -> apple.getWeight() > 150);           1
```

* **1 Encadena dos predicates para producir otro objeto `Predicate`**

Puedes combinar el predicado resultante un paso más para expresar manzanas rojas y pesadas (más de 150 g) o solo manzanas verdes:

```java
Predicate<Apple> redAndHeavyAppleOrGreen =
    redApple.and(apple -> apple.getWeight() > 150)
            .or(apple -> GREEN.equals(a.getColor()));          1
```

* **1 Encadena tres predicates para construir un objeto `Predicate` más complejo**

¿Por qué es esto genial? A partir de expresiones lambda más simples, puede representar expresiones lambda más complicadas que aún se leen como el enunciado del problema. Tenga en cuenta que la precedencia de los métodos **`and`** y **`or`** en la cadena es de izquierda a derecha; no existe ningún equivalente al uso de corchetes. Por lo tanto **`a.or(b).and(c)`** debe leerse como **`(a || b) && c`**. Del mismo modo, **`a.and(b).or(c)`** debe leerse como **`(a && b) || c`**.

### 3.8.3. Componer Functions

Finalmente, también puedes componer expresiones lambda representadas por la interfaz **`Function`**. La interfaz **`Function`** viene con dos métodos predeterminados para esto **`andThen`** y **`compose`**, los cuales devuelven una instancia de **`Function`**.

El método **`andThen`** devuelve una función que primero aplica una función determinada a una entrada y luego aplica otra función al resultado de esa aplicación. Por ejemplo, dada una función **`f`** que incrementa un número **`(x -> x + 1)`** y otra función **`g`** que multiplica un número por **`2`**, puedes combinarlas para crear una función **`h`** que primero incrementa un número y luego multiplica el resultado por **`2`**:

```java
Function<Integer, Integer> f = x -> x + 1;
Function<Integer, Integer> g = x -> x * 2;
Function<Integer, Integer> h = f.andThen(g);     1
int result = h.apply(1);                         2
```

* **1 En matemáticas escribirías `g(f(x))` or `(g o f)(x)`**.
* **2 Esto devuelve `4`.**

También puede utilizar el método **`compose`** de manera similar para aplicar primero la función dada como argumento **`compose`** y luego aplicar la función al resultado. Por ejemplo, en el ejemplo anterior, usar **`compose`**, significaría **`f(g(x))`** en lugar de **`g(f(x))`** usar **`andThen`**:

```java
Function<Integer, Integer> f = x -> x + 1;
Function<Integer, Integer> g = x -> x * 2;
Function<Integer, Integer> h = f.compose(g);     1
int result = h.apply(1);                         2
```

* **1 En matemáticas escribirías `f(g(x))` o `(f o g)(x)`**.
* **2 Esto devuelve `3`.**


La figura 3.6 ilustra la diferencia entre `andThen` y `compose`.

**Figura 3.6. Usando `andThen` versus `compose`**

![image](https://github.com/adolfodelarosades/Java/assets/23094588/50f7028d-363a-4cea-875d-e84205059761)


Todo esto suena demasiado abstracto. ¿Cómo puedes utilizarlos en la práctica? Digamos que tiene varios métodos de utilidad que realizan transformaciones de texto en una letra representada como **`String`**:

```java
public class Letter{
    public static String addHeader(String text) {
        return "From Raoul, Mario and Alan: " + text;
    }
    public static String addFooter(String text) {
        return text + " Kind regards";
    }
    public static String checkSpelling(String text) {
        return text.replaceAll("labda", "lambda");
    }
}
```

Ahora puede crear varios canales de transformación componiendo los métodos de utilidad. Por ejemplo, crear una canalización que primero agregue un encabezado, luego revise la ortografía y finalmente agregue un pie de página, como se muestra a continuación (y como se ilustra en la figura 3.7 ):

**Figura 3.7. Un proceso de transformación que utiliza `andThen`**

![image](https://github.com/adolfodelarosades/Java/assets/23094588/a343546a-90b7-44b7-9c44-b59d9e66f822)


```java
Function<String, String> addHeader = Letter::addHeader;
Function<String, String> transformationPipeline
  = addHeader.andThen(Letter::checkSpelling)
             .andThen(Letter::addFooter);
```

Una segunda canalización podría consistir únicamente en agregar un encabezado y un pie de página sin revisar la ortografía:

```java
Function<String, String> addHeader = Letter::addHeader;
Function<String, String> transformationPipeline
  = addHeader.andThen(Letter::addFooter);
```

## 3.9. Ideas similares de matemáticas

Si se siente cómodo con las matemáticas de la escuela secundaria, esta sección le brinda otro punto de vista sobre la idea de las expresiones lambda y las funciones de transferencia. Siéntete libre de omitirlo; nada más en el libro depende de ello. Pero es posible que disfrute viendo otra perspectiva.

### 3.9.1. Integración

Supongamos que tiene una función **`f`** (matemática, no Java), quizás definida por

```java
f(x) = x + 10
```

Entonces, una pregunta que se hace a menudo (en la escuela y en las carreras de ciencias e ingeniería) es encontrar el área debajo de la función cuando se dibuja en papel (contando el eje x como la línea cero). Por ejemplo, escribes

![image](https://github.com/adolfodelarosades/Java/assets/23094588/d2648f6d-17b6-4af7-bf0a-3a22ec7475d7)

para el área que se muestra en la figura 3.8.

**Figura 3.8. Área bajo la función `f(x) = x + 10` de `x` desde `3` a `7`**

![image](https://github.com/adolfodelarosades/Java/assets/23094588/67e7b699-940c-4932-bcb4-15b5cf16dcea)


En este ejemplo, la función **`f`** es una línea recta, por lo que puedes calcular fácilmente esta área mediante el método del trapecio (dibujando triángulos y rectángulos) para descubrir la solución:

```java
1/2 × ((3 + 10) + (7 + 10)) × (7 – 3) = 60
```

Ahora, ¿cómo podrías expresar esto en Java? Su primer problema es conciliar la notación extraña como el símbolo de integración o **`dy/dx`** la notación del lenguaje de programación familiar.

De hecho, para pensar desde los primeros principios se necesita un método, quizás llamado **`integrate`**, que requiere tres argumentos: uno es **`f`** y los otros son los límites (3.0 y 7.0 aquí). Por lo tanto, desea escribir en Java algo parecido a esto, donde la función **`f`** se pasa como argumento:

```java
integrate(f, 3, 7)
```

Tenga en cuenta que no puede escribir algo tan simple como

```java
integrate(x + 10, 3, 7)
```

por dos razones. En primer lugar, el alcance de **`x`** no está claro y, en segundo lugar, esto pasaría un valor de **`x+10`** para integrar en lugar de pasar la función **`f`**.

De hecho, el papel secreto de **`dx`** en matemáticas es decir "esa función que toma un argumento **`x`** cuyo resultado es **`x + 10`**".

### 3.9.2.Conexión a Java 8 lambdas

Como mencionamos anteriormente, Java 8 usa la notación **`(double x) -> x + 10`** (una expresión lambda) exactamente para este propósito; por eso puedes escribir

```java
integrate((double x) -> x + 10, 3, 7)
```

o

```java
integrate((double x) -> f(x), 3, 7)
```

o, utilizando una referencia de método como se mencionó anteriormente,

```java
integrate(C::f, 3, 7)
```

Si **`C`** es una clase que contiene **`f`** un método estático. La idea es que estás pasando el código **`f`** al método integrate.

Quizás ahora te preguntes cómo escribirías el método integrate en sí. Continúe suponiendo que **`f`** es una función lineal (recta). Probablemente le gustaría escribir de una forma similar a las matemáticas:

```java
public double integrate((double -> double) f, double a, double b) {     1
      return (f(a) + f(b)) * (b - a) / 2.0
}
```

* **1 ¡Código Java incorrecto! (No puedes escribir funciones como lo haces en matemáticas).**
  
Pero debido a que las expresiones lambda solo se pueden usar en un contexto que espera una interfaz funcional (en este caso, **`DoubleFunction`**[ 4 ] ), debes escribirlas de la siguiente manera:

**4**

*Usar **`DoubleFunction<Double>`** es más eficiente que usar **`Function<Double,Double>`** ya que evita encajonar el resultado.*

```java
public double integrate(DoubleFunction<Double> f, double a, double b) {
      return (f.apply(a) + f.apply(b)) * (b - a) / 2.0;
}
```

o usando **`DoubleUnaryOperator`**, lo que también evita encajonar el resultado:

```java
public double integrate(DoubleUnaryOperator f, double a, double b) {
      return (f.applyAsDouble(a) + f.applyAsDouble(b)) * (b - a) / 2.0;
}
```

Como comentario adicional, es un poco vergonzoso que tengas que escribir **`f.apply(a)`** en lugar de simplemente escribir **`f(a)`** como en matemáticas, pero Java simplemente no puede alejarse de la idea de que todo es un objeto en lugar de la idea de que una función sea verdaderamente independiente!

### Resumen

* Una *expresión lambda* puede entenderse como una especie de función anónima: no tiene nombre, pero tiene una lista de parámetros, un cuerpo, un tipo de retorno y posiblemente también una lista de excepciones que se pueden generar.
* Las expresiones lambda le permiten pasar código de forma concisa.
* Una *interfaz funcional* es una interfaz que declara exactamente un método abstracto.
* Las expresiones Lambda solo se pueden utilizar cuando se espera una interfaz funcional.
* Las expresiones Lambda le permiten proporcionar la implementación del método abstracto de una interfaz funcional directamente en línea y *tratar la expresión completa como una instancia de una interfaz funcional*.
* Java 8 viene con una lista de interfaces funcionales comunes en el paquete **`java.util .function`**, que incluye **`Predicate<T>`**, **`Function<T, R>`**, **`Supplier<T>`**, **`Consumer<T>`** y **`BinaryOperator<T>`**, descritas en la tabla 3.2 .
* Las especializaciones primitivas de interfaces funcionales genéricas comunes, como **`Predicate<T>`** y, **`Function<T, R>`** se pueden utilizar para evitar operaciones de boxeo: **`IntPredicate`**, **`IntToLongFunction`**, etc.
* El patrón de ejecución (para cuando necesita ejecutar algún comportamiento determinado en medio del código repetitivo que es necesario en un método, por ejemplo, asignación y limpieza de recursos) se puede usar con lambdas para obtener flexibilidad y reutilización adicionales.
* El tipo esperado para una expresión lambda se denomina tipo de destino .
* Las referencias a métodos le permiten reutilizar la implementación de un método existente y transmitirla directamente.
* Las interfaces funcionales como **`Comparator`**, **`Predicate`** y **`Function`** tienen varios métodos predeterminados que se pueden usar para combinar expresiones lambda.
