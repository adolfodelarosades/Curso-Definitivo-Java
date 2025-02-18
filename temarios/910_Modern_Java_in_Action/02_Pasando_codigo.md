
# Capítulo 2. Pasando código con parametrización de comportamiento

*Este capítulo cubre*

* Hacer frente a los requisitos cambiantes
* Parametrización de comportamiento
* Clases anónimas
* Vista previa de expresiones lambda
* Ejemplos del mundo real: `Comparator`, `Runnable` y GUI

Un problema bien conocido en la ingeniería de software es que no importa lo que haga, ***los requisitos del usuario cambiarán***. Por ejemplo, imagine una aplicación para ayudar a un agricultor a comprender su inventario. El agricultor puede querer una funcionalidad para encontrar todas las manzanas verdes en su inventario. Pero al día siguiente podría decirle: "En realidad, también quiero encontrar todas las manzanas que pesen más de 150 g". Dos días después, el agricultor regresa y agrega: "Sería realmente bueno si pudiera encontrar todas las manzanas que son verdes y pesan más de 150 g". ¿Cómo puede hacer frente a estos requisitos cambiantes? Idealmente, le gustaría minimizar su esfuerzo de ingeniería. Además, nuevas funcionalidades similares deberían ser sencillas de implementar y de mantener a largo plazo.

La ***parametrización del comportamiento*** (***Behavior parameterization***) es un patrón de desarrollo de software que le permite manejar cambios frecuentes en los requisitos. En pocas palabras, significa tomar un bloque de código y ponerlo a disposición sin ejecutarlo. Este bloque de código puede ser llamado posteriormente por otras partes de sus programas, lo que significa que puede diferir la ejecución de ese bloque de código. Por ejemplo, puede pasar el bloque de código como argumento a otro método que lo ejecutará más tarde. Como resultado, el comportamiento del método se parametriza en función de ese bloque de código. Por ejemplo, si procesa una colección, es posible que desee escribir un método que

* Puede hacer "algo" por cada elemento de una lista
* Puede hacer "otra cosa" cuando termine de procesar la lista
* Puede hacer "todavía algo más" si encuentra un error

A esto se refiere la ***parametrización del comportamiento***. He aquí una analogía: tu compañero de cuarto sabe cómo conducir hasta el supermercado y volver a casa. Puede decirle que compre una lista de cosas como pan, queso y vino. Esto es equivalente a llamar a un método **`goAndBuy`** pasando una lista de productos como argumento. Pero un día estás en la oficina y necesitas que él haga algo que nunca había hecho antes: recoger un paquete de la oficina de correos. Debe pasarle una lista de instrucciones: vaya a la oficina de correos, use este número de referencia, hable con el gerente y recoja el paquete. Puede pasarle la lista de instrucciones por correo electrónico, y cuando la reciba, podrá seguir las instrucciones. Ahora ha hecho algo un poco más avanzado que es equivalente a un método **`goAndDo`**, que puede ejecutar varios comportamientos nuevos como argumentos.

Comenzaremos este capítulo con un ejemplo de ***cómo puede evolucionar su código para que sea más flexible para los requisitos cambiantes***. Basándonos en este conocimiento, ***mostramos cómo utilizar la parametrización del comportamiento para varios ejemplos del mundo real***. Por ejemplo, es posible que ya haya usado el patrón de parametrización de comportamiento, usando clases e interfaces existentes en la API de Java para ordenar una lista, filtrar nombres de archivos o decirle a **`Thread`** que ejecute un bloque de código o incluso realice el manejo de eventos de GUI . Pronto se dará cuenta de que este patrón ha sido históricamente detallado en Java. Las expresiones lambda en Java 8 en adelante abordan el **problema de la verbosidad**. En el capítulo 3 mostraremos cómo construir expresiones lambda, dónde usarlas y cómo puede hacer que su código sea más conciso al adoptarlas.

## 2.1. HACER FRENTE A LOS REQUISITOS CAMBIANTES

Escribir código que pueda hacer frente a los requisitos cambiantes es difícil. Veamos un ejemplo que mejoraremos gradualmente, mostrando algunas prácticas recomendadas para hacer que su código sea más flexible. En el contexto de una aplicación de inventario agrícola, debe implementar una funcionalidad para filtrar manzanas ***verdes*** de una lista. Suena fácil, ¿verdad?

### 2.1.1. Primer intento: Filtrar manzanas verdes

