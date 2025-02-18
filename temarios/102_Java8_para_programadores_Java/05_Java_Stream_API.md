# 5. Java Stream API 106m

   * 16 Introducción al API Stream 16:04 
   * 17 Métodos de búsqueda de datos 6:27 
   * 18 Métodos de datos, cálculo y ordenación 10:25 
   * 19 Uso de Map y flapMap 12:04 
   * 19 Uso de la clase Collector 19:28 
   * 20 Uso de streams y filtros 8:27 
   * 21 Referencias a métodos con stream 10:55 
   * 22 Práctica: Todos los elementos del API stream trabajando conjuntamente 22:25 
   * Contenido adicional 7
   
# 16 Introducción al API Stream 16:04 

[Introducción al API Stream](pdfs/16_Introducción_al_API_Stream.pdf)

## Resumen del Profesor

### 16.1 Introducción

El API Stream es una de las grandes novedades de Java SE 8, junto con las expresiones lambda. Permite realizar operaciones de filtro/mapeo/reducción sobre colecciones de datos, de forma secuencial o paralela.

Un Stream es una secuencia de elementos que soporta operaciones para procesarlos

* Usando expresiones lambda

* Permitiendo el encadenamiento de operaciones (para producir así un código que se lee mucho mejor y es más conciso)

* De forma secuencial o paralela

En Java, los streams vienen definidos por el interfaz `java.util.stream.Stream<T>`.

### 16.2 Características de un Stream

* Las operaciones intermedias retornan un Stream (permitiendo así el encadenamiento de llamadas a métodos).
* Las operaciones intermedias se encolan, y son invocadas al invocar una operación terminal.
* Solo se puede recorrer una vez; si lo intentamos recorrer una segunda vez, provocará una excepción.
* Utiliza iteración interna en lugar de iteración externa; así nos centramos en qué hacer con los datos, no en como recorrerlos.

### 16.3 Algunos subtipos de streams

En el caso de que vayamos a utilizar un stream de tipos básicos (`int`, `long` y `double`), Java nos proporciona las interfaces `IntStream`, `LongStream` y `DoubleStream`.

### 16.4 Formas de obtener un stream

* `Stream.of(...)`: retorna un stream secuencial y ordenado de los parámetros que se le pasan.
* `Arrays.streams(T[] t)`: retorna un stream secuencial a partir del array proporcionado. Si el array es de tipo básico, se retorna un subtipo de `Stream`.
* `Stream.empty()`: retorna un stream vacío.
* `Stream.iterate(T, UnaryOperator<T>)`: devuelve un stream infinito, ordenado y secuencial. Lo hace a partir de un valor y de aplicar una función a ese valor. Se puede limitar el tamaño con `limit(long)`.
* `Collection.stream()` y `Collection.parallelStream()`: devuelve un stream (secuencial o paralelo) a partir de una colección.
* `Collection.generate`: retorna un stream infinito, secuencial y no ordenado a partir de una instancia de `Supplier` (o su correspondiente expresión lambda).

### 16.5 Operaciones intermedias

Son operaciones que devuelven un `Stream`, y por tanto, permiten encadenar llamadas a métodos. Sirven, entre otras funcionalidades, para filtrar y transformar los datos.

#### 16.5.1 Operaciones de filtrado

* `filter(Predicate<T>)`: nos permite filtrar usando una condición.
* `limit(n)`: nos permite obtener los n primeros elementos.
* `skip(m)`: nos permite *obviar* los m primeros elementos.

#### 16.5.2 Operaciones de mapeo

* `map(Function<T,R>)`: nos permite transformar los valores de un stream a través de una expresión lambda o una instancia de `Function`.
* `mapToInt(...)`, `mapToDouble(...)` y `mapToLong(...)` nos permite transformar a tipos básicos, obteniendo `IntStream`, `DoubleStream` o `LongStream`, respectivamente.

### 16.6 Operaciones terminales

Provocan que se ejecuten todas las operaciones intermedias. Las hay de varios tipos:

* Para consumir los elementos (por ejemplo, `forEach`)
* Para obtener datos de un stram (agregación)
* Para recolectar los elementos y transformarlos en otro objeto, como una colección.

## Transcripción

<img src="images/16-01.png">
<img src="images/16-02.png">
<img src="images/16-03.png">
<img src="images/16-04.png">
<img src="images/16-05.png">
<img src="images/16-06.png">
<img src="images/16-07.png">
<img src="images/16-08.png">
<img src="images/16-09.png">
<img src="images/16-10.png">
<img src="images/16-11.png">
<img src="images/16-12.png">

Vamos a comenzar con un nuevo bloque podríamos decir de alguna manera que va a ser el bloque nuclear del curso porque además va a impregnar todo lo que veamos adelante la gran novedad junto con la expresión Irlanda de la versión 8 de Java y la verdad es que pretenden cambiarnos un poco el esquema de cómo programar orientándonos a realizar un código de bono pista en ocasiones bastante del código clásico al que estamos acostumbrados nos va a permitir trabajar con colecciones de información como si fueran secuencias de datos de manera que podremos hacer muy fácilmente operaciones de filtrado mapeo otra formación y reducción sobre esas colecciones de edad otra gran novedad es que a través de Fort Joy vale desde un marco de trabajo que también ofrece Java desde hace alguna persona no va a permitir también trabajar con extreme paralelos vale lo que van a ejecutarse en los diferentes y que puede que nos aporten cierta velocidad y el número de elementos a protestar es grande o nos permitirá trabajar de forma secuencial bueno pues sobre 1 y después el siguiente y el siguiente y el siguiente cómo podremos comprobar todas estas operaciones son transparente al programador es decir que van a pasar en alguna ocasiones casi desapercibidas para nosotros todo lo que va sucediendo por allí dentro y son una combinación perfecta con la interfaz de funcionales y por ende con las expresiones lambda un string como elemento fundamental de este API es una secuencia de elementos que soporta una serie de operaciones para procesar como elementos característicos podríamos decir que permite el uso de expresiones lambda que permite el encadenamiento de operaciones el perfil tendremos operaciones intermedias que retornarán siempre un string con lo cual podremos encadenar la llamada a método y que por ende vamos a producir código que sea muy legible vale bastante legible y mucho más conciso que se lo intentaremos además haciendo código comparativos decir código que no utiliza este código que si lo utiliza y comparar y además permite el procesamiento de los elementos de forma secuencial o para cenar cómo características de un string podríamos decir que la operaciones intermedias no retorna para hacer este encadenamiento además la operación es que se conocen como intermedia se van a encolar de forma que no se invocan hasta que no se invoca una operación de las conocidas como finales o terminales solamente se va a poder recorrer una vez es decir si queremos volver a recorrer los tenemos que volver a crear el tren y no ofrece el concepto de interacción interna frente a la iteración externa M-100 a la hora de integrar frente al ejemplo que hacíamos de interacción en las clases internas nosotros nos vamos a centrar en qué hacer con los datos y no en cómo se recoge no tenemos que indicar cómo se van a recorrer de eso se va a encargar el propio para trabajar con tipo de datos primitivos hemos visto antes que teníamos un interfaz genérico para stream ETT para trabajar con tipo de datos primitivos vamos a tener algunos subtipos básicos como son Quintín los Teen Titans go en streaming que no están parametrizado y qué sirven para trabajar con tipos de datos vamos a comenzar viendo las diferentes maneras quejaban ofreces de obtener un script que no son pocas la primera sería un interfaz perdón en el método de la interfaz stream que nos permite de una secuencia de elementos de un parar que va a recibir obtener un estrés vale que nos serviría para obtener un excelente de una serie de elementos conocidos vale un stream finito también podemos obtener un string a partir del método trim de la clase Array o método estático que va a recibir un array y nos va a devolver con los elementos de ese array con string secuencial si las rayas de tipo básico por ejemplo un array de int el método string devuelve intrinsic fuera de León o de doble de los tipos correspondientes aunque parezca extraño en ocasiones podremos querer obtener un screen vacío para ello tenemos el método de estático en ti que nos devolvería un string vacío vale sin ningún elemento otros métodos sería a partir del método iterate de la clase stream esté en este caso vale obtendríamos un string en principio sería infinito ordenado y secuencial qué bueno nos permite partir de un valor concreto y aplicar una función de normalmente de incremento o decremento sobre ese valor vale para obtener los elementos de Street lo normal es que limitemos el tamaño con el método limit vale que bueno que limita a un número máximo en el string infinito todos los elementos de la interfaz collection todos los perdón no los subtipos de colección por ejemplo ser las colecciones de tipo más también tienen un método en principio y otro para él el que nos devuelven un stream bien paralelo o bien secuencial con los elementos de esta colección también tenemos por último el método general te que acepta un supplier y que nos devolvería un string a partir de un supplier vale construyéndolo con Yolanda o una clase que implementará esta interfaz funcional como vemos tenemos una amplia gama de posibilidades para obtener un extreme nosotros casi siempre trabajaremos con los streams que nos proporcionen una determinada colección los string como hemos visto antes soporta operaciones intermedias operaciones terminal la intermedia son operaciones que afectan a un sprint pero que a su vez devuelven Electric vamos a ver algunas cómo son las de filtrado y Mateo la de filtrado que trabajaremos ampliamente en un vídeo nos permite filtrar los elementos de un string utilizando una condición que se proporciona como un predicado también tenemos la posibilidad de limitar un string a los n primeros elementos o de obviar los m primero elementos con el método key como otro tipo de operaciones intermedia también tenemos la operación este mapeo la principal operación de Mateo que tenemos es más que nos permite recibir una función y que te la formaría los valores de un string cada uno de los valores de un tipo en otro vale lo hemos comprobado en el ejemplo de la interfaz de la interfaz funcional con quién tenemos tipos también a valores básicos como son más Twins doble tú lo que nos permite transformar el valor que se recibe en datos básicos y que en lugar de bueno hemos dicho que la operación es intermedia retornan también un string en este caso cambiaría en el tipo de String a uno de los extremos de tipo básico cómo ir doble o no respectivamente aquí a un conjunto de operaciones intermedias que son de las manos por lo menos de las que inicialmente se utiliza por último por conocer algo sobre las operaciones terminales decir que como comentábamos anteriormente son las que provocan en primera instancia que se ejecuten todas esas operaciones intermedias que tenemos en Coslada podemos ver que son de varios tipos el primero es el consumo de los elementos el ejemplo más claro de oír el obtener los datos del stream de alguna otra forma o recolectar los elementos del Extreme para transformarlos en otro como una colección transformarlos en otro como una colección la de obtener los datos del stream y la de recolectarlos pues no estudiaremos en lecciones posteriores dentro de este bloque de la cistitis seamos algún ejemplo de todo esto que hemos ido comentando vamos a ver en particular 4 ejemplos en primer lugar de Amos la creación de Scream cómo lo podemos realizar con dicho que tenemos bastante formas si tenemos alguna de ella podríamos crear un stream de una serie de elementos aquí por ejemplo de 10 números el tipo podríamos tener un string a partir de un array vale en particular alquila estamos pasando un array también del uno al 10 y obtendríamos un string recordad que al llamar al método Scream de la clase arrays y esto era de un tipo básico que pudiera ser correspondido con un stream de los subtipos que ya hemos visto pues tendríamos el correspondiente en este caso entre vale que comenzaría en infinito inicialmente pero limitado a 100 elementos vale un string a partir de una colección fijado que utilizamos el método más listo de la clase Array para obtener una colección en este caso un lifting y del cual podríamos tener pues en este caso no tendría mucho sentido para tampoco elementos pero un stream paralelo o por ejemplo aquí podríamos tener un stream con generates vale utilizando podríamos crear un screen infinito utilizando una función generadora no que en este caso por ejemplo sería la función next in the random vale múltiples maneras diferentes de obtener Street por ejemplo si quisiéramos filtrar una serie de capitales que tenemos aquí utilizando el mecanismo de filtrado vale así parece que es más legible vamos a resumir en una línea con el formato automático recordad que podéis formatear para qué sirven te viene el código y seco lo que viene con control mayúscula bueno podríamos tener aquí un screen de las capitales que tienen sé cómo podemos comprobar filter lo que hace devolver un script vale y podríamos tener todas las capitales que tuvieron la letra C mediante esta expresión Landa vale conociste en el caso por ejemplo si quisiéramos obtener las capitales sin repetir podríamos llamar al método cistin que nos devuelve los elementos de un screen pero sin repetido en este caso operando sobre las capitales repetida stream tendría solamente a Sevilla y a Huelva Sevilla una sola vez si queremos tener un sprint limitado en el número de elementos de las capitales podrían obtener las primeras 5 Jaén Córdoba Sevilla Huelva y Cádiz y en el caso de que quisiéramos copiarlas 4000 me da lo que haría sería devolvernos todas las capitales a partir de qué haces vale Málaga Granada y Almería sobre capitales serían por ejemplo la operaciones de Mateo aquí en este caso también estamos haciendo uso de la referencia a funciones que conoceremos en otro sitio más adelante aquí estaríamos teniendo un string con las capitales en mayúsculas vale de manera que estaríamos protestando cada una de las capitales para mediante el método to uppercase obtener la Emma y en este caso por ejemplo podríamos utilizar el McQueen para calcular la longitud de la cada una de la cadena de caracteres que conforman el stream y guardarlo en un string de los que presentamos por último como operaciones terminales tenemos el método o incluso el metodo foreach order nos permitirían consumir los elementos de una p perdón los elementos de un string bueno y aquí tenemos alguno de los que hemos hecho con anterioridad para imprimirlo mediante mediante el un consumidor con folios no como operación terminal vale que tenemos por aquí en todos podemos comprobar como se imprimiría las capitales que empiezan por C las capitales sin repetir que teníamos aquí las 5 primeras capitales de Andalucía últimas capitales de Andalucía en función del orden del string mayúsculas la longitud de las capitales y aquí lo haríamos mapeando vale mediante 100 formas no solamente la longitud sino que obtendríamos el nombre de la capital la longitud indicando la palabra caracteres forma hemos presentado el API string ahora lo vamos a ir conociendo en profundidad vamos a comenzar con algunos métodos de búsqueda de datos

