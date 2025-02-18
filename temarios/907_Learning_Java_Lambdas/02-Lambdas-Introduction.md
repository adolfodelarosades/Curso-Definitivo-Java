# Capítulo 2. Introducción a Lambdas

En este capítulo, presentaremos las ideas de lambdas, haremos:

* Analice algunos antecedentes de lambdas y la programación funcional en general.
* Habla sobre funciones versus clases en Java.
* Mire la sintaxis básica de lambdas en Java.

## λs en programación funcional

Antes de analizar las cosas con más profundidad, veamos algunos antecedentes generales de las lambdas.

Si no lo has visto antes, la letra griega λ ( lambda ) se utiliza a menudo como taquigrafía cuando se habla de lambdas.

### Década de 1930 y el cálculo lambda

En informática, las lambdas se remontan al cálculo lambda. Notación matemática para funciones introducida por Alonzo Church en la década de 1930. Era una forma de explorar las matemáticas utilizando funciones y luego fue redescubierta como una herramienta útil en informática.

Formalizó la noción de términos lambda y las reglas para transformar esos términos. Estas reglas o funciones se corresponden directamente con las ideas modernas de la informática. Todas las funciones del cálculo lambda son anónimas, lo que también se ha tomado literalmente en informática.

A continuación se muestra un ejemplo de una expresión de cálculo lambda:

#### Una expresión de cálculo lambda

```sh
λx.x+1
```

Esto define una función anónima o lambda con un solo argumento x. El cuerpo sigue el punto y añade uno a ese argumento.

### Década de 1950 y LISP

En la década de 1950, John McCarthy inventó LISP mientras estaba en el MIT. Este era un lenguaje de programación diseñado para modelar problemas matemáticos y estaba fuertemente influenciado por el cálculo lambda.

Usó la palabra lambda como operador para definir una función anónima.

He aquí un ejemplo:

#### Una expresión LISP

```sh
(lambda (arg) (+ arg 1))
```

Esta expresión LISP se evalúa como una función que, cuando se aplica, tomará un solo argumento, lo vinculará **`arg`** y luego **`1`** lo agregará.

Las dos expresiones producen lo mismo, una función para incrementar un número. Puedes ver que los dos son muy similares.

El cálculo lambda y LISP han tenido una gran influencia en la programación funcional. Las ideas de aplicar funciones y razonar sobre problemas utilizando funciones se han trasladado directamente a los lenguajes de programación. De ahí el uso del término en nuestro campo. Una lambda en cálculo es lo mismo que en los lenguajes de programación modernos y se usa de la misma manera.

## ¿Qué es una lambda?

Entonces, en términos simples, **una lambda es solo una función anónima**. Eso es todo. Nada especial. ***Es solo una forma compacta de definir una función***. ***Las funciones anónimas son útiles cuando desea transmitir fragmentos de funcionalidad reutilizable. Por ejemplo, pasar funciones a otras funciones***.

Muchos lenguajes principales ya admiten lambdas, incluidos **Scala**, **C#**, **Objective-C**, **Ruby**, **C++(11)**, **Python** y muchos otros.

## Funciones vs Clases

Tenga en cuenta que ***una función anónima no es lo mismo que una clase anónima*** en Java. Aún es necesario crear una instancia de una clase anónima en Java en un objeto. Puede que no tenga un nombre propio, pero sólo puede resultar útil cuando es un objeto.

Por otro lado, una función no tiene ninguna instancia asociada. Las funciones están disociadas de los datos sobre los que actúan, mientras que un objeto está íntimamente asociado con los datos sobre los que actúa.

Puede usar lambdas en Java moderno en cualquier lugar donde anteriormente hubiera usado una interfaz de método único, por lo que puede parecer azúcar sintáctico, pero no lo es. Echemos un vistazo a en qué se diferencian y comparemos las clases anónimas con lambdas; clases versus funciones.

### Lambdas en Java moderno

Una implementación típica de una clase anónima (una interfaz de método único) en Java anterior a 8 podría verse así. El método **`anonymousClassmétodo`** llama al método **`waitFor`** y pasa alguna implementación de **`Condition`**; en este caso, dice, espere a que se apague algún servidor:

#### Uso típico de una clase anónima

```java
void anonymousClass() {
    final Server server = new HttpServer();
    waitFor(new Condition() {
        @Override
        public Boolean isSatisfied() {
            return !server.isRunning();
        }
    });
}
```