Suponga, como en el capítulo 1, que tiene una enumeración **`Color`** disponible para representar diferentes colores de una manzana:

```java
enum Color { RED, GREEN }
```

Una primera solución podría ser la siguiente:

```java
public static List<Apple> filterGreenApples(List<Apple> inventory) {
   List<Apple> result = new ArrayList<>();                         1
   for(Apple apple: inventory){
      if( GREEN.equals(apple.getColor() ) {                        2
         result.add(apple);
      }
   }
   return result;
}
```

**1. Una lista acumulativa para manzanas**
**2. Selecciona solo manzanas verdes**

La línea resaltada (2) muestra la condición requerida para seleccionar manzanas verdes. Puede suponer que tiene una enumeración **`Color`** con un conjunto de colores, como **`GREEN`**, disponible. Pero ahora el agricultor cambia de opinión y quiere filtrar también manzanas *rojas*. ¿Qué puedes hacer? Una solución ingenua sería duplicar su método, renombrarlo como **`filterRedApples`** y cambiar la condición **`if`** para que coincida con las manzanas rojas. Sin embargo, este enfoque no se adapta bien a los cambios si el agricultor quiere varios colores. ***Un buen principio es este: cuando se encuentre escribiendo código casi repetido, intente abstraerlo***.

### 2.1.2. Segundo intento: Parametrizar el color

¿Cómo evitamos duplicar la mayor parte del código en **`filterGreenApples`** para hacer **`filterRedApples`**? Para parametrizar el color y ser más flexible a tales cambios, lo que podría hacer es agregar un parámetro a su método:

```java
public static List<Apple> filterApplesByColor(List<Apple> inventory, Color color) {
   List<Apple> result = new ArrayList<>();
   for (Apple apple: inventory) {
      if ( apple.getColor().equals(color) ) {
         result.add(apple);
      }
   }
   return result;
}
```

Ahora puede hacer feliz al agricultor e invocar su método de la siguiente manera:

```java
List<Apple> greenApples = filterApplesByColor(inventory, GREEN);
List<Apple> redApples = filterApplesByColor(inventory, RED);
...
```

Demasiado facil, verdad? Compliquemos un poco el ejemplo. El agricultor vuelve y le dice: "Sería genial diferenciar entre manzanas ligeras y pesadas. Las manzanas pesadas suelen tener un peso superior a 150g".

Con su sombrero de ingeniero de software, se da cuenta de antemano de que el agricultor puede querer variar el peso. Entonces crea el siguiente método para hacer frente a varios pesos a través de un parámetro adicional:

```java
public static List<Apple> filterApplesByWeight(List<Apple> inventory, int weight) {
   List<Apple> result = new ArrayList<>();
   for (Apple apple: inventory){
      if ( apple.getWeight() > weight ) {
         result.add(apple);
      }
   }
   return result;
}
```

Ésta es una buena solución, pero observe cómo debe duplicar la mayor parte de la implementación para recorrer el inventario y aplicar los criterios de filtrado en cada manzana. Esto es algo decepcionante porque rompe el ***principio DRY(don’t repeat yourself)*** de la ingeniería de software. ¿Qué sucede si desea modificar el desplazamiento del filtro para mejorar el rendimiento? Ahora tiene que modificar la implementación de ***todos*** sus métodos en lugar de solo uno. Esto es caro desde la perspectiva del esfuerzo de ingeniería.

Puede combinar el color y el peso en un método, llamado **`filter`**. Pero, de todos modos, necesitaría una forma de diferenciar el atributo por el que desea filtrar. Puede agregar una bandera para diferenciar entre consultas de color y peso. (¡Pero nunca hagas esto! Explicaremos por qué en breve).

### 2.1.3. Tercer intento: filtrar con todos los atributos que se te ocurran

Un feo intento de fusionar todos los atributos podría ser el siguiente:

```java
public static List<Apple> filterApples(List<Apple> inventory, Color color, int weight, boolean flag) {
   List<Apple> result = new ArrayList<>();
   for (Apple apple: inventory) {
      if ( (flag && apple.getColor().equals(color)) ||
            (!flag && apple.getWeight() > weight) ){          1
         result.add(apple);
      }
   }
   return result;
}
```

**1. Una forma fea de seleccionar color o peso**

Podrías usar esto de la siguiente manera (pero es feo):