# 17 Métodos de búsqueda de datos 6:27 

[Métodos de búsqueda de datos](pdfs/17_Métodos_de_búsqueda_de_datos.pdf)

## Resumen del Profesor

### 17.1 Métodos de búsqueda

Son un tipo de operaciones terminales sobre un stream, que nos permiten:

* Identificar si hay elementos que cumplen una determinada condición

* Obtener (si el stream contiene alguno) determinados elementos en particular.

Algunos de los métodos de búsqueda son:

* `allMatch(Predicate<T>)`: verifica si todos los elementos de un stream satisfacen un predicado.
* `anyMatch(Predicate<T>)`: verifica si algún elemento de un stream satisface un predicado.
* `noneMatch(Predicate<T>)`: opuesto de `allMatch(...)`
* `findAny()`: devuelve en un `Optional<T>` un elemento (cualquiera) del stream. Recomendado en streams paralelos.
* `findFirst()` devuelve en un `Optional<T>` el primer elemento del stream. *NO RECOMENDADO* en streams paralelos.

## Transcripción

<img src="images/17-01.png">
<img src="images/17-02.png">
<img src="images/17-03.png">
<img src="images/17-04.png">

Hola vamos a conocer con respecto a los API Street en la piscin los métodos de búsqueda de datos que nos ofrecen en este vídeo y lo sucesivo vamos a ir viendo en diferente operaciones que podemos hacer intermedio terminales con los streams de forma que al final del bloque podremos ir componiendo esta operaciones según nos interesa los métodos de búsqueda de la peste son operaciones terminales y nos van a permitir identificar si hay elementos que cumplen una condición y obtener los pies que el string contiene elementos vale algún elemento en particular tenemos algunos métodos de búsqueda como por ejemplo es por más vale que recibe un predicado y que verifica y todos y cada uno de los elementos del Sting satisfacen es el predicado verifica si alguno de los elementos del street al menos uno satisface es el predicado y no más es el opuesto a all mad here decir que verifica que todos los elementos del stream no cumplen con ese predicado vale tenemos también otros métodos como son para ingenii findfirst que nos van a permitir devolver un elemento de un string en particular es muy recomendado en el caso de los Teen paralelo findfirst sería más recomendado en el caso de veloz sprint secuenciales no nada recomendado para el caso de los estoy en paralelo porque redundaría entonces en problemas de rendimiento ambos dos devuelve una instancia de opcional sobre el tipo de dato de sobre el que se ha definido el tren y queréis conocer más sobre la clase opcional que también es nueva de la última persona dejada también tenéis una píldora en Youtube sobre sobre el canal vamos a ver algún ejemplo de estos métodos de búsqueda volvemos a tener las capitales de provincia de Andalucía y podemos ir utilizando los diferentes métodos hermana va a ser un método que devuelva un valor Boolean recibe un predicado predicado en este caso sería bueno para la capital queremos quedarnos con todo aquello que tengan una longitud de 4 o más caracteres y lo podríamos guardar en un valor Boolean lo podríamos utilizar también como parte de este otra estructura vale aquí construimos el mensaje de longitud y todas las capitales tienen 4 o más caracteres pues imprimir este mensaje y si no pues en primer mensaje hay al menos alguna capital con menos de 4 caracteres vale tienen 4 o más caracteres desde en adelante recordaste los bloques de comentarios se pueden poner con control siete al igual que así como se pueden quitar si queremos verificar si alguno cumple una determinada condición podríamos pedir con en IMAX un predicado y decir oye hay alguno que sea igual independientemente de fiestas mayúscula o minúscula que Jaén vale que lo podríamos guardar en un valor Julián capital decíamos norma tiene como inversa el perdón en la inversa de Olma y podríamos decir oye se cumple que ninguna tenga 4 o más caracteres entonces podríamos ver que bueno al menos hay alguna capital en mis puntos son todas que tienen 4 caracteres, no por último cómo que podíamos utilizar los métodos find me if I'm old find any file first que devuelven un opcional vale y bueno suponiendo que hiciéramos un tratamiento con ESPN paralelos que es donde tiene más recomendación aunque sean solo 8 elementos fine lo que hace es de volvernos algún elemento del state localiza un elemento cualquiera te lo devuelven una opción al recordar que la clase opcional bueno fue es un contenedor que nos va evitar errores de tipo NullPointerException nos permite comprobar si tiene un objeto dentro o no lo tiene vale mediante el método y presente y tiene alguna serie de método que como yo digo si queréis visitar la píldora sobre opcional la podréis conocer mucho mejor capital cualquiera y mediante el método Urgel podríamos indicar que bueno si no está la capital pues en el script no queda capital de ningún tipo opondrían usar el método spain-first para obtener la primera capital de todas en este caso fue una cualquiera sería Málaga y bueno teníamos por otro lado Jaén vale que sería la primera de todas hemos conocido los métodos de búsqueda nos vamos a lanzar ahora a conocer una serie de métodos que son las de tratamiento de datos y métodos de cálculo y métodos de ordenación


# 18 Métodos de datos, cálculo y ordenación 10:25 

[Métodos de datos, cálculo y ordenación](pdfs/18_Métodos_de_datos_cálculo_y_ordenación.pdf)

## Resumen del Profesor

### 18.1 Métodos de datos y cálculo

Los streams nos ofrecen varios tipos de métodos terminales para realizar operaciones y cálculos con los datos. Durante el curso trabajaremos con tres tipos:

* Reducción y resumen (en esta lección)
* Agrupamiento
* Particionamiento

### 18.2 Métodos de reducción

Son métodos que reducen el stream hasta dejarlo en un solo valor.

* `reduce(BinaryOperator<T>):Optional<T>` realiza la reducción del Stream usando una función asociativa. Devuevle un Optional
* `reduce(T, BinaryOperator<T>):T` realiza la reducción usando un valor inicial y una función asociativa. Se devuelve un valor como resultado.

### 18.3 Métodos de resumen

Son métodos que resumen todos los elementos de un stream en uno solo:

* `count`: devuelve el número de elementos del stream.
* `min(...), max(...)`: devuelven el máximo o mínimo (se puede utilizar un `Comparator` para modificar el orden natural).

### 18.4 Métodos de ordenación