La lambda funcionalmente equivalente se vería así:

#### Funcionalidad equivalente a lambda

```java
void closure() { 
     Server server = new HttpServer();
     waitFor(() -> !server.isRunning()); 
 }
```

En aras de la integridad, un método de encuesta ingenuo **`waitFor`** podría verse así:

```java
class WaitFor {
    static void waitFor(Condition condition) throws   
    InterruptedException {
        while (!condition.isSatisfied())
            Thread.sleep(250);
    }
}
```

### Algunas diferencias teóricas

En primer lugar, ambas implementaciones son de hecho closures; la última también es una lambda. Veremos esta distinción con más detalle más adelante en la sección Lambdas vs closures. Significa que ambos tienen que capturar su "environment" en tiempo de ejecución. En Java anterior a 8, esto significa copiar las cosas que el closure necesita en una instancia de una clase (instancias anónimas de **`Condition`**). En nuestro ejemplo, la variable del servidor debería copiarse en la instancia.

Como es una copia, debe declararse final para garantizar que no se pueda cambiar entre el momento de su captura y su uso. Estos dos momentos podrían ser muy diferentes dado que los closures se utilizan a menudo para diferir la ejecución hasta algún momento posterior (consulte la evaluación diferida, por ejemplo). El Java moderno utiliza un ingenioso truco mediante el cual, si puede razonar que una variable nunca se actualiza, también podría ser final, por lo que la trata efectivamente como final y no es necesario declararla explícitamente como final.

Por otro lado, una lambda no necesita copiar su entorno ni capturar ningún término. Esto significa que puede tratarse como una función genuina y no como una instancia de una clase. ¿Cual es la diferencia? Infinidad.

### Funciones vs Clases

Para empezar, funciones; funciones genuinas, no es necesario crear instancias muchas veces. No estoy seguro de si creación de instancias es siquiera la palabra correcta cuando se habla de asignar memoria y cargar un fragmento de código de máquina como una función. El punto es que, una vez que esté disponible, se puede reutilizar, es de naturaleza idempotente ya que no retiene ningún estado. Los métodos de clases estáticas son lo más parecido que tiene Java a las funciones.

Para Java, esto significa que no es necesario crear una instancia de una lambda cada vez que se evalúa, lo cual es un gran problema. A diferencia de la creación de instancias de una clase anónima, el impacto en la memoria debería ser mínimo.

En términos de algunas diferencias conceptuales entonces:

* Se deben crear instancias de las clases, mientras que las funciones no.
* Cuando se actualizan las clases, se asigna memoria para el objeto.
* La memoria sólo necesita asignarse una vez para las funciones. Se almacenan en el área permanente del montón.
* Los objetos actúan sobre sus propios datos, las funciones actúan sobre datos no relacionados.
* Los métodos de clases estáticas en Java son aproximadamente equivalentes a funciones.

### Algunas diferencias concretas

Algunas diferencias concretas entre funciones y clases incluyen su semántica de captura y cómo ocultan las variables.

#### Capturar semántica

Otra diferencia tiene que ver con la semántica de captura para esto. En una clase anónima, esto se refiere a la instancia de la clase anónima. Por ejemplo, **`Foo$InnerClass`** y no **`Foo`**. Es por eso que tiene una sintaxis ligeramente extraña, como **`Foo.this.x`** cuando se refiere al ámbito adjunto de la clase anónima.

Por otro lado, en lambdas, esto se refiere al alcance adjunto (Foo directamente en nuestro ejemplo). De hecho, las lambdas tienen un alcance completamente léxico, lo que significa que no heredan ningún nombre de un supertipo ni introducen un nuevo nivel de alcance; puede acceder directamente a campos, métodos y variables locales desde el ámbito adjunto.

Por ejemplo, esta clase muestra que lambda puede hacer referencia a la variable **`firstName`** directamente.

```java
public class Example {
    private String firstName = "Jack";

    public void example() {
        Function<String, String> addSurname = surname -> {
            // equivalent to this.firstName
            return firstName + " " + surname;  // or even, this.firstName
        };
    }
}
```

Aquí, **`firstName`** es una abreviatura de **`this.firstName`** y debido a que se refiere al ámbito adjunto (la clase **`Example`**), su valor será **"Jack"**.

La clase anónima equivalente tendría que hacer referencia explícita a **`firstName`** desde el ámbito adjunto. No puedes usar esto como en este contexto, esto significa la instancia anónima y no existe **`firstName`** allí. Entonces, se compilará lo siguiente:

```java
public class Example {
    private String firstName = "Charlie";

    public void anotherExample() {
        Function<String, String> addSurname = new Function<String, String>() {
            @Override
            public String apply(String surname) {
                return Example.this.firstName + " " + surname;   
                // OK
            }
        };
    }
}
```

pero esto no lo hará.

```java
public class Example {
    private String firstName = "Charlie";

  public void anotherExample() {
    Function<String, String> addSurname = new Function<String, String>() {
      @Override
      public String apply(String surname) {
        return this.firstName + " " + surname;   // compiler error
      }
    };
  }
}
```

Aún puedes acceder al campo directamente (es decir, simplemente llamando a **`return firstName + " " + surname`**) pero no puedes hacerlo usando esto. El punto aquí es demostrar la diferencia en los esquemas de captura para esto cuando se usa en instancias lambdas frente a instancias anónimas.

#### Variables sombreadas

Hacer referencia a variables sombreadas se vuelve mucho más sencillo de razonar con la semántica **`this`** simplificada. Por ejemplo,

```java
public class ShadowingExample {

    private String firstName = "Charlie";

    public void shadowingExample(String firstName) {
        Function<String, String> addSurname = surname -> {
            return this.firstName + " " + surname;
        };
    }
}
```

Aquí, debido a que **`this`** está dentro de lambda, se refiere al ámbito circundante. Entonces **`this.firstName`** tendrá el valor "**`Charlie`**" y no el parámetro del método del mismo nombre. La semántica de captura lo deja más claro. Si usa **`firstName`** (y suelta **`this`**), se referirá al parámetro.

En el siguiente ejemplo, utilizando una instancia anónima, **`firstName`** simplemente se hace referencia al parámetro. Si desea hacer referencia a la versión adjunta, usaría **`Example.this.firstName`**:

```java
public class ShadowingExample {

    private String firstName = "Charlie";

    public void anotherShadowingExample(String firstName) {
        Function<String, String> addSurname = new Function<String, String>() {
            @Override
            public String apply(String surname) {
                return firstName + " " + surname;
            }
        };
    }
}
```

### Resumen

Las funciones en el sentido académico son cosas muy diferentes de las clases anónimas (que a menudo tratamos como funciones en Java anterior a 8). Es útil comprender las distinciones para poder justificar el uso de lambdas por algo más que solo su sintaxis concisa. Por supuesto, hay muchas ventajas adicionales en el uso de lambdas (entre ellas la actualización del JDK para utilizarlas en gran medida).

Cuando echemos un vistazo a la nueva sintaxis lambda a continuación, recuerde que aunque las lambdas se usan de manera muy similar a las clases anónimas en Java, son técnicamente diferentes. No es necesario crear una instancia de Lambdas en Java cada vez que se evalúan, a diferencia de una instancia de una clase anónima.

Esto debería servirle para recordarle que las lambdas en Java no son sólo azúcar sintáctico.

## λ sintaxis básica

Echemos un vistazo a la sintaxis lambda básica.

Una lambda es básicamente un bloque de funcionalidad anónimo. Es muy parecido a usar una instancia de clase anónima. Por ejemplo, si queremos ordenar una matriz en Java, podemos usar el método **`Arrays.sort`** que toma una instancia de la interfaz **`Comparator`**.

Se vería algo como esto:

```java
Arrays.sort(numbers, new Comparator<Integer>() {
    @Override
    public int compare(Integer first, Integer second) {
        return first.compareTo(second);
    }
});
```

La instancia **`Comparator`** aquí es una parte abstracta de la funcionalidad; no significa nada por sí solo; sólo cuando se utiliza mediante el método **`sort`** tiene un propósito.

Usando la nueva sintaxis de Java, puedes reemplazar esto con una lambda que se ve así:

```java
Arrays.sort(numbers, (first, second) -> first.compareTo(second));
```

Es una forma más sucinta de lograr lo mismo. De hecho, Java trata esto como si fuera una instancia de la clase **`Comparator`**. Si tuviéramos que extraer una variable para lambda (el segundo parámetro), su tipo sería **`Comparator<Integer>`** como la instancia anónima anterior.

```java
Comparator<Integer> ascending = (first, second) -> first.compareTo(second);
Arrays.sort(numbers, ascending);
```