```java
List<Apple> greenApples = filterApples(inventory, GREEN, 0, true);
List<Apple> heavyApples = filterApples(inventory, null, 150, false);
...
```

Esta solución es extremadamente mala. Primero, el código del cliente se ve terrible. ¿Qué significan **`true`** y **`false`**? Además, esta solución no se adapta bien a los requisitos cambiantes. ¿Qué pasa si el agricultor le pide que filtre con diferentes atributos de una manzana, por ejemplo, su tamaño, su forma, su origen, etc.? Además, ¿qué pasa si el agricultor le pide consultas más complicadas que combinan atributos, como manzanas verdes que también son pesadas? O tendría varios métodos **`filter`** duplicados o un método enormemente complejo. Hasta ahora, ha parametrizado el método **`filterApples`** *con valores* como **`String`**, **`Integer`**, un tipo **`enum`** o **`boolean`**. Esto puede estar bien para ciertos problemas bien definidos. Pero en este caso, lo que necesita es una mejor manera de decirle a su método **`filterApples`** los criterios de selección para las manzanas. En la siguiente sección, describimos cómo hacer uso de la parametrización del comportamiento para lograr esa flexibilidad.

## 2.2. PARAMETRIZACIÓN DE COMPORTAMIENTO

En la sección anterior vio que necesita una mejor manera que agregar muchos parámetros para hacer frente a los requisitos cambiantes. Retrocedamos y encontremos un mejor nivel de abstracción. Una posible solución es modelar sus criterios de selección: está trabajando con manzanas y devuelve un **`boolean`** basado en algunos atributos de **`Apple`**. Por ejemplo, ¿es verde? ¿Pesa más de 150 g? A esto lo llamamos **Predicado** (***una función que devuelve un `booleano`***). Por lo tanto, definamos una interfaz ***para modelar los criterios de selección***:

```java
public interface ApplePredicate{
   boolean test (Apple apple);
}
```

Ahora puede declarar múltiples implementaciones de **`ApplePredicate`** para representar diferentes criterios de selección, como se muestra a continuación (y se ilustra en la **figura 2.1**):