Son operaciones intermedias, que devuelven un stream con sus elementos ordenados.

* `sorted()` el stream se ordena según el orden natural.
* `sorted(Comparator<T>)` el stream se ordena según el orden indicado por la instancia de `Comparator`.

## Transcripción

<img src="images/18-01.png">
<img src="images/18-02.png">
<img src="images/18-03.png">
<img src="images/18-04.png">
<img src="images/18-05.png">

Hola todo vamos a continuar conociendo el lápiz tren para conocer los métodos de gato de cálculo y ordenación no string no ofrecen métodos terminales para hacer múltiple operaciones con los datos trabajar con tres tipos los de reducción y resumen los métodos de agrupamiento y particionamiento el agrupamiento y particionar yo y particionamiento lo trabajaremos más adelante en en sucesivos vídeo vamos a ver ahora los métodos de reducción y resumen con respecto a los métodos de reducción tenemos alguna implementaciones del método review que nos permite reducir un string a un solo valor mediante un operador de reducción vale se trataría de una función asociativa supone por ejemplo en el primero que siempre se viene a la cabeza que tenemos una excelente números y queremos quedarnos por ejemplo con el máximo vale pues lo podríamos hacer de esa manera también tenemos la reducción usando un valor inicial ni una función asociativa por ejemplo la suma de todos los elementos de un array la podríamos hacer de esta manera no añadiendo en este valor inicial puesto dolores también algunos métodos de resumen por recuerdo que son operaciones terminales como son con que devuelve el número de elementos del Scream que nos devolvería en el mínimo el máximo vale encapsulado dentro de una opcional vale bueno también tenemos que tener presente que la interfaz Comparator también nos va a ayudar con algunos métodos estáticos en este. Tenemos también en los métodos de ordenación que en este caso serían operaciones intermedias vale y que nos permitiría vale no hemos querido meter también en el saco de este vídeo que nos permitiría hacer una ordenación de los elementos del stream mediante el orden natural o la interfaz coja vamos a ver algunos ejemplos de ello cómo sería la reducción aquí tenemos un método estático que nos va a generar un array aleatorio de 100 elemento de números enteros entre 0 y 1000 string ha impregnado no solamente lo que tenemos dentro de su paquete y no que ha impregnado clases que ya eran archiconocida la interfaz perdón en la clase random vale incluye desde la versión 18 un método que se llama bien que devuelve un intrim que acepta tres parámetros el primero es el número de elementos que va a tener eléctrica y después el intervalo mediante dos número de en el que se quieren embarcar los números aleatorios que se generen tiene a su vez un método que nos permite obtener todos los elementos de String en un array en este tipo de centeno con local usando flujos también podremos tener una raíz rápidamente número aleatorio entre 0 y 1000 haríamos ese método estático para obtener aquí en la RAE cómo arreglar estamos haciendo uso del método to String que lo que haces de volvernos para un array de enteros una cadena de caracteres que representa a todas a todo el Ferrari cómo podemos comprobar no quisiéramos reducir stream para quedarnos solamente con un valor como digo deberíamos proporcionar al método reviews 1 una expresión lambda o una interfaz de del tipo en vinari operator por tratarse de un stream de entero en este caso vale sería una un operador binario que nos permitiría hacer está está reducción en este caso lo que va haciendo es quedarse solamente con el Máster de manera que nos quedaríamos con este máximo en opcional tenemos creador las clases correspondientes tenemos el método imprenta que nos permite saber pie máximo que de tipo penal tiene valor o no y el método el que hacen que no lo devuelve como valor entero el valor máximo en 990 como son una cantidad considerable de número aleatorio siempre estará rozando el 900 y pico no es muy muy raro que salga mapa una operación de reducción con una función asociativa podemos usar tal y como hemos visto antes también vale con un valor inicial en este caso proporcionamos la función asociativa x sí que nos va devolviendo la suma y empezando espero que el acumulador natural de la suma podríamos tener la suma de todos los elementos podremos tener también el producto o cualquier otro tipo de operación en el caso de la reducción también que había alguna serie de operaciones de datos como por ejemplo montar el máximo y mínimo podríamos hacer lo mismo con un número un array de números aleatorios de tipo double el método doble que funciona igual que hemos visto antes y podríamos de un array en particular en un extremo y obtener el número de elementos el máximo o el mínimo mediante estos métodos directamente vale los correspondientes de uno de un string de una clase que no sean tipo primitivo requerirían de que le proporcionemos un comparador un contrato vale esto devuelven también una opción aquí podemos llamar al método por el vale para que no te volviera el valor 0 tenemos también la posibilidad de una tacada de calcular la suma en el caso de los de los stream de tipo más básico vale y podríamos imprimir aquí todo el array con lo elemento en una sola línea el máximo perdón el número de elemento de 100 lo sabíamos porque lo hemos generado nosotros aquí en el valor máximo es 986 81 es 26,36 olvidado añadir comparar la longitud de las palabras la palabra más larga no pues de todas las palabras que tenemos aquí la más larga es toda esta palabra qué es casi impronunciable no que un compuesto de química orgánica por último la operaciones intermedias de ordenación que las queríamos conocer ahora nos van a permitir a partir de un array aleatorio de número entero hacer una ordenación sencilla fijado como aquí podemos construir un screen sobre la raíz ordenarlo y volver a obtener un array ordenado y en el caso de querer ordenar un stream de objetos de otro tipo deberíamos proporcionar un comparato fija como aquí en una sola línea de código obtenemos un screen a través de una lista de entero lo ordenamos por orden alfabético inverso y lo imprimimos por consola empezaría por Sevilla Málaga Jaén Huelva terminando en Almería que empieza por la I en el caso de los número tenemos el array que los títulos han equivocado elementos de la RAE se han ordenado perfectamente frente al primer arranque obviamente al ser generado de forma aleatoria estaba fenomenal en el siguiente vídeo aprenderemos alguna de las operaciones más complicadas con el API stream cómo son las operaciones de Mateo con plasma y colector


# 19 Uso de Map y flapMap 12:04 

[Uso de Map y flapMap](pdfs/19_Uso_de_map_flatMap_y_Collector.pdf)

## Resumen del Profesor

### 19.1 Uso de map

`map` es una de las operaciones intermedias más usadas, ya que permite la transformación de un objeto en otro, a través de un `Function<T, R>`. Se invoca sobre un `Stream<T>` y retorna un `Stream<R>`. Además, es muy habitual realizar transformaciones sucesivas.

```java
lista
   .stream()
   .map(Persona::getNombre)
   .map(String::toUpperCase)
   .forEach(System.out::println);
```

### 19.2 Uso de `flatMap`

Los streams sobre colecciones de un nivel permiten transformaciones a través de `map` pero, ¿qué sucede si tenemos una colección de dos niveles (o una dentro de objetos de otro tipo)?:

```java
public class Persona {

    private String nombre;
    private List<Viaje> viajes = new ArrayList<>();

  //resto de atributos y métodos
}
```

Para poder trabajar con la colección interna, necesitamos un método que nos unifique un `Stream<Stream<T>>` en un solo `Stream<T>`. Ese es el cometido de `flatMap`.

```java
lista
   .stream()
   .map((Persona p) -> p.getViajes())
   .flatMap(viajes -> viajes.stream())
   .map((Viaje v) -> v.getPais())
   .forEach(System.out::println);
```

También tenemos disponibles las versiones primitivas `flatMapToInt`, `flatMapToLong` y `flatMapToDouble`:

```java
Arrays
    .stream(numeros)
    .flatMapToInt(x -> Arrays.stream(x))
    .map(IntUnaryOperator.identity())
    .distinct()
    .forEach(System.out::println);
```

## Transcripción

<img src="images/19-01.png">
<img src="images/19-02.png">
<img src="images/19-03.png">
<img src="images/19-04.png">
<img src="images/19-05.png">
<img src="images/19-06.png">
<img src="images/19-07.png">
<img src="images/19-08.png">
<img src="images/19-09.png">
<img src="images/19-10.png">
<img src="images/19-11.png">