Porque **`Comparator`** tiene un solo método abstracto; **`compareTo`**, el compilador puede entender que cuando tenemos un bloque anónimo como este, en realidad nos referimos a una instancia de **`Comparator`**. Puede hacer esto gracias a un par de otras características nuevas de las que hablaremos más adelante; Interfaces funcionales y mejoras en la inferencia de tipos.

### Desglose de sintaxis

Siempre puedes pasar de usar un único método abstracto a usar lambda.

Digamos que tenemos una interfaz **`Example`** con un método **`apply`**, que devuelve algún tipo y toma algún argumento:

```java
interface Example {
     R apply(A arg);
}
```

Podríamos crear una instancia con algo como esto:

```java
new Example() {
    @Override
    public R apply(A args) {
        body
    }
};
```

Y para convertirlo a lambda, básicamente recortamos la grasa. Eliminamos la creación de instancias y la anotación, eliminamos los detalles del método, lo que deja solo la lista de argumentos y el cuerpo.

```java
(args) {
    body
}
```

Luego introducimos el nuevo símbolo de flecha para indicar que todo es una lambda y que lo que sigue es el cuerpo y esa es nuestra sintaxis lambda básica:

```java
(args) -> {
    body
}
```

Tomemos el ejemplo de clasificación anterior y sigamos estos pasos. Empezamos con la instancia anónima:

```java
Arrays.sort(numbers, new Comparator<Integer>() {
    @Override
    public int compare(Integer first, Integer second) {
        return first.compareTo(second);
    }
});
```

y recortar la creación de instancias y la firma del método:

```java
Arrays.sort(numbers, (Integer first, Integer second) {
    return first.compareTo(second);
});
```

introducir la lambda

```java
Arrays.sort(numbers, (Integer first, Integer second) -> {
    return first.compareTo(second);
});
```

y terminamos. Sin embargo, hay un par de optimizaciones que podemos hacer. Puede eliminar los tipos si el compilador sabe lo suficiente como para inferirlos.

```java
Arrays.sort(numbers, (first, second) -> {
    return first.compareTo(second);
});
```

y para expresiones simples, puede quitar las llaves para producir una expresión lambda:

```java
Arrays.sort(numbers, (first, second) -> first.compareTo(second));
```

En este caso, el compilador puede inferir lo suficiente como para saber a qué se refiere. La declaración única devuelve un valor consistente con la interfaz, por lo que dice: "no es necesario que me digas que vas a devolver algo, puedo verlo por mí mismo".

Para métodos de interfaz de un solo argumento, incluso puede eliminar los primeros corchetes. Por ejemplo, la lambda toma un argumento **`x`** y lo devuelve **`x + 1`**;

```java
(x) -> x + 1
```

se puede escribir sin paréntesis

```java
x -> x + 1
```

### Resumen

Recapitulemos con un resumen de las opciones de sintaxis.

Resumen de sintaxis:

```java
(int x, int y) -> { return x + y; }
(x, y) -> { return x + y; }
(x, y) -> x + y; x -> x * 2
() -> System.out.println("Hey there!");
System.out::println;
```

El primer ejemplo (**`(int x, int y) -> { return x + y; }`**) es la forma más detallada de crear una lambda. Los argumentos de la función junto con sus tipos están entre paréntesis, seguidos de la nueva sintaxis de flecha y luego el cuerpo; el bloque de código que se va a ejecutar.

A menudo puedes eliminar los tipos de la lista de argumentos, como **`(x, y) -> { return x + y; }`**. El compilador utilizará aquí la inferencia de tipos para intentar adivinar los tipos. Lo hace en función del contexto en el que intenta utilizar la lambda.

Si su bloque de código devuelve algo o es una expresión de una sola línea, puede quitar las llaves y la declaración de retorno, por ejemplo **`(x, y) -> x + y;`**.

En el caso de un solo argumento, puede eliminar los paréntesis **`x -> x * 2`**.

Si no tiene ningún argumento, se necesita el símbolo de "hamburguesa", **`() -> System.out.println("Hey there!");`**.

En aras de la exhaustividad, existe otra variación; una especie de atajo a una lambda llamada referencia de método. Un ejemplo es algo así como **`System.out::println;`**, que es básicamente un atajo hacia la lambda **`(value) -> System.out.prinltn(value)`**.

Hablaremos sobre las referencias de métodos con más detalle más adelante, así que por ahora, tenga en cuenta que existen y que pueden usarse en cualquier lugar donde pueda usar una lambda.