![image](https://github.com/adolfodelarosades/Java/assets/23094588/47ba2a2d-621d-46a9-bfd1-2f48a1bd000d)

```java
public class AppleHeavyWeightPredicate implements ApplePredicate {   1
   public boolean test(Apple apple) {
      return apple.getWeight() > 150;
   }
}
public class AppleGreenColorPredicate implements ApplePredicate {    2
   public boolean test(Apple apple) {
      return GREEN.equals(apple.getColor());
   }
}
```

**1. Selecciona solo manzanas pesadas**
**2. Selecciona solo manzanas verdes**

Puede ver estos criterios como comportamientos diferentes para el método **`filter`**. Lo que acaba de hacer está relacionado con el **Patrón de Diseño de la Estrategia (strategy design pattern)** (consulte http://en.wikipedia.org/wiki/Strategy_pattern), ***que le permite definir una familia de algoritmos, encapsular cada algoritmo (llamado estrategia) y seleccionar un algoritmo en tiempo de ejecución***. ***En este caso la familia de algoritmos es*** **`ApplePredicate`** ***y las diferentes estrategias son*** **`AppleHeavyWeightPredicate`** ***y*** **`AppleGreenColorPredicate`**.

Pero, ¿cómo se pueden utilizar las diferentes implementaciones de **`ApplePredicate`**? Necesita su método **`filterApples`** para aceptar objetos **`ApplePredicate`** para probar una condición en un **`Apple`**. Esto es lo que significa la ***parametrización del comportamiento (behavior parameterization)***: **la capacidad de decirle a un método que tome múltiples comportamientos (o estrategias) como parámetros y los use internamente para lograr diferentes comportamientos**.

Para lograr esto en el ejemplo en ejecución, agrega un parámetro al método **`filterApples`** para tomar un objeto **`ApplePredicate`**. Esto tiene un gran beneficio de ingeniería de software: ahora puede separar la lógica de iterar la colección dentro del método **`filter-Apples`** con el comportamiento que desea aplicar a cada elemento de la colección (en este caso, un predicado).

### 2.2.1. Cuarto intento: Filtrado por criterios abstractos

Nuestro método de filtro modificado, que utiliza un **`ApplePredicate`**, se ve así:

```java
public static List<Apple> filterApples(List<Apple> inventory, ApplePredicate p) {
   List<Apple> result = new ArrayList<>();
   for(Apple apple: inventory) {
      if(p.test(apple)) {                       1
         result.add(apple);
      }
   }
   return result;
}
```

**1. El predicado `p` encapsula la condición para probar en una manzana.**
   
### Passing code/behavior (Pasar código/comportamiento)
   
Vale la pena detenerse un momento para una pequeña celebración. Este código es mucho más flexible que nuestro primer intento, pero al mismo tiempo es fácil de leer y usar. Ahora puede crear diferentes objetos **`ApplePredicate`** y pasarlos al método **`filterApples`**. ¡Flexibilidad libre! Por ejemplo, si el agricultor le pide que busque todas las ***manzanas rojas que pesen más de 150 g***, todo lo que necesita hacer es crear una clase que implemente **`ApplePredicate`** en consecuencia. Su código ahora es lo suficientemente flexible para cualquier cambio de requisitos que involucre los atributos de **`Apple`**:

```java
public class AppleRedAndHeavyPredicate implements ApplePredicate {
   public boolean test(Apple apple){
      return RED.equals(apple.getColor())
             && apple.getWeight() > 150;
   }
}
List<Apple> redAndHeavyApples = filterApples(inventory, new AppleRedAndHeavyPredicate());
```

Has logrado algo genial; el comportamiento del método **`filterApples`** depende del código que le pase a través del objeto **`ApplePredicate`**. ***¡Ha parametrizado el comportamiento del método*** **`filterApples`**!

Tenga en cuenta que en el ejemplo anterior, el único código que importa es la implementación del método **`test`**, como se ilustra en la **figura 2.2**; esto es lo que define los nuevos comportamientos para el método **`filterApples`**. Desafortunadamente, debido a que el método **`filterApples`** solo puede tomar objetos, debe envolver ese código dentro de un objeto **`ApplePredicate`**. Lo que está haciendo es similar a pasar código en línea, porque está pasando una expresión **`boolean`** a través de un objeto que implementa el método **`test`**. Verá en la sección 2.3 (y con más detalle en el capítulo 3) que al usar lambdas, puede pasar directamente la expresión **`RED.equals(apple.getColor()) && apple.getWeight() > 150`** al método **`filterApples`** sin tener que definir varias clases **`ApplePredicate`**. ***Esto elimina la verbosidad innecesaria***.

![image](https://github.com/adolfodelarosades/Java/assets/23094588/86b6c486-9acd-4f6b-b0e8-f4ac4de63b60)

### Multiple behaviors, one parameter (Múltiples comportamientos, un parámetro)

Como explicamos anteriormente, la ***parametrización del comportamiento*** es excelente porque le permite separar la lógica de iterar la colección para filtrar y el comportamiento para aplicar en cada elemento de esa colección. Como consecuencia, puedes reutilizar el mismo método y darle diferentes comportamientos para lograr cosas diferentes, como se ilustra en la **figura 2.3**. Es por eso que la ***parametrización del comportamiento*** es un concepto útil que debe tener en su conjunto de herramientas para crear API flexibles.

![image](https://github.com/adolfodelarosades/Java/assets/23094588/758b4756-6832-4bfe-b97c-98708891097f)

Para asegurarse de que se siente cómodo con la idea de la ***parametrización del comportamiento***, intente realizar el cuestionario 2.1.

**Prueba 2.1: Escribe un método `prettyPrintApple` flexible**

Escriba un método **`prettyPrintApple`** que tome una **`List`** de **`Apples`** y que pueda parametrizarse con múltiples formas de generar una salida **`String`** a partir de un **`apple`**(un poco como múltiples métodos **`toString`** personalizados). Por ejemplo, podría indicarle a su método **`prettyPrintApple`** que imprima solo el peso de cada manzana. Además, podrías indicar a tu método **`prettyPrintApple`** para imprimir cada manzana individualmente y mencionar si es pesada o liviana. La solución es similar a los ejemplos de filtrado que hemos explorado hasta ahora. Para ayudarle a empezar, le proporcionamos un esquema aproximado del método **`prettyPrintApple`**:

```java
public static void prettyPrintApple(List<Apple> inventory, ???) {
    for(Apple apple: inventory) {
        String output = ???.???(apple);
        System.out.println(output);
    }
}
```

**Respuesta:**

Primero, necesita una forma de representar un comportamiento que tome un **`Apple`** y devuelva un **`String`** formateado como resultado. Hiciste algo similar cuando creaste una interfaz **`ApplePredicate`**:

```java
public interface AppleFormatter {
    String accept(Apple a);
}
```

Ahora puedes representar múltiples comportamientos de formato implementando la interfaz **`Apple-Formatter`**:

```java
public class AppleFancyFormatter implements AppleFormatter {
    public String accept(Apple apple) {
        String characteristic = apple.getWeight() > 150 ? "heavy" : "light";
        return "A " + characteristic +
               " " + apple.getColor() +" apple";
    }
}
public class AppleSimpleFormatter implements AppleFormatter {
    public String accept(Apple apple) {
        return "An apple of " + apple.getWeight() + "g";
    }
}
```

Finalmente, debes indicarle a tu método **`prettyPrintApple`** que tome objetos **`AppleFormatter`** y los use internamente. Puedes hacer esto agregando un parámetro a **`AppleFormatter`**:

```java
public static void prettyPrintApple(List<Apple> inventory, AppleFormatter formatter) {
  for(Apple apple: inventory) {
    String output = formatter.accept(apple);
    System.out.println(output);
  }
}
```

¡Bingo! Ahora puedes pasar múltiples comportamientos a tu método prettyPrintApple. Para ello, cree instancias de implementaciones **`AppleFormatter`** y proporciónelas como argumentos para **`prettyPrintApple`**:

```java
prettyPrintApple(inventory, new AppleFancyFormatter());
```

Esto producirá un resultado similar al de

```java
A light green apple
A heavy red apple
...
```

O prueba esto:

```java
prettyPrintApple(inventory, new AppleSimpleFormatter());
```

Esto producirá un resultado similar al de

```sh
An apple of 80g
An apple of 155g
...
```

Ha visto que puede abstraer el comportamiento y hacer que su código se adapte a los cambios de requisitos, pero el proceso es detallado porque necesita declarar múltiples clases de las que crea instancias solo una vez. Veamos cómo mejorar eso.

## 2.3. Abordar la verbosidad

Todos sabemos que se evitará una característica o concepto que sea complicado de usar. En este momento, cuando desea pasar un nuevo comportamiento a su método **`filterApples`**, se ve obligado a declarar varias clases que implementan la interfaz **`ApplePredicate`** y luego crear instancias de varios objetos **`ApplePredicate`** que asigna solo una vez, como se muestra en el siguiente listado que resume lo que ha visto hasta ahora. ¡Hay mucha verbosidad involucrada y es un proceso que requiere mucho tiempo!

**Listado 2.1. Parametrización del comportamiento: filtrado de manzanas con predicados**

```java
public class AppleHeavyWeightPredicate implements ApplePredicate {      1
    public boolean test(Apple apple) {
        return apple.getWeight() > 150;
    }
}
public class AppleGreenColorPredicate implements ApplePredicate {       2
    public boolean test(Apple apple) {
        return GREEN.equals(apple.getColor());
    }
}
public class FilteringApples {
    public static void main(String...args) {
        List<Apple> inventory = Arrays.asList(new Apple(80, GREEN),
                                              new Apple(155, GREEN),
                                              new Apple(120, RED));
        List<Apple> heavyApples =
            filterApples(inventory, new AppleHeavyWeightPredicate());   3
        List<Apple> greenApples =
            filterApples(inventory, new AppleGreenColorPredicate());    4
    }
    public static List<Apple> filterApples(List<Apple> inventory,
                                           ApplePredicate p) {
        List<Apple> result = new ArrayList<>();
        for (Apple apple : inventory) {
            if (p.test(apple)){
                result.add(apple);
            }
        }
        return result;
    }
}
```

**1. Selects heavy apples**
**2. Selects green apples**
**3. Results in a List containing one Apple of 155 g** 
**4. Results in a List containing two green Apples**

Esto es un gasto innecesario. ¿Puedes hacerlo mejor? Java tiene mecanismos llamados clases anónimas, que le permiten declarar y crear instancias de una clase al mismo tiempo. Le permiten mejorar su código un paso más haciéndolo un poco más conciso. Pero no son del todo satisfactorios. La sección 2.3.3 anticipa el próximo capítulo con una breve vista previa de cómo las expresiones lambda pueden hacer que su código sea más legible.

### 2.3.1. Clases anónimas

Las clases anónimas son como las clases locales (una clase definida en un bloque) con las que ya estás familiarizado en Java. Pero las clases anónimas no tienen nombre. Le permiten declarar y crear una instancia de una clase al mismo tiempo. En resumen, le permiten crear implementaciones ad hoc.

### 2.3.2. Quinto intento: usar una clase anónima

El siguiente código muestra cómo reescribir el ejemplo de filtrado creando un objeto que implemente **`ApplePredicate`** mediante una clase anónima:

```java
List<Apple> redApples = filterApples(inventory, new ApplePredicate() {     1
    public boolean test(Apple apple){
        return RED.equals(apple.getColor());
    }
});
```

**1. Parameterizes the behavior of the method filterApples with an anonymous class.**

Las clases anónimas se utilizan a menudo en el contexto de aplicaciones GUI para crear objetos controladores de eventos. No queremos traer recuerdos dolorosos de Swing, pero el siguiente es un patrón común que se ve en la práctica (aquí usando la API JavaFX, una plataforma UI moderna para Java):

```java
button.setOnAction(new EventHandler<ActionEvent>() {
    public void handle(ActionEvent event) {
        System.out.println("Whoooo a click!!");
    }
});
```

Pero las clases anónimas todavía no son lo suficientemente buenas. Primero, tienden a ser voluminosos porque ocupan mucho espacio, como se muestra aquí en el código en negrita usando los mismos dos ejemplos usados ​​anteriormente:

```java
List<Apple> redApples = filterApples(inventory, new ApplePredicate() {   1
    public boolean test(Apple a){
        return RED.equals(a.getColor());
    }
});
button.setOnAction(new EventHandler<ActionEvent>() {                     1
    public void handle(ActionEvent event) {
        System.out.println("Whoooo a click!!");
    }
```

**1. Lots of boilerplate code - Mucho código repetitivo**

En segundo lugar, muchos programadores encuentran su uso confuso. Por ejemplo, la Prueba 2.2 muestra un rompecabezas clásico de Java que toma por sorpresa a la mayoría de los programadores. Pruébalo.

Prueba 2.2: rompecabezas de clase anónima
¿Cuál será el resultado cuando se ejecute este código: 4, 5, 6 ó 42?

```java
public class MeaningOfThis {
    public final int value = 4;
    public void doIt() {
        int value = 6;
        Runnable r = new Runnable() {
                public final int value = 5;
                public void run(){
                    int value = 10;
                    System.out.println(this.value);
                }
            };
            r.run();
        }
        public static void main(String...args) {
            MeaningOfThis m = new MeaningOfThis();
            m.doIt();                                1
        }
}
```

**1 What’s the output of this line? - ¿Cuál es el resultado de esta línea?** 

**Respuesta:**

La respuesta es **`5`** porque **`this`** se refiere a la clase envolvente **`Runnable`**, no a la clase envolvente **`MeaningOfThis`**.

La verbosidad en general es mala; Desalienta el uso de una función del lenguaje porque lleva mucho tiempo escribir y mantener un código detallado, ¡y no es agradable de leer! Un buen código debe ser fácil de comprender de un vistazo. Aunque las clases anónimas abordan de alguna manera la verbosidad asociada con la declaración de múltiples clases concretas para una interfaz, siguen siendo insatisfactorias. En el contexto de pasar un fragmento de código simple (por ejemplo, una expresión **`boolean`** que representa un criterio de selección), aún debe crear un objeto e implementar explícitamente un método para definir un nuevo comportamiento (por ejemplo, el método **`test`** para **`Predicate`** o el método **`handle`** para **`EventHandler`**).

Idealmente, nos gustaría animar a los programadores a utilizar el patrón de parametrización de comportamiento porque, como acaba de ver, hace que su código se adapte más a los cambios de requisitos. En el capítulo 3 , verá que los diseñadores del lenguaje Java 8 resolvieron este problema introduciendo expresiones lambda, una forma más concisa de pasar código. Basta de suspenso; A continuación se ofrece una breve vista previa de cómo las expresiones lambda pueden ayudarle en su búsqueda de un código limpio.

### 2.3.3.Sexto intento: usar una expresión lambda

El código anterior se puede reescribir de la siguiente manera en Java 8 usando una expresión lambda:

```java
List<Apple> result = filterApples(inventory, (Apple apple) -> RED.equals(apple.getColor()));
```

¡Tienes que admitir que este código parece mucho más limpio que nuestros intentos anteriores! Es genial porque está empezando a parecerse mucho más al planteamiento del problema. Ahora hemos abordado el problema de la verbosidad. La figura 2.4 resume nuestro viaje hasta el momento.

**Figura 2.4. Parametrización de comportamiento versus parametrización de valor**

![02-04](images/02-04.png)

### 2.3.4.Séptimo intento: abstraer sobre el tipo de lista

Hay un paso más que puedes dar en tu viaje hacia la abstracción. Por el momento, el método **`filterApples`** sólo funciona para Apple. Pero también puedes abstraer el tipo **`List`** para ir más allá del dominio del problema en el que estás pensando, como se muestra:

```java
public interface Predicate<T> {
    boolean test(T t);
}
public static <T> List<T> filter(List<T> list, Predicate<T> p) {     1
    List<T> result = new ArrayList<>();
    for(T e: list) {
        if(p.test(e)) {
            result.add(e);
        }
    }
    return result;
}
```

**1 Introduce un parámetro de tipo T**

¡Ahora puedes usar el método **`filter`** con una **`List`** de plátanos, naranjas, **`Integers`** o **`Strings`**! Aquí hay un ejemplo, usando expresiones lambda:

```java
List<Apple> redApples =
  filter(inventory, (Apple apple) -> RED.equals(apple.getColor()));
List<Integer> evenNumbers =
  filter(numbers, (Integer i) -> i % 2 == 0);
```

¿No es genial? ¡Has logrado encontrar el punto óptimo entre flexibilidad y concisión, lo que no era posible antes de Java 8!

## 2.4. Ejemplos del mundo real

Ahora ha visto que la parametrización del comportamiento es un patrón útil para adaptarse fácilmente a los requisitos cambiantes. Este patrón le permite encapsular un comportamiento (un fragmento de código) y parametrizar el comportamiento de los métodos pasando y utilizando estos comportamientos que cree (por ejemplo, diferentes predicados para un Apple). Mencionamos anteriormente que este enfoque es similar al patrón de diseño de estrategias. Es posible que ya hayas utilizado este patrón en la práctica. Muchos métodos de la API de Java se pueden parametrizar con diferentes comportamientos. Estos métodos se suelen utilizar junto con clases anónimas. Mostramos cuatro ejemplos, que deberían solidificar la idea de pasar código: ordenar con un **`Comparator`**, ejecutar un bloque de código con **`Runnable`**, devolver un resultado de una tarea usando **`Callable`** y manejo de eventos GUI.

### 2.4.1.Ordenar con un Comparator

Ordenar una colección es una tarea de programación recurrente. Por ejemplo, supongamos que su granjero quiere que clasifique el inventario de manzanas según su peso. O tal vez cambia de opinión y quiere que clasifiques las manzanas por color. ¿Suena familiar? Sí tú Necesita una forma de representar y utilizar diferentes comportamientos de clasificación para adaptarse fácilmente a los requisitos cambiantes.

Desde Java 8, a **`List`** viene con un método **`sort`** (también puedes usar **`Collections.sort`**). El comportamiento de **`sort`** se puede parametrizar mediante un  objeto **`java.util.Comparator`**, que tiene la siguiente interfaz:

```java
// java.util.Comparator
public interface Comparator<T> {
    int compare(T o1, T o2);
}
```

Por lo tanto, puede crear diferentes comportamientos para el método **`sort`** creando una implementación ad hoc de **`Comparator`**. Por ejemplo, puedes usarlo para ordenar el inventario aumentando el peso usando una clase anónima:

```java
inventory.sort(new Comparator<Apple>() {
   public int compare(Apple a1, Apple a2) {
      return a1.getWeight().compareTo(a2.getWeight());
   }
});
```

Si el agricultor cambia de opinión sobre cómo clasificar las manzanas, puede crear un **`Comparator`** ad hoc que coincida con el nuevo requisito y pasarlo al método **`sort`**. Los detalles internos de cómo ordenar se abstraen. Con una expresión lambda quedaría así:

```java
inventory.sort(
  (Apple a1, Apple a2) -> a1.getWeight().compareTo(a2.getWeight()));
```

De nuevo, no te preocupes por esta nueva sintaxis por ahora; El siguiente capítulo cubre en detalle cómo escribir y usar expresiones lambda.

### 2.4.2.Ejecutar un bloque de código con Runnable

Los subprocesos de Java permiten ejecutar un bloque de código al mismo tiempo que el resto del programa. Pero, ¿cómo puedes decirle a un hilo qué bloque de código debe ejecutar? Cada uno de varios subprocesos puede ejecutar código diferente. Lo que necesita es una forma de representar un fragmento de código que se ejecutará más adelante. Hasta Java 8, sólo se podían pasar objetos **`Thread`** al constructor, por lo que el patrón de uso típico y torpe era pasar una clase anónima que contenía un método **`run`** que devolvía **`void`**(sin resultado). Estas clases anónimas implementan la interfaz **`Runnable`**.

En Java, puedes usar la interfaz **`Runnable`** para representar un bloque de código a ejecutar; tenga en cuenta que el código devuelve **`void`**(sin resultado):

```java
// java.lang.Runnable
public interface Runnable {
    void run();
}
```

Puede utilizar esta interfaz para crear hilos con su elección de comportamiento, de la siguiente manera:

```java
Thread t = new Thread(new Runnable() {
    public void run() {
        System.out.println("Hello world");
    }
});
```

Pero desde Java 8 puedes usar una expresión lambda, por lo que la llamada a **`Thread`** se vería así:

```java
Thread t = new Thread(() -> System.out.println("Hello world"));
```

### 2.4.3. Devolver un resultado usando Callable

Quizás esté familiarizado con la abstracción **`ExecutorService`** que se introdujo en Java 5. La interfaz **`ExecutorService`** desacopla cómo se envían y ejecutan las tareas. Lo que es útil en comparación con el uso de **`threads`** and **`Runnable`** que al usar un **`Executor-Service`** puedes enviar una tarea a un grupo de subprocesos y almacenar su resultado en un archivo **`Future`**. No se preocupe si esto no le resulta familiar; volveremos a abordar este tema en capítulos posteriores cuando analicemos la concurrencia con más detalle. Por ahora, todo lo que necesitas saber es que la interfaz **`Callable`** se utiliza para modelar una tarea que devuelve un resultado. Puedes verlo como una actualización **`Runnable`**:

```java
// java.util.concurrent.Callable
public interface Callable<V> {
    V call();
}
```

Puede usarlo de la siguiente manera, enviando una tarea a un servicio ejecutor. Aquí devuelves el nombre del **`Thread`** responsable de ejecutar la tarea:

```java
ExecutorService executorService = Executors.newCachedThreadPool();
Future<String> threadName = executorService.submit(new Callable<String>() {
    @Override
    public String call() throws Exception {
        return Thread.currentThread().getName();
    }
});
```

Usando una expresión lambda, este código se simplifica a lo siguiente:

```java
Future<String> threadName = executorService.submit(
                     () -> Thread.currentThread().getName());
```
### 2.4.4.Manejo de eventos GUI

Un patrón típico en la programación GUI es realizar una acción en respuesta a un evento determinado, como hacer clic o pasar el cursor sobre el texto. Por ejemplo, si el usuario hace clic en el botón Enviar, es posible que desee mostrar una ventana emergente o quizás registrar la acción en un archivo. Una vez más, se necesita una forma de afrontar los cambios; Debería poder realizar cualquier respuesta. En JavaFX, puedes usar un **`EventHandler`** para representar una respuesta a un evento pasándolo a **`setOnAction`**:

```java
Button button = new Button("Send");
button.setOnAction(new EventHandler<ActionEvent>() {
    public void handle(ActionEvent event) {
        label.setText("Sent!!");
    }
});
```

Aquí el comportamiento del método **`setOnAction`** se parametriza con objetos **`EventHandler`**. Con una expresión lambda quedaría así:

```java
button.setOnAction((ActionEvent event) -> label.setText("Sent!!"));
```

### Resumen

* La parametrización del comportamiento es la capacidad de un método de tomar múltiples comportamientos diferentes como parámetros y usarlos internamente para lograr diferentes comportamientos.
* La parametrización del comportamiento le permite hacer que su código se adapte mejor a los requisitos cambiantes y le ahorra esfuerzos de ingeniería en el futuro.
* Pasar código es una forma de dar nuevos comportamientos como argumentos a un método. Pero es detallado antes de Java 8. Las clases anónimas ayudaron un poco antes de Java 8 a deshacerse de la verbosidad asociada con la declaración de múltiples clases concretas para una interfaz que se necesitan solo una vez.
* La API de Java contiene muchos métodos que se pueden parametrizar con diferentes comportamientos, que incluyen sorting, threads, y GUI handling.