Vamos a continuar trabajando con el API spring y en este caso nos vamos a centrar en operaciones de transformación de datos y de recolección de los pies comencemos hemos hablado en algunos vídeos anteriores de la operación de transformación la presentábamos de hecho cuando conocíamos la interfaz funcional. Una de las operaciones intermedias más utilizada ya que permite transformar un objeto en otro recibe como argumento una instancia de una interfaz funcional qué es función y que se define sobre dos tipos el tipo de dato que acepta y un tipo de dato de retorno es muy interesante que seamos conscientes de que cuando invoquemos más siempre lo haremos sobre un string de un tipo determinado y que tras la invocación lo que obtenemos es otro stream pero en este caso de datos trabaja además podemos encadenar una llamada de más con otra al más puro estilo de la piscina aquí por ejemplo tenemos un fragmento de código en el que sufre una lista de instancia de la clase persona que incluirían fue unos cuantos atributos podríamos hacer una transformación para obtener primero los nombres de esas personas y posteriormente pasarlo a mayúscula antes de que lo imprimimos por consola a la derecha cómo poder comprobar o pongo el tipo de retorno que tendría que dar uno de los métodos y como cada llamada a Matt hace una transformación del stream al tipo de datos correspondientes en este caso ambas llamadas no se volverían un stream de streamplay cadena de cárcel sobre la clase persona que bueno es una clase normal y corriente qué feliz se termine en este caso tres atributos privados y no me lo ha pedido y la fecha de nacimiento pues podríamos hacer una serie de transformaciones la primera en la que hemos visto antes en la play vale anda que obtendríamos el nombre y lo pasaríamos a mayúscula en este caso a ti en el código lo he hecho con una expresión Landa menos concisa vale la referencia métodos las veremos en alguno de los vídeos de este bloque pero el efecto sería el mismo no nos permitiría transformar mapear una persona aquí está impidiendo el tipo de datos automáticamente desde el contexto vale porque estamos trabajando con un stream de persona por lo para aquí por defecto está aceptando como podemos ver una función de persona vale y el tipo de retorno al ser un script porque lo marca el tiempo de retorno de este método como podemos comprobar estamos trabajando sobre bueno pues persona que se transforma en un stream de cadenas de caracteres en este caso como el tipo que recibimos y el tipo que retornamos es el mismo fue seguiríamos teniendo un stream de cadena de caracteres para después poder imprimirlas por consola también otro sencillo código de transformación en este caso a otro tipo de dato distinto podríamos mapear para obtener la fecha de nacimiento y a partir de la fecha de nacimiento como local de ir podríamos tener solamente el día y el mes para bueno puede hacer una lista de fecha de cumpleaños de las personas coma en primer en primer lugar hemos tenido el nombre de las personas las personas que tenemos aquí solamente el nombre mayúscula y en este caso como estamos teniendo de las personas la fecha de nacimiento y de la fecha de nacimiento y solamente el día y el mes en un formato de cadena de caracteres como veis más es una operación que es sencilla de implementar pero que he pasarte potencia además me demás tenemos la posibilidad de usar flatmap y alguno de vosotros en vuestros primeros usos de la Play stream pues casi que no seréis capaces de diferenciar las pero que tras ver este vídeo nos quede ninguna como hemos comprobado antes cuando trabajamos con una colección podríamos decir que de un solo nivel es decir una lista de un tipo de entidad como persona como la que hemos visto antes pues sería sencillo y transformarlo con más me echo podemos hacer transformaciones sucesivas pero qué sucede con colecciones que tienen más de pelis o con entrada que son formadas por clave valor o si una colección incluye dentro otra aquí vamos a modificar la clase persona para gestionar una serie de viajes que haga cada una de ellas una persona podrá hacer varios viajes como podemos ver aquí a continuación que tendríamos que hacer para poder trabajar sobre los viajes no solamente de una persona y no de todas las personas enfoque clásico sería si quisiéramos fuerte imprimir todos los viajes sola sería hacer un bucle anidado para cada persona la implementación sería un poco más complejas principio podríamos plantearnos hacer este doble bucle ir almacenando en un set los viajes y después recorrer el test para imprimir bueno pues el API string nos proporciona otra operación que es plasma que nos va a permitir para esta operaciones de transformación que nos devuelven un stream de Steam de otra clase aquí por ejemplo si sacáramos los viajes de cada persona tendríamos un stream de Steam de viaje nos va a permitir unificar todos esos stream en unos zuecos vale así como podemos ver en pantalla tendríamos los viajes de la primera persona lo de la segunda y la tercera en bueno en una estructura de stringstream podríamos unificarlo con flatman todos en un solo stream de viaje con lo cual vamos a poder seguir haciendo el resto de operaciones con normalidad aquí tendríamos un ejemplo de cómo hacer como habíamos hecho antes con el Google el doble bucle for un recorrido de bueno de todas las personas es traerlos viaje unificar todos los viajes mediante flatmap a partir de ahí obtener los viajes que teníamos y sacar el país para poder imprimirlo los países por consola es una operación muy útil porque nos va a permitir unificar todos esos individuales en uno solo como no podía ser de otra manera también tenemos los métodos correspondientes de transformación a a entero a doble para trabajar con los tipos de datos primitivos no como podemos ver aquí si tuviéramos una matriz bidimensional con número y quisiéramos obtener los números diferentes podríamos hacer una unificación de todos esos stream en uno solo o un plasma twin de manera que posteriormente podríamos obtener sobre los distintos invocando al método veamos algún ejemplo supongamos que tenemos la clase persona y la clase viaje las bacterias muy sencillas tenemos que cenar que determinadas personas viajan algunos países y el viaje incluye solamente el país y las personas tendrían su nombre y la lista de viajes que han realizado en el fondo de la lista de países que han visitado supongamos que tenemos estos datos de aquí donde la primera persona ha viajado a todos estos países la segunda persona viajado a todo esto y la tercera a eso que como podéis ver algunos viajes están solapados no lo podríamos retornar dentro de una lista de personas cómo os decía al principio de la aplicación de plasma si quisiéramos hacer un recorrido de esos países a la vieja usanza tendríamos que hacer un bucle for 2 bucles For anidados uno dentro del otro sería una estructura clásica y no sería totalmente funcional sin embargo como decía como podríamos hacer todo eso son con países diferentes no requeriría un poco más fuerte sin embargo hacerlo con poner API stream va a ser muy sencillo desde la lista obtenemos el stream más veamos las personas en su viaje con flagma construimos un stream con todos los viajes vapeamos entonces el viaje para obtener los países los filtramos para que sean distintos y lo imprimimos por vosotras vamos a ver el ejemplo no para montar está segunda parte imprimirlos todo esfuerzo pero como aquí haría un filtrado para que no aparezcan los países vale de aquí podemos hacer lo mismo pero no entero una matriz bidimensional número mate haríamos vale cada uno de los arrays unidimensionales que conforman el array bidimensional para obtener un string del mismo obtenemos con este mapeo con la función identidad cada uno de los elementos para después filtrarlo e imprimirlos por consola si os dais cuenta van a parecer solamente unos pocos números de todos los que tenemos lo único que hay diferentes dentro de los dos arrays unidimensionales que conforman el bidimensional con uno dos tres cuatro y cinco para quitar los repetidos de forma que ya no aparecería plasma en una operación bastante potente y que nos va a ayudar a poder trabajar con colecciones que tengan más de un mes


# 19 Uso de la clase Collector 19:28 

[Uso de la clase Collector](pdfs/19_Uso_de_map_flatMap_y_Collector.pdf)

## Resumen del Profesor

### 19.3 Collectors

Los *collectors* nos van a permitir, en una operación terminal, construir una collección *mutable*, el resultado de las operaciones sobre un *stream*.

### 19.3.1 Colectores “básicos”

Nos permiten operaciones que recolectan todos los valores en uno solo. Se solapan con alguans operacinoes finales ya estudiadas, pero están presentes porque se pueden combinar con otros colectores más potentes.

* `counting`: cuenta el número de elementos.
* `minBy(...)`, `maxBy(...)`: obtiene el mínimo o máximo según un comparador.
* `summingInt`, `summingLong`, `summingDouble`: la suma de los elementos (según el tipo).
* `averagingInt`, `averagingLong`, `averagingDouble`: la media (según el tipo).
* `summarizingInt`, `summarizingLong`, `summarizingDouble`: los valores anteriores, agrupados en un objeto (según el tipo).
* `joinning`: unión de los elementos en una cadena.

### 19.3.2 Colectores “grouping by”

Hacen una función similar a la cláusula GROUP BY de SQL, permitiendo agrupar los elementos de un stream por uno o varios valores. Retornan un `Map`.

```java
Map<String, List<Empleado>> porDepartamento =
   empleados
     .stream()
     .collect(groupingBy(Empleado::getDepartamento));
```

Se pueden usar en conjunción cno los colectores básicos, o con otro colector *grouping by*:

```java
Map<String, Long> porDepartamentoCantidad =
   empleados
      .stream()
      .collect(groupingBy(Empleado::getDepartamento, counting()));

Map<String, Map<Double, List<Empleado>>> porDepartamentoYSalario =
   empleados
     .stream()
     .collect(groupingBy(Empleado::getDepartamento, groupingBy(Empleado::getSalario)));
```

También tenemos los colectores *partitioning*, que nos agrupan los resultados dos conjuntos, según si cumplen una condición:

```java
Map<Boolean, List<Empleado>> salarioMayorOIgualque32000 =
   empleados
     .stream()
     .collect(partitioningBy(e -> e.getSalario() >= 32000));
```
  
### 19.3.3 Colectores “Collection”

Producen como resultado una colección: List, Set y Map.

```java
Set<Empleado> setEmpleados = empleados.stream().collect(Collectors.toSet());
List<Empleado> listEmpleados = empleados.stream().collect(Collectors.toList());
Map<String, Double> mapEmpleados = empleados.stream().distinct()
                .collect(Collectors.toMap(Empleado::getNombre, Empleado::getSalario));
```

## Transcripción

<img src="images/19-12.png">
<img src="images/19-13.png">
<img src="images/19-14.png">
<img src="images/19-15.png">
<img src="images/19-16.png">
<img src="images/19-17.png">
<img src="images/19-18.png">
<img src="images/19-19.png">
<img src="images/19-20.png">

,, el último apartado de capítulo versará sobre colector novedad y la propia intuición como programadores no invitaba a que existiera algo así todas las operaciones que hemos venido realizando hasta ahora con el API string han finalizado con una operación terminal de salida por consola algo que didácticamente puede ser útil pero que la práctica es posible que no lo sé Ana stream filtrado Mateo reducción y guardar su resultado en una entidad que fuese mutable porque ya decíamos en el primer vídeo sobre las 10:30 que los streams eran inmutable y si quisiéramos tener una colección en una colección de elementos trajese filtrado más feo y reducción para ello el API stream nos proporciona una operación que el cole y qué es una operación terminal y nos va a permitir recolectar todos los elementos que queden en este momento en el estén en bueno un contenedor saber cómo montar este contenedor produce una clase que tiene una gran cantidad de métodos estáticos y que bueno pues son muy usuales y de hecho ya veréis que son súper prácticos para hacer un montón de de operaciones dado que son estáticos si queremos importar para evitar usar conector punto y el nombre de los métodos qué crema usar directamente el nombre de los métodos podemos hacer una importación estática de esa manera de la forma que ya solamente tendríamos que invocar a los métodos estáticos de conectar sin anteponer el nombre colector con la importación automática que hace triste no suele trabajar de esta manera está la tendrías que hacer manualmente tipos de colectores porque la verdad es que tiene bastante podríamos hablar primero de una serie de conectores básicos estos colectores nos van a permitir a partir de un string reducirlos para obtener unos sol algunos de estos semanas o la paz con operaciones que ya hemos visto como por ejemplo calcular el máximo o mínimo la suma o incluso contar porque existe aparte de la otra operación es que hemos visto por fin veremos que los colectores se podrán componer de alguna manera sobre todo en los grupos punk de forma que es ahí donde cobra sentido estos conectores paso aunque vamos a verlo individual ojo different pantalla tenemos alguno que bueno son bastante intuitivos no vamos a poder la mayoría de ellos están en erupción vale vamos a comprobar cómo podemos contar los elementos de obteniendo un long vale como resultado de la operación el mínimo el máximo según una instancia de compara todo una expresión lambda la posibilidad de sumar todos los elementos de tipo entero de tipo de tipo doble de calcular la media método empate potente que es el de fumar Aizen in long o doble que nos va a permitir bueno pues toda esa operación es que hemos visto antes de forma conjunta un conteo el máximo el mínimo la suma y la media con lo cual podemos hacer una primera aproximación podríamos decir de Estadística Descriptiva muy muy sencilla y con un solo método por ejemplo para cadena de caracteres tendríamos la operación joining que nos uniría todos los elementos del string en una cadena de caracteres y separadas por un carácter que nosotros le podríamos proporcionar igual también en muchas ocasiones puede vamos a ver un ejemplo de estos conectores básicos el último apartado de capítulos sobre colector de la colección Brando perdón la clase random y como también ha sido modificada sido impregnada de SEAFI de Steam desde la versión de Java este y de hecho para comenzar el ejemplo ya empezamos con parte del ejemplo porque lo que vamos a hacer es obtener una serie de números enteros aleatorios entre 0 y 100 en particular 20 elementos que los queremos obtener no como un tipo entero es decir no como un tipo primitivo sino con el correspondiente tipo integra vale eso lo podemos hacer con boxer porque como podemos comprobar aquí in devuelve un instream lo mismo que sucede con la operación límite y si queremos transformar extintor en un sprinter de integra pues tendremos que hacer el boxer explico la operación que es una operación terminal vamos a construir una lista a partir de este tren los colectores hacia colecciones los veremos en este vídeo un poco madre aquí podemos ver cómo funcionan estos colectores básicos no cómo podríamos recolectar todos los elementos de números para saber cuántos hay calcular su máximo Mac el colector macbike va a devolver un opcional de 20 que vale ya hemos hablado de él con anterioridad y bueno si quieres conocer más de la clase opcional tenéis una píldora en Youtube podríamos sumar todos los valores para obtener la suma de esta manera sería del tipo entero calcular la media como podéis comprobar podemos usar diferentes formas de llamada a la función e summarize en va a ser bastante interesante porque va a alucinar un montón de métodos los vamos a poder comprobar o cómo podríamos juntar todos los elementos en una cadena de caracteres separadas por comas vale también bastante bastante foto comprobar como tenemos que son 20 elementos en la colección que el máximo bueno son aleatorio entre 0 y 1931 que la suma son 8970 está en la media el mínimo es 6 y el número de elemento de la colección pues los a concatenado los 20 elementos en una cadena de caracteres vale como decía si quisiéramos usar de res alguna operación más podéis comprobar como red nos va a permitir utilizar un montón de métodos como tomarla media el máximo el mínimo la suma vale todo en una sola clase vale que este tipo instalar istatistik vale la el problema decir que un resumen de Estadística gente estores básicos otro tipo de conectores muy interesantes son los grouping by si alguno venir de trabajar con con SQL manejo sonara a bueno tendrá un sonido parecido a la cláusula GROUP BY deben de las tendencias Celes de Secue van a ser unos conectores que nos van a permitir hacer agrupamientos de un valor con una función asociativo una función que reduzca esos valores por ejemplo el primer agrupamiento que tendríamos más sencillo sería agrupar por un valor de la clase de Electric no supongamos que tenemos un sprint en empleado dentro de los empleados guardamos el departamento y podremos decir pollo pues devuélveme una lista trufada de departamento y bueno la primera manera de trabajar sería devuélveme y que dentro tengamos pues los diferentes empleados si son las pinturas que devuelve porque va a ser muy similar en la mayoría de las ocasiones si usamos un colector de tipo Group invite lo que vamos a tener siempre va a ser un magma de hecho si queremos hacer algún tipo de versión más avanzada veremos cómo podemos escoger el tipo de mar que queremos tener vale porque por defecto utilizar ajá aquí obtendríamos un que te queremos tener vale porque por defecto utilizar ajá aquí obtendríamos un Mac del tipo de retorno de la función que te partamento que en este caso sería un string tiene el segundo nivel del mar cómo sería la clave cómo valoro obtendríamos una lista de el tipo de dato del Stinger de esta forma estaremos reduciendo o mejor dicho agrupan a todos los empleados según el departamento en el que esté como me iré pues un elemento bastante potente para trabajar con los colectores van a poder ser compuestos con lo cual podemos modificar el tipo de datos que estamos obteniendo al final si quisiéramos hacer algún tipo de operación de recolección para obtener los empleados por departamento pero en lugar de obtener la lista de empleados obtener el número podríamos componer el colector grouping by con el uno de los conectores Vasco por ejemplo son sin que nos permitiría saber para cada departamento cuántos empleados empleados tiene fijado en el tipo de retorno de metro de recolección el tipo sobre el que definimos el magno sería stream por ser el departamento y con fin que devolvería un loft como podéis comprobar son bastante potente podemos crear a mamá diferentes niveles de agrupamiento estas estructuras comienzan a ser algo más compleja pero responde también a la misma dinámica y adiós y algunos venís del mundo es secuela de bueno de la sentencia SELECT con GROUP BY y en el ir diferentes niveles de equipamiento y si quisiéramos agrupar a los empleados en primera instancia por el departamento y para de cada departamento que hiciéramos diferenciarlo por salario es muy habitual hacerlo en ese pueblo aquí también lo tendríamos para trabajar con un string de manera que haríamos primero un agrupamiento por departamento y dentro de esa cruzamiento lo habíamos por salario fijado en la estructura qué obtenemos para cada departamento tendríamos un más que nos permitiría agrupar a los empleados por su salario mediante doble para obtener la lista de empleados de esta manera tendríamos los dos niveles de agrupamiento vamos a verlo en un ejemplo reparta mento vale y tenemos aquí los diferentes ejemplos que hemos utilizado para agrupar aquí haríamos un primer agrupamiento cómo podéis comprobar el looping bike viene porque he hecho manualmente la importación estática de Java útil stream collector's y todo eso método están definidos aquí dentro vale utilizando la referencia a método ballet grouping by cómo podéis comprobar lo que recibe es una función que permite clasificar a los empleados y el tipo de retorno que tiene vale pues sería en particular del método cómo puedes comprobar sería un map del tren y un listado de esquiar función del agrupamiento que estamos haciendo aquí podríamos imprimir mediante un for it que aceptaría en este caso en lugar de un consumer aceptaría un derivado suyo un PI consumer elegir un consumer que afecta los valores nos viene perfecto para poder recorrer una estructura de tipo clave valor donde la clave sería el departamento y aquí como volvemos a utilizar otro colector vale para obtener todos los nombres en una sola cadena de caracteres separadas por comas apartado a Pepe y a Juan Antonio y a María tiene el Departamento Ejecutivo estaría solamente Manuel vale quisiéramos hacer agrupamiento para obtener un valor que no fuese la lista de empleados embestir por ejemplo el número de empleados por departamento lo podríamos tener aquí abajo vete haciendo empleados compras y en otros dos empleados y el Departamento Ejecutivo tendría un empleado podemos añadir dos niveles de agrupamiento no que se hace nuestra Cole se le pasen dos colectores inox group in bike acepta una función de clasificación y otro colector vale tener cuidado con la sintaxis porque es un error muy habitual será tarde de poner dos colectores al método cole cuando realmente hay que poner colectores compuestos aquí tendríamos para imprimir el los dos niveles pues tendríamos que hacer dos folios vale sería la mejor manera para poder imprimirlo podemos comprobar como vamos los elementos por departamento y suelto tendré izquierdo tendríamos el departamento de venta el departamento de compra los empleados cobran lo mismo y en el Departamento Ejecutivo también agrupado por su salario otro tipo de conectores que están relacionados con los guppys son los de particionamiento son similares porque nos van a permitir agrupar los valores pero en función de un predicado de forma que podríamos crear dos particiones sobre el stream / los elementos que si cumple la condición y no cumplen la condición por ejemplo si quisiéramos obtener agrupados por un lado los empleados que cobran 32000 € o más y los que no podríamos hacerlo con un por un predicado en este ejemplo de aquí podemos ver como el particionado lo que recibe como argumento es un predicado vale en este caso un predicado sencillo en el que comprobamos que el salario sea mayor o igual que 32000 el tipo de retorno va a ser un más pero el primer tipo de dato será un Boolean obtendremos siempre un último tipo de colectores que seguramente utilicéis muy a menudo en el inicio son los conectores de tipo colección y son vamos ya veíamos en el primer ejemplo de uso no son aquellos que nos permiten recolectar todos los elementos en una colección conocida como un list un cepo uma algún ejemplo pero podemos ver aquí alguno más en el que a partir de los mismos empleados de antes podríamos tener 11 vale y podríamos imprimir estos empleados un Live y podríamos tener en un Mac lo empleado en este caso vamos a tratar de filtrar los que sean distintos para introducirlo en el más vale si no fueran distintos sino lo aseguramos estar recolección en 1 más podría darnos algún problema inicialmente cómo podemos comprobar reprimo directamente el filtrado de manera que Pepe no va a parecer lo pensé vale te echo fijado como no asegura nada sobre el orden Pepe aparece solamente una vez en el link que permite repetidos y aparecerían pues Pepe más de una vez además en este caso no lo está devolviendo en el orden de inserción y como en el mar lo que nos devuelve una estructura de tipo clave-valor con el tipo de dato string para el la clave y el para el valor el tipo de dato double qué es lo que espera en el método tú más no dos funciones el mapeador para la clave y el mapeador para el balón con esto terminamos este este vídeo sobre la operación en Maps flatmap y colector vale vamos a seguir trabajando con los trim en este caso no es la operaciones de filtrado


# 20 Uso de streams y filtros 8:27 

[Uso de streams y filtros](pdfs/20_Uso_de_streams_y_filtros.pdf)

## Resumen del Profesor

### 20.1 `streams` y `filtros`

`filter` es una operación intermedia, que nos permite *eliminar* del stream aquellos elementos que no cumplen con una determinada condición, marcada por un `Predicate<T>`.

```java
personas
      .stream()
      .filter(p -> p.getEdad() >= 18 && p.getEdad() <= 65)
      .forEach(persona -> System.out.printf("%s (%d años)%n", persona.getNombre(), persona.getEdad()));
```            
            
Es muy combinable con algunos métodos como `findAny` o `findFirst`:

```java
Persona p1 = personas
                   .stream()
                   .filter(p -> p.getNombre().equalsIgnoreCase("Andrés"))
                   .findAny()
                   .orElse(new Persona());
```

Y se puede usar también en streams sobre colecciones tipo `Map`.

```java
Map<Integer, Persona> personas = new HashMap<>();
//Inicialización
personas.entrySet()
            .stream()
            .filter(map -> map.getKey() >= 5)
            .collect(Collectors.toMap(p -> p.getKey(), p -> p.getValue()))
            .forEach((key, value) -> System.out.printf("%d: %s%n", key, value.getNombre()));
```

## Transcripción

<img src="images/20-01.png">
<img src="images/20-02.png">

Vamos a continuar con el API stream ahora en particular por las operaciones de filtrado está operaciones para ser muy sencillas hacer el filtrado es una operación intermedia es decir de las que se invoca antes de una operación térmica funcionar a modo de colador nos va a permitir filtrar y eliminar aquello elementos eléctricos que no cumplen la condición marcada por un determinado predicado como hemos visto porque ya lo hemos venido utilizando cruce en anterior escribió esto va a ser bastante útil además va a ser muy combinable con los métodos para enfermo Fainé y también vivo en uno de los vidrios anterior como no podría ser de otra manera también va a ser muy combinable con métodos como el quemó en el anterior vídeo de recolección de alimentos de manera que podríamos sobre una colección trabajarla común entre filtrar lo de elementos no queramos tener y devolver todo eso elemento en una nueva colección veamos un sencillo ejemplo en el que además vamos a trabajar supongamos que tenemos una persona y un identificador para cada una de ellas vale simplemente un número del 1 al 10 persona vamos a trabajar con con un con el la clase persona vamos a tener su nombre y su fecha de nacimiento hacer un filtrado en función de la clave la manera de trabajar sería sencilla pero lo que tenemos que tener presente por eso quería que trabajáramos ejemplo es que para poder trabajar con Spring sobre un más tendríamos que obtener primero tú entrada vale todo más trabajas una estructura clave valor pero realmente lo que hace almacenar de forma interna una colección de de un tipo de dato que es la entrada del mar necesito un par clave balonmano del tipo más punto F bueno mediante este método podemos obtener un set y podemos trabajar con stream sobre césped podríamos por ejemplo si entrar solamente aquellos que cumplieran que tú que fuera mayor o igual que 5 es decir descartando estos cuatro recolectar este Mac en un nuevo más de esa manera vale recordar que te colector necesitaba de dos funciones el key Mapper y el barrio más pero para después poder imprimirlo en este caso por montones vale sería una operación sencilla que sí que nos permitiría filtrar el más completo y obtener una serie de entradas en particular siempre y cuando cumplan una condición sobre las clases trabajar sobre colecciones más sencillas pues francamente más sencillo las mismas personas incluidas solamente en una estructura de tipo list nos permitirían hacer operaciones de filtrado y Mateo de esta manera no por ejemplo si quisiéramos obtener el nombre de aquellas personas que son mayores de 18 años lo podríamos hacer con esta estructura como la que tenemos en pantalla no daríamos por la edad de la persona más fea haríamos para quedarnos con la mente con el nombre y los podríamos imprimir por consola si quisiéramos obtener a las personas que están en el activa la condición del filter podría ser está de aquí pero no dejaría de ser una estructura similar cómo puedes comprobar lo que hacemos es dar un predicado vale en este caso lo que vamos a hacer imprimir la edad de la persona en años vale mediante el método que queda lo tenemos por aquí implementado ya hemos usado alguno parecido si queréis conocer más sobre el API de fechas que incluye cada 8 os recomiendo que visitéis nuestro curso de Java 8 desde cero si todavía no lo habéis hecho este método lo que va a hacer es devolvernos en años lo devuelven un no por eso lo transformamos a un entero no creo que nadie viva lo suficiente para tener que almacenarlo en un don vamos posiblemente tampoco no me entero pero es un tipo de dato más habitual y calcularía la diferencia en año entre su fecha de nacimiento y diarios eso también por consola podríamos querer contar las personas que cumplen una determinada condición en decirlas que pasa necesito no por ejemplo las que han nacido a partir del siglo 21 incluso podemos combinar las operaciones de filtrado con colectores de agrupamiento como los que hemos visto anteriormente seleccionar aquellas personas que son mayores de edad pero agruparlas para que aparezcan por agrupadas por la cantidad de personas que tienen esa edad no cómo podemos ver a continuación un momento en el vídeo en el que hablamos sobre el interfaz por el predicado y podemos componer predicado para hacer condiciones más complejas podríamos definir un filtro para buscar a una persona en particular y componer predicado sobre persona para que bueno pues de alguna manera más complejo no aquí tenemos el predicado que sería algo más complejo vale bueno pues otra persona y podríamos intentar buscar algún elemento mediante el método fine fer vale que como sabemos devuelve un opcional por eso utilizamos este método de y está presente el valor lo imprimimos por consola y que no pues no se hace nada como le cojo comprobar el uso de los filtros estanque mente encendido y lo podemos complementar con todo lo que ya sabemos de lápiz en las personas mayores que el nombre de las personas mayores de 18 años las que están en edad activa imprimiendo el nombre y la edad el número de personas que han nacido en el siglo 21 las personas mayores de edad a agrupadas por edad o poder comprobar tenemos dos personas de 86 años o aquí como podríamos ir filtrando vale de manera que podríamos buscar si hay algunos que cumplan con determinada o no condición lo que está haciendo buscar a alguien que se llame Andrés y si no lo hay no te vuelve una persona vacía la clase persona al crearse mediante el constructor vacío le hacen a la fecha de nacimiento hoy por eso no aparece resultado están localizando a alguien que bueno que contenga la S y que la edades entre 60 y 90 vale aquí estamos teniendo en ambos casos sería aquí estamos teniendo alguno y aquí estamos teniendo el primero en ambos casos no está devolviendo a Jesús vale que nació en esta fecha de nacimiento cómo podéis comprobar la operación es decir tirado son bastante potentes pero sencillas de implementar habiendo terminado de verla vamos a pasar a hablar ahora de las referencias a métodos que ya venimos utilizándola para bueno pues ir acostumbrando pero que vamos a explicar la ESO


# 21 Referencias a métodos con stream 10:55 

[Referencias a métodos con stream](pdfs/21_Referencias_a_métodos.pdf)

## Resumen del Profesor

### 21.1 Referencias a métodos

Las referencias a métodos son una forma de hacer nuestro código aun más conciso:

```java
public class Persona {
   //…
   public static int compararPorEdad(Persona a, Persona b) {
      return a.fechaNacimiento
                  .compareTo(b.fechaNacimiento);
   }
}

//...

List<Persona> personas = //...

// De menos a más "conciso"
personas.sort((Persona p1, Persona p2) -> {
   return p1.getFechaNacimiento()
      .compareTo(p2.getFechaNacimiento());
});

personas.sort((p1, p2) -> p1.getFechaNacimiento()
                                .compareTo(p2.getFechaNacimiento()));

personas.sort(Persona::compararPorEdad);
```

### 21.2 Tipos de referencias a métodos

* `Clase::metodoEstatico`: referencia a un método estático.
* `objeto::metodoInstancia`: referencia a un método de instancia de un objeto concreto.
* `Tipo::nombreMetodo`: referencia a un método de instancia de un objeto arbitrario de un tipo en particular.
* `Clase::new`: referencia a un constructor.

## Transcripción

<img src="images/21-01.png">
<img src="images/21-02.png">
<img src="images/21-03.png">
<img src="images/21-04.png">

Hola vamos a continuar trabajando con el API string y lo vamos a hacer con la referencia si se trata de un mecanismo que ya hemos utilizado en alguno de los vídeos anteriores sobre todo a la hora de consumir los elementos en stream y vamos a explicarlo con profundidad para entender su funcionamiento la referencia métodos podríamos decir que son cómo rizar el rizo porque es una forma de hacer un código aún más supongamos que tenemos una clase persona sale modelo que incluye además de los atributos y métodos habitualmente un método que nos va a permitir comparar por edad 2 personas vale y lo haríamos fue de esta manera que ya de por sí es sencilla vale la fecha localdate implementa el interfaz comparable incluye el método compare to y podríamos compararlo devolviendo este número de teléfono la referencia método nos van a permitir hacer llamar a determinados métodos de una clase de una manera mucho más concreta este modo de comparar las personas por fecha de nacimiento bueno pues lo podríamos haber implementado mediante una expresión Irlanda por ejemplo para trabajar con el método sol que nos permitiría ordenar la colección de persona hemos visto diferentes maneras de hacerlo aquí tenemos tres códigos de menos con fiso abajo el primero bueno pues aunque utilicen una expresión lambda pues estamos proporcionando los tipos de datos aunque si bien nos damos cuenta en el segundo ejemplo de código los tipos de datos los puede obtener a partir del contexto no se los tenemos que dar decir tamente con lo cual podríamos hacer una llamada más contigo en cambio nos podríamos plantear y si este ordenamiento lo tengo que hacer yo en más de un sitio bueno pues podríamos utilizar el método comparar edad de la clase persona con una referencia a metros cómo podemos comprobar una referencia método para hacer el nombre de una clase utilizaríamos 12 en los dos puntos y el nombre de un método al que nunca le vamos a pasar explícitamente unos argumentos como podemos ver en la llamada embargo como podemos comprobar en el código anterior y método comparar por edad sí que espera que nosotros le proporcionaremos todo saca el argumento pues al igual que ha sido capaz de inferir el tipo por el concepto en este caso será capaz de inferir que tiene que proporcionar los dos argumentos que espera desde la colección Alex desde el stream de persona tenemos varios tipos de referencias y vamos a tratar de ejemplificar cada uno de ellos podemos invocar a un método estático de una clase vale con el primer ejemplo que tenemos nombre de la clase 2.2.22 mejor dicho y el nombre del método estático también podríamos invocar a un método de una instancia en particular a través de una referencia de ellas decir con el nombre del objeto:: método dental podríamos querer y veremos en qué contexto se utiliza ya lo hemos utilizado por ejemplo en la operación Maps hacer referencia a un método de una instancia pero a partir de su tipo es decir con el nombre de la clase los puntos de punto y el nombre del método haríamos una llamada a un método de instancia de un objeto arbitrario de algo en las funciones de Mateo eran un claro ejemplo de ello no podemos indicar a una referencia en particular con lo cual decimos que será este método de instancia de un tipo de una caja y también incluso podemos hacer referencia a un constructor vale cosa que podemos utilizar también en algunos cortes nombre de la clase los puntos los puntos míos nos permitiría construir un objeto en ocasiones era una propia abreviatura que podremos utilizar para construir objetos veamos algunos ejemplos de todo estos tipos que hemos comentado podríamos plantearnos aquí tenemos un ejemplo que trabaja con persona que tiene nombre y fecha de nacimiento hola vamos a continuar trabajando con el lápiz tren y lo vamos a hacer con la referencia qué tal se trata de mecanismo que ya hemos utilizado en alguno de los vídeos anteriores sobre todo a la hora de consumir lo elementos tener Trini vamos a quitar loco profundidad para encender su funcionamiento la referencia métodos podríamos decir que son como rizar el rizo porque es una forma de hacer un código además , le pongamos que tenemos una clase personal para quedarnos con aquellas personas que sean estudiantes


, las transparencias comparar por edad quedarnos con aquellas personas que sean estudiantes vale y también un comparador de persona vale que nos permitiría agrupar las comparaciones de personas por diferentes criterios no comparar con nombre operar verdad podríamos añadir incluso algo algo sistema de comparación humano y tendríamos una colección de persona podríamos menos por cierto uno que le proporcionamos explícitamente los tipos uno en el que los tipos de contexto no podemos comprobar este código no pero conciso es mucho más concreto vale haciendo referencia al método estático comparar fuera evidenciamos que un método estático porque está en cursiva de la clase persona vale estaría invocando a este método que estamos viendo aquí este será un método bastante mucho otro ejemplo de referencias a métodos por ejemplo sería el que tenemos aquí para un método de instancia de una persona particular el comparador de personas podríamos pensarlo en una especie de proveedor de de comparador que podríamos utilizar para usar uno u otro tipo de comparaciones entre entre objetos y bueno podemos ver como para la instancia concreta comparador de persona en minúscula estaremos llamando al método comparar por nombre


Para la instancia concreta comparador de persona en minúscula haremos llamando al método comparar por nombre vale que tenemos aquí que no haría la comparación en base al nombre de las cosas vale aquí tenemos por contra algún ejemplo de la llamada a un método de instancia partir de su tipo supongamos que queremos para un stream de de personas como el que hemos definido arriba obtener en una lista los nombres bueno pues en este caso tendríamos que utilizar este tipo de referencia a método no lo podíamos hacer a uno el particular porque bueno porque aquí vamos a iterar sobre el este del cual desconocemos a priori cuántos elementos tiene lo sabemos porque la lista para nosotros conocida pero bueno no sabemos en cada momento sobre qué elementos vamos a aparecer con lo cual no podemos hacer una referencia explícita al método perdón al objeto por lo que usaríamos este este tipo de referencia oye sé que voy a recibir una persona lo que hago es invocar al método que el nombre de la persona concreta que recibo en este momento aquí podríamos comparar perdón proporcionar un comprador para ordenar los nombres vale a partir de la propia clase stream streaming Clemente algunos comparadores vale como el compare to ignoré case que nos hace una comparación de dos cadenas por orden lexicografico pero sin tener en cuenta mayúscula o minúscula aquí tendríamos la manera de bueno de filtrar los estudiantes suponiendo que fueran personas que tengan 18 años o menos mapear los para obtener su nombre a partir del nombre construir una serie de objetos de tipo estudiantes Valencia y recogerlos todos en una lista para imprimirlos posteriormente aquí también tenemos como podemos comprobar como con la referencia me todo lo que estamos haciendo es invocar al método print LN del objeto out de la que hace chiste malo si queréis porque queda un poco más evidente la diferencia en este caso ya que la venía utilizando esto sería igual que hacer una expresión de este tipo no resulta francamente cómo imprimir todo esto ejemplo de una vez para que lo podamos ver como el primer método el segundo y el tercero que utilizábamos al principio de este código son igualmente operativos pero el tercero muchos más conciso vale como podríamos tener todas las personas ordenadas por nombre con este proveedor de comparación no resultado ser francamente cómodo los nombres ordenados también lo podríamos realizar de esta manera no mediante esta ordenación cómo podríamos quedarnos con los estudiantes imprimirlos por consola solamente aquellas personas que cumplan con el filtro haríamos la transformación para obtener el nombre del nombre transformarlo en estudiante como podemos comprobar aquí ya no devuelve un string de estudiante como los podemos recoger en una lista y como lo podríamos imprimir por consola con alguna de estas dos expresiones medio siempre preferiblemente con la referencia a metros con esto vamos a terminar el bloque de API extreme pero antes de hacerlo definitivamente vamos a hacer un gran ejemplo que nos permita implementar una aplicación en la que usemos exhaustivamente muchos de los conceptos que venimos trabajando desde el inicio del curso especialmente del ápice

# Práctica: Todos los elementos del API stream trabajando conjuntamente 22:25 

Ponemos en práctica todo los elementos del API stream trabajando conjuntamente

## Transcripción

No podíamos finalizar este bloque del API stream en un gran ejemplo que nos permita incluir todos los conceptos que hemos venido trabajando por la gran mayoría de ellos incluido los contenidos que hemos venido trabajando en muchos de los vídeos que hemos hecho desde el inicio de este curso de Java 8 para programadores vamos a tratar de hacer una aplicación que sirva como cliente de datos meteorológicos es decir una aplicación que va a recibir los datos de alguna manera en particular en este caso lo vamos a hacer con datos reales pero que se los vamos a proporcionar directamente a la aplicación sobre meteorología son los datos que la recogió en un día en particular el 31 de octubre de este año para todas las estaciones meteorológicas que tienen distribuidas por todas las provincias de España para cada estación meteorológica vamos a tener una serie de datos que hemos querido reflejar en esta clase registro datos dónde estarán la fecha el nombre de la estación meteorológica que notará también una pista de dónde está la provincia en la cual se encuentra las temperaturas máximas y mínimas con la hora en las cuales se registraron mediante esta clase temperatura ahora que tendrá un tipo de dato real cloud para la temperatura y un local time para almacenar la hora vale y también la precipitación acumulada a lo largo de ese día la clase registro dato no va a tener mucho más misterio Pepe hashCode equal to string habitual todo ello lo vamos a hacer implementando un servicio vale que era una de las clases que lo comentábamos en el primer bloque de este curso el servicio de climatología nos va a permitir realizar alguna operaciones sobre todo el conjunto de datos que vamos a tener como decía ese conjunto de datos lo tendremos de una manera que está entretenido aquí directamente como ya os digo son bastante teatro reales qué son reales para ese día y que bueno simular Ian que tuviéramos una lectura de estos datos desde un origen de fuera un API REST o en XML o en formato CSV o cualquier otro origen que nos lo pudiera proporcionar simulamos que estamos leyendo todos los datos de esas diferentes estaciones meteorológicas la clase app vale vamos a generar un informe que va a incluir entre otros datos la máxima y mínima totales absolutas de toda España para ese día las temperaturas máximas por provincia las temperaturas mínimas por provincias las temperaturas medias por provincia y los datos de una provincia en particular con la por las precipitaciones vale por desgracia vaya comprobar como las precipitaciones en este caso y por esta fecha está haciendo bastante baja no bueno todo ello lo vamos a implementar mediante métodos estáticos dentro de la clase app la referencia que vamos a crear del servicio también va a ser estática vale la del propio servicio de climatología incluso alguno de vosotros podríais plantear implementar este servicio como como un singleton ya avisaba que patrón Singleton lo lo utilizaremos más adelante por ejemplo con con todo el bloque de acceso a la celebración o en este caso no hemos hecho con una simple referencia estática para no emborronar mucho más lenta invocar al servicio y también alguna clase de utilidad bueno que nos va a permitir imprimir de manera ordenada un balón más en particular que obtendremos en más de en más de una ocasión dónde tenemos el meollo de la aplicaciones en el servicio que lo vamos a ver a continuación pero antes para que podamos ver cómo se van a presentar los datos esto como ya digo singular y amos la producción de un fichero por ejemplo PDF o HTML que queda fuera del alcance de este curso en este caso lo vamos a imprimir solamente por consola todos los datos de España como hemos dicho máxima y mínima total las máximas y mínimas por provincias las temperaturas medias por provincia y los datos de una provincia en particular para los cuales no es un método bueno que podríamos pasar de cualquier otra provincia de España vamos a fija para el día 31 de octubre tendríamos que la temperatura máxima y mínima de España se tuvieron la máxima en una estación de Las Palmas la mínima en una estación de Cantabria con estas temperaturas y estas horas vale todo ello lo hemos tenido filtrando los datos que teníamos gatos ahora centro de M como decía que son bastante rato ahora veremos como hemos hecho esto mediante lápiz las máximas por provincias podemos comprobar como y lo que tenemos son un montón de registros de datos para cada provincia lo que hemos tenido que hacer es un agrupamiento una recolección con agrupamiento de esos datos que nos han permitido obtener la temperatura máxima y la hora a la que se produjo las mínimas por provincias para hacer similares vale donde cuando lo que tenemos son las mínimas y la hora a las 7 se produjeron la temperatura media va a tratar de aglutinar además los datos de la máxima y la mínima en un solo dato que también vamos a tener que calcular los datos para una provincia lo que van a hacer es bueno pues filtrar no para obtener todos los registros de una sola provincia con los datos de la estación meteorológica la máxima la mínima y la precipitación cómo podéis comprobar también y aunque no es algo que sea materia de este curso y queréis algo más sobre el formateo de cadenas de caracteres del uso de string to StringBuilder podéis echar un ojo al curso de Java 8 desde cero pero hemos sido algo cuidadoso con el formateo de las cadenas de caracteres y también vamos a aprovechar para comer a ver el código cómo podemos comprobar en estos métodos estáticos lo que vamos haciendo este invocar al servicio la temperatura máxima y mínima total lo que va a hacer eh y recoger del servicio la máxima total la mínima total y si están presentes las vamos a imprimir cómo podéis comprobar estamos usando de forma podremos decir imperativa la clase opcional de manera que comprobamos y los valores que no retornan están presente o no antes de imprimirlo recordar que también aparte de ir presa teníamos expresan malo que no aceptaría un consumer dentro y lo podríamos haber hecho todo de un bloque lo único que bueno de esa manera perdemos el hecho de poder imprimir un mensaje el vale por lo tendríamos que hacer de otra manera distinta cómo calcula el servicio la máxima y la mínima total bueno pues solo podríamos ver dentro del servicio lo hace utilizando el API stream vimos en su momento que teníamos las operaciones Max y min y Max calcular el máximo de todos los datos registro y utilizando la comparación que nos proporciona y la clase float podríamos comprar la máxima de ambos registro temperatura máxima y bueno utilizando esta comparación ya podríamos a comparar todos los elementos del Extreme y obtener la máxima lo mismo sucedería con la mínima podríamos comprar la mínima de ambos registros de datos para obtener la temperatura misma cómo podéis comprobar algo que he hecho con un código clásico Java nos hubiera requerido quizá ordenar todo el todo el toda la colección para obtener el primero tener el último porque lo podemos hacer desde un golpe de código bastante sencillo en el caso de la máxima por provincia para requerir algo más de trabajo y como la máxima y la mínima se parecen bastante en ese sentido simplemente es luego la temperatura concreta que vamos a utilizar vamos a comprobar como hemos podido generalizar un poco el método que nos devuelve el tema con el nombre de la provincia la temperatura y hora y lo vamos a imprimir mediante este método estático vale que va imprimiendo para cada provincia sin temperatura imprime la provincia la temperatura y la aquí es donde decía que por ejemplo hemos sido algo cuidadosos con el formateo de las cadenas de caracteres el especificador de formato va a ser que cuando imprima con la provincia se alinea a la izquierda y ahí en negativo la provincia se alinea a la izquierda negativo y ocupe 25 caracteres la cadena de la provincia 25 caracteres o no lo ocupe por eso vale haciendo esta especie de padding vale pues todas las cadenas ocupa los 25 caracteres y ahí que de ahí que la barra aparezca en todos los todos los registros que hemos impreso en el mismo sitio no a partir de ahí en este caso sería imprimir y amos la temperatura que va a ocupar vale dándole espacio lo que hacemos es que bueno lo menos sea el INEM a izquierda de los números y bueno utilizando este especificador de formato así permitiremos tener dos decimales que todos los números ocupen 6 vale en total de forma que bueno que aparecen alineados de esta forma la cual nos permite que ocupen prácticamente siempre los mismos caracteres vale no requeriría demás especificador de formato que el método to String que lo devuelve como podemos comprobar bueno porque esta manera no si tiene la hora y minutos pues la hora minutos y incluye información sobre los segundos también incluiría los segundo hasta los nanosegundo hubiéramos proporcionado información decíamos que eso eran la manera de imprimir el método temperatura máxima por provincia y temperatura mínima en el servicio de climatología vamos a comprobar lo único que hace es invocar al metro temperatura más o 1000 por provincia vale lo mismo que hace el método de temperatura mínima que es un método privado y que acepta como argumento una temperatura tipo lo hemos implementado a partir de una enumeración sencilla incluye la máxima y la mínima habanera vale podemos comprobar aquí y la máxima o la mínima es el valor que le hemos proporcionado mediante el operador ternario y bueno nos permite devolver el colector que vamos a utilizar específicamente para en el método poder hacer un agrupamiento de los datos vale en base a la provincia y bueno indicándole aquí el segundo nivel de agrupamiento le podemos decir y lo queremos hacer por la máxima por la mínima para el usamos tipo de colector que no hemos presentado antes sí que es el colector mapping y que nos va a permitir hacer una transformación vale una reducción de los datos para obtener de todos los datos la máxima vale y máxima la vamos a obtener on Mac bike del estilo que ya hemos visto antes si fuera la mínima lo que haríamos sería hacerlo de esta manera no va a hacer el mapeo de la temperatura mínima para calcular la mínima de información todo esto hace que podamos obtener en definitiva un un mastín y una temperatura opcional recordar que la clase opcional era un contenedor que nos permite evitar errores de tipo NullPointerException vale y nos devolverá pues la temperatura y la hora máxima mínima provincia en función del colector que vayamos a utilizar cómo podemos probar cuando invocamos esto desde la aplicación tanto la máxima como la mínima se obtiene la misma estructura por eso podemos utilizar un mismo método para recorrerlo imprimirlo y cuando se imprime por consola obtenemos las temperaturas máximas de cada uno de los días provincia por ejemplo una total de la provincia de Las Palmas que fue la máxima registrada en toda España es 31° a las 15:30 vale o por ejemplo cuando vemos las temperaturas mínimas que aparece en Cantabria vale menos 4,60 a las 7:40 vale okay y los datos enfermería de una manera totalmente presentable ya por eso totalmente para el caso de las temperaturas medias hemos añadido otro método al servicio que nos va a devolver un Maps de String ido directamente veamos cómo lo hemos conseguido en este caso tenemos el uso de varios conectores vale que nos van a permitir en primera instancia transformar nuestro más vale de forma que la estructura que vamos tener va a ser un Mac de registro de datos vale y la temperatura media de manera que construimos una colección de este tipo dónde es la clave C registro de dato y el valor la temperatura media la hemos calculado de una manera sencilla no haciendo el promedio de la temperatura máxima en la temperatura me esa colección accedemos al entryset como hemos visto en alguno de los minion teriores y construimos de nuevo en Steam aprovechamos para volver a recolectar sopa Lore agrupados por bueno pues por la provincia calculando la media de los valores para cada registro de datos habíamos calculado la media lo que hemos hecho ahora es agrupar por la provincia vale para ellos como en la estructura de entristecer recordamos que era un más dentro y declaré valor lo que hacemos ser indica específicamente para que seamos conscientes que te lo vamos a utilizar y poder sacar de la clave la provincia y los agrupamos vale el valor que vamos a tener es el valor de la media para poder hacer la media de las medias diarias vale hemos añadido este tercer argumento aquí para que podamos comprobar como el tipo de Mac que se devuelve no tiene porqué ser karma y utilizando una referencia a método de las ya conocidas podemos construir el más que se va a devolver como un tribal recordemos que un tema almacenaba o guardar el orden no de estar de esos datos que nosotros vamos almacenar vale utilizando el orden por las claves de esta manera ya digo estamos haciendo una llamada al colector que nos va a permitir agrupar los datos por clave en un treemap vale obteniendo el valor como la media de los valores promedios de cada uno los registros de datos o disco bar ha sido una recolección para obtener otra colección y una vuelta a recolectar los valores pero que nos permite obtener los datos que nosotros necesitamos que son las provincias y la temperatura promedio vale de esa provincia lo hemos impreso aquí nos la temperatura media en cada una de las provincias que serían la media del promedio diario de los registros de datos que tenemos para cada una de las estaciones de esa provincia para obtener los datos de una provincia en particular fue hemos hecho un filtrado para que veamos cómo con un predicado sencillo que nos permite obtener vale aquellas provincias que son iguales que una cadena de caracteres que nosotros le proporcionamos todos los datos recolectados mediante un colector de lista permitir devolver no en un listado de registro de datos que nosotros podemos utilizar después para imprimir además podemos ordenar los comparando vale la interfaz comparato tiene algunos métodos estáticos en particular tiene un método compare que nos permite hacer una comparación con una anda o en este caso con una función que le pasamos como una persona anda en este caso en particular una referencia al método GET estación meteorológica que básicamente lo que haría sería ordenar los todos los datos todos los registros de datos por orden alfabético de las estaciones meteorológicas prepara el caso de la temperatura media en Moaña Vigo otro método al servicio que nos va a devolver un par de exprimido de sirena nos vemos como luego conseguido tenemos el uso de Derio colectores vale que no van a permitir en primera instancia transformar nuestro más vale de forma que la la estructura que vamos a tener para ser un mar de registro de datos vale y la temperatura medievales

Por orden alfabético de la estación meteorológica volveríamos a hacer una impresión cuidando el formato no la izquierda hiciéramos pasármelo la mínima de la cual ya hemos hablado así en lugar de Sevilla quisiéramos imprimir los datos de Jaén no le hemos puesto el tema de las tildes María vale bueno si te esperamos otra algo mal norte los datos de Madrid vale Cantabria vale cualquier precio planteado aunque ya digo tampoco era el meollo de esta aplicación haber planteado que este dato se pudiera recibir como como argumento de la invocación a a la clase para poder imprimir los datos de una provincia en particular bueno pues como podéis comprobar el uso de no elementos que hemos visto hasta ahora en particular del API stream hace de cava otro lenguaje de programación casi que totalmente nuevo sobre todo en lo que es operaciones de procesamiento filtrado reducción y transformación de datos lo convierte en un lenguaje mucho más atractivo y potente medio sobre todo en este tipo de operaciones con este vídeo nosotros terminamos el tratamiento del API stream nos vamos a lanzar de lleno a trabajar con flujos y ficheros en el siguiente bloque con la Java io y la sabani o dos


## Contenido adicional 7   

[Introducción al API Stream](pdfs/16_Introducción_al_API_Stream.pdf)

[Métodos de búsqueda de datos](pdfs/17_Métodos_de_búsqueda_de_datos.pdf)

[Métodos de datos, cálculo y ordenación](pdfs/18_Métodos_de_datos_cálculo_y_ordenación.pdf)

[Uso de Map y flapMap](pdfs/19_Uso_de_map_flatMap_y_Collector.pdf)

[Uso de la clase Collector](pdfs/19_Uso_de_map_flatMap_y_Collector.pdf)

[Uso de streams y filtros](pdfs/20_Uso_de_streams_y_filtros.pdf)

[Referencias a métodos con stream](pdfs/21_Referencias_a_métodos.pdf)

