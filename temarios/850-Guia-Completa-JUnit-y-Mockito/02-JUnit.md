# 02 JUnit 5

* Introducción Pruebas Unitarias 09:14
* Introducción JUnit 5 10:31
* Actualización creando proyecto Maven en IntelliJ 01:31
* Creando y configurando el proyecto con JUnit 5 08:48
* Escribiendo y ejecutando primeras Pruebas Unitarias con Assertions 17:19
* Escribiendo test para el **`saldo`** 11:44
* Test driven development TDD con JUnit 10:13
* TDD para **`debito`** y **`credito`** 11:12
* Probando y afirmando excepciones con **`assertThrows`** en JUnit 5 12:14
* Añadiendo la clase Banco y la realcion con las cuentas 09:55
* Probando y afirmando las realciones entre Banco y Cuenta 08:01
* Usando el método **`assertAll`** 07:10
* Agregando mensajes de falla en los métodos **`assertions`** 07:29
* Usando anotaciones **`@DisplayName`** y **`@Disabled`** 08:02
* Ciclo de vida anotaciones **`@AfterEach`** y **`@BeforeEach`** 11:12
* Ciclo de vida anotaciones **`@AfterAll`** y **`@BeforeAll`** 07:25
* Test condicionales **`@EnabledOnOs`**, **`@EnabledOnJre`**, **`@EnabledIfSystemProperty`** etc. 18:13
* Ejecuciones de test condicionales con **`@EnabledIfEnvironmentVariable`** 08:45
* Ejecución de test condicional con **`Assumptions`** programáticamente 10:17
* Clases de test anidadas usando **`@Nested`** 11:03
* Repitiendo pruebas con **`@RepeatedTest`** 08:36
* Escribiendo pruebas parametrizadas con **`@ParameterizedTest`** 10:50
* Pruebas parametrizadas con **`@ParameterizedTest`** parte 2 08:03
* Pruebas parametrizadas con **`@ParameterizedTest`** parte 3 11:15
* Tagging tests con anotación **`@Tag`** 06:47
* Inyección de Dependencia & componentes **`testInfo`** y **`TestReporter`** 12:38
* **`Timeout`** en JUnit 5 09:06
* Maven surefire plugin 09:50
* Descargar Código Fuente 00:03

## Introducción a Pruebas Unitarias 09:14

<img width="787" alt="image" src="https://user-images.githubusercontent.com/23094588/195566540-8835b0ab-dc25-48b6-adff-fa00870b22d4.png">

Cuando uno comienza a desarrollar una aplicación, hay tres paso importante que hay que seguir.

<img width="685" alt="image" src="https://user-images.githubusercontent.com/23094588/195579300-67ef4a50-9792-4b87-9a61-baa80a871c85.png">

Primero, ***el diseño***, antes ecribir cualquier línea de código tienes que tener algo, algún esquema, algún plano, igual que cuando se construye una casa necesitamos un plano de cómo hacer esta casa. La cantidad de piezas, dormitorio. En programación, en desarrollo de aplicación es lo mismo, tenemos que tener un diseño, un layout de que hacer. Por ejemplo, ***Diagramas de Casos de Uso*** para la toma requerimiento, ¿Cuáles van a ser los usos de nuestra aplicación? ¿Cuáles van a ser las funcionalidades? ¿Requerimientos? En fin.

También tenemos que resolver la lógica de negocios, los datos, cuáles van a ser nuestras clases, cómo se van a relacionar una con otra. En fin, tenemos que tener algo, algún diseño, algún esquema y resolver algún problema según regla de negocio.

Luego empezamos a escribir nuestro código, a implementar nuestra programación.

Como tercera etapa, tenemos que **probar nuestro código** y tenemos muchas formas de probarlo, por ejemplo, utilizando herramienta de depuración del Ide o simplemente escribiendo alguna validación en nuestro código, una clase con el metodo **`main`**, instanciar una clase y probarla. Vamos a tener, por ejemplo, un dato de entrada, un dato de salida, un parámetro esperado y compararlo con el dato que retorno por ejemplo un método y comparar las expectativas vs la realidad, si se cumple, utilizando validación de nuestro código con sentencia de control, con un **`if`** por ejemplo, imprimir el resultado en la consola del navegador. Todo esto sería una forma de escribir nuestras pruebas de nuestro código. Probar el código es una etapa muy importante.

También lo podemos probar en el navegador si estamos desarrollando una aplicación Web, si estamos trabajando con API Rest probarlo por ejemplo en **Postman**, que es una herramienta especial para testear una API Rest.

Tenemos muchas alternativas y está perfecto, son excelentes cuando estamos desarrollando nuestra aplicación, pero cuando intentamos depurar cualquier error lógico, cualquier problema en caso de fallo, quizás estas formas son un poco más limitadas y necesitamos algo un poco más robusto. Requieren un juicio humano, analizar el problema de forma un poco más manual y aquí esta la limitación.

Entonces necesitamos una herramienta que nos agiliza un poco más el proceso por ejemplo, automatización, un sistema de reportería donde falló,  como falló, ¿Cuáles son los métodos que fallaron?, que nos de la posibilidad de ejecutar varios casos de uso, escenarios posible y no solamente probar el ahora.

En el proceso de desarrollando estamos probamos, pero ¿Qué pasa el día mañana? ¿Quién no asegura que nuestra aplicación va a seguir funcionando correctamente? No sé, en seis meses más, en un año podrían surgir nuevas actualizaciones, nuevas funcionalidades, esos cambios, quizá afecten.

Entonces tenemos que tener un plan de continuidad, de realización, de test, de esa forma que nos asegure que siga funcionando hoy y mañana.

Pero bien, entremos un poco más en detalle sobre la necesidad de realizar nuestras Pruebas Unitarias.

<img width="754" alt="image" src="https://user-images.githubusercontent.com/23094588/195579520-1eee4304-42d7-48cb-b9d0-64a5269812aa.png">

Por ejemplo, tenemos parámetro de entrada **`Input`**, todo código, todo método, toda clase necesita datos de entrada.

Según estos datos se va a realizar algún tipo de algoritmo, un fragmento de código que resuelve algo y devuelve un resultado.

Tenemos la **Pieza de código** devuelve una salida, un **`Output`**.

Podríamos querer probar nuestra salida, el **`Output`** con un valor esperado, la espectativa.

Siempre en Pruebas Unitarias vamos a tener un dato que nosotros queremos obtener, que esperamos como resultado de un algoritmo, pero finalmente podría ser otro o podría ser un valor **`null`**, un valor mal calculado, por lo que tenemos que probar nuestro código.

En la imagen vemos el ejemplo de la clase **`Matematicas`** con un método **`sumar`** recibe los parámetros **`a`** y **`b`** y retorna su suma.

<img width="865" alt="image" src="https://user-images.githubusercontent.com/23094588/195585626-241f83d3-9db0-4906-9fff-b498dc71101d.png">

**`a`** y **`b`** son datos de prueba, datos input, de entrada si estos valores son **`2`** y **`3`** nosotros sabemos que el resultado es **`5`**, pero bueno, hay que probarlo.

<img width="848" alt="image" src="https://user-images.githubusercontent.com/23094588/195585724-683db204-631b-449b-92cd-acfaf13ce9a8.png">

Si la suma es distinto de 5, ojo, hay un problema, algo pasó porque nuestro valor esperado es 5 y no podría ser distinto. Entonces acá podríamos implementar nuestra prueba, pero no solamente para este caso. no solo para **`2`** y **`3`** podríamos tener, no sé, sumar valores negativos, ¿Qué pasa cuando tenemos 0?, cuando su número es extremadamente grande o muy pequeño, incluso con  decimales.

Son diferentes escenarios que podríamos testear, por ahora solo tenemos uno solo, se fijan entonces al final del día probar nuestro código no es tan simple.

De forma manual tendríamos que crear múltiples escenarios es decir, eso significa muchas clases con el método **`main`** y validar con un **`if`** si es distinto de **`null`**, si es distinto **`5`** y distinto a otro valor esperado y así por cada escenario, incluso si es igual, por si quiero compar si un valor es igual a otro.

Bueno, ahí es cuando **JUnit** como framework toma importancia, de que no solamente nos permite escribir nuestras pruebas, sino también nos permite escribir nuestras pruebas de forma mucho más ágil. En una sola clase, por ejemplo, podríamos tener múltiples y varios escenarios, incluso en una sola ejecución de una prueba unitaria. Podríamos ejecutar muchos test, muchos, no solamente uno, sino muchos escenarios, mucha clase de test y cada clase de test con múltiple método. Y todo eso organizado es decir, se ejecutan si falla, nos va a mostrar un sistema de reportería, un sistema de alertas. Qué método falló, en qué clase, esta información es muy valiosa, muy importante al desarrollador.

Si hacemos esto de forma manual podemos usar el **`System.out.println`** o el **`System.err.println`** para mostrar el error.

Podremos tener cientos de clase, cientos de líneas de código y todo eso se va a mostrar de forma poco amistosa en la consola. Difícil de dibujar, difícil de analizar, difícil de saber dónde ocurrió el problema.

Se necesita automatizar, parametrizar.

<img width="1059" alt="image" src="https://user-images.githubusercontent.com/23094588/195588839-c18f6981-b58f-466b-b0a6-23eb9a101dfc.png">

Como hemos explicado, necesitamos ejecuciones de Pruebas Unitarias Continuas en el tiempo, no solamente en el ahora.

Esto asegura que ante cualquier cambio de nuestra aplicación siga funcionando correctamente. Por ejemplo, si otra persona, incluso un compañero de nuestro equipo de trabajo, de la empresa, más adelante realiza algún cambio en el código, nuevas funcionalidad, ¿Qué pasa? Hay que probarlo.

Y si falla significa que es actualización o cambio en el código está rompiendo algo en la funcionalidad del código, que ya existía de nuestro código y este test que habíamos escrito, nuestra, Pruebas Unitarias lo va a detectar, si o si lo va a detectar, y de ahí la importancia es decir, no es algo que se pueda detectar en una prueba manual, implementada por nosotros en el desarrollo, por ejemplo, o realizando con la herramienta de debug. No, esto se detecta en el tiempo, ya están escritas, simplemente después de modifir el código, ejecutamos las pruebas.

Bueno, si falló, detectamos y corregimos, entre paréntesis. Bueno, en realidad no es tanto modificar, porque como buena práctica no se recomienda tanto modificar el código que ya existe, sino más bien extender. De hecho, hay un principio en la programación orientada objeto **Open Close - Abierto Cerrado**. Abierto a la extensión, heredar de clases, extender, pero cerrado a la modificación.

Claro, porque si modificamos siempre vamos a alterar de forma directa el código que existe, cuando modificamos algo. Porque ese código lo más probable que se está utilizando en algún lugar, al ser modificado de alguna forma podría alterar lo que ya existe. Podría romper otra cosa. Entonces siempre es bueno heredar, extender.

Pero así todo, también podrían haber problemas. Y esos problemas es lo que tenemos que también probar, si ocurren y tomar alguna acción, entonces muy importante, no solamente probar, ejecutar nuestros test cuando hayamos terminado de implementar o escribir nuestro código, no, sino también dar continuidad en la ejecución de test en el futuro. Y esto es lo fundamental para el éxito de nuestra aplicación.

<img width="1255" alt="image" src="https://user-images.githubusercontent.com/23094588/195606979-57f9591f-edde-44b5-9014-0cf92567b97f.png">

Vemos definiciones un poco más resumida ¿Qué son las pruebas unitarias?

Es un proceso de examen para verificar que una pieza de código cumple con cierta regla de negocios.

Y eso es fundamental porque vamos a ***afirmar*** un resultado esperado. Comparamos algo esperado VS la realidad, VS el valor que realmente devolvió un método. Si falla, bueno, hay que revisar ese método, ver qué está pasando. Quizá hicimos mal un cálculo o devolvimos **`null`**. Nos olvidamos de hacer el retorno correcto. El return, devolver la instancia o el valor.

<img width="1009" alt="image" src="https://user-images.githubusercontent.com/23094588/195607951-d63dfd19-4825-4b70-90b1-cf601edc1c15.png">

¿Qué es JUnit?

JUnit es un framework de Java, una librería que nos permite escribir y ejecutar Pruebas Unitarias.

Es importante, escribir y ejecutar, porque no solamente ejecutar, sino también escribirlas y de forma automatizada y también repetibles en el tiempo.

Por eso ejecutar repetibles pruebas unitarias no solamente hoy, sino también mañana.

## Introducción JUnit 5 10:31

En la sección anterior vimos Pruebas Unitarias, pero algo general. Ahora me voy a centrar en **JUnit 5** que es lo que vamos a utilizar en el curso.

¿Cuál es la arquitectura? ¿De qué se trata? Comparar un poco con la versión 4, la versión anterior.

La versión 4 ya tenía mucho tiempo en la industria, por lo menos unos 10 años, o quizá un poco más de años. En un software es demasiado tiempo, porque han pasado muchos cambios, de hecho todo lo nuevo de Java 8 en **JUnit 4** no existe, por ejemplo, las expresiones labda, la programación funcional, el API Stream, todo eso se viene a implementar en la versión 5.

**JUnit 4** más o menos fue lanzado desarrollado en 2006 por ahí, la versión 5 se lanza por ahí 2015/2016. Mucho tiempo sin actualizarse, sin nuevas característica, sin nuevos patrones de testing, sin nuevas características de Java, un montón de cosas que se incorporaron en la versión 5.

Y no solamente eso, sino también muchos tipos, formas de realizar Pruebas Unitarias o estilos que se agregan en la versión 5. Es decir, un cambio bastante rotundo.

<img width="899" alt="image" src="https://user-images.githubusercontent.com/23094588/195619556-d627d778-3cbe-481e-84ae-4cd097dcd725.png">

Pero comencemos con **JUnit 5**.

**JUnit 5** es una librería de Java, un framework que le permite escribir y ejecutar Pruebas Unitarias, repetibles, es decir, en el tiempo. Además de eso, **JUnit 5** trae un nuevo modelo de  programación mucho más robusto que la versión anterior. Programación funcional, expresiones lambda, es decir, todas las características de Java 8, incluye además varios estilos diferentes de pruebas que vamos a ver durante el curso. 

Por ejemplo, ***Pruebas Unitarias Repetibles***. Podemos indicar la cantidad de veces que queremos repetir una misma prueba o bien podemos repetir una prueba con distintos tipos de parámetros, ***Pruebas Parametrizables*** es decir, por ejemplo una lista, un arreglo de varios parámetros de entrada o inputs con diferentes valores y probar cómo se comporta con estos diferentes valores, es importante, automatizada y parametrizar, se van a ejecutar esta mismas prueba pero con diferentes escenarios.

Otro estilo por ejemplo ***Manejo de Excepciones***, es decir, bajo ciertos parámetros de entrada debería fallar nuestra aplicación. Bueno, se está manejando correctamente. Realmente se está lanzando una excepción que nosotros deseamos o esperamos, si así se está ejecutando bien, a pesar de que estamos probando o estamos simulando un error, pero lo estamos simulando de forma controlada, estamos provocando el error para que ocurra tal como queremos que ocurra bajo ese contexto, ahora si se lanza por ejemplo otra opción distinta a la que esperamos y falla la prueba, no pasa, o bien si no se lanza ninguna excepción, significa que no estamos controlando o manejando el error. Y ahí tenemos un problema que tenemos que solucionar. Es un estilo de prueba distinto.

También todo lo que es ejecución de pruebas condicionales, es decir, que se ejecuten no siempre sino bajo cierto contexto, bajo cierto ambiente, cierta versión, por ejemplo, de JDK, si la versión es la 11, por ejemplo, que ejecute una cierta prueba única especial para probar un código o una características del JDK 11 o superior. Pero si estamos utilizando la 1.8, la 8 no.

Pero también bajo ciertos ***parámetros de nuestro sistema***,  *variable de ambiente*, si tenemos configurada un área de ambiente o no, bueno, aquí ejecuta la prueba, sino que no la ejecute. O si estamos en *Linux* que la ejecute. Si tengo *Mac* o un *Windows* que no la ejecute.

Un montón de cosas que veremos durante el curso, uso extensivo de ***anotaciones***, manejo de ***ciclo de vida*** en las clase de test.

<img width="817" alt="image" src="https://user-images.githubusercontent.com/23094588/195624868-8aea8ba0-8160-415c-8663-2e9fd3b888eb.png">

Veámos la arquitectura de **JUnit 5**.

Una diferencia con **JUnit 4** la versión anterior es que era monolítica, es decir, había un solo gran componente que hacía todo y estaba todo dentro de ese mismo componente, por así decir **JUnit 4** y finalmente era un solo JAR.

Con **JUnit 5** tenemos una arquitectura un poco más robusta, ya tenemos diferentes componentes, tenemos un ***Core***, tenemos el ***API***, tenemos componente llamados ***Vintage***.

Comencemos con **JUnit Platform** la plataforma de **JUnit 5**, es el ***Core***, es la librería principal que está enfocada al contexto de ejecución del test. Es como el framework de ejecución que lanza nuestras Pruebas Unitarias.

Nuestras Pruebas Unitarias pueden estar escritas utilizando **JUnit Jupiter**, por ejemplo el ***API Júpiter***, con el cual nosotros interactuamos como programadores, desarrolladores. Siempre estamos escribiendo nuestros test con Júpiter o Yúpiter, como le queremos decir, es el API.

Por ejemplo, como desarrolladores nunca vamos a interactuar con la plataforma, la plataforma es solamente para ejecutar. Es el framework de ejecución.

Por ejemplo, los IDEs como Eclipse, NetBeans, IntelliJ, todos integran **JUnit 5** y por supuesto, la ejecución de Pruebas Unitarias a través de la plataforma. Por lo tanto, el IDE sí que interactúa con la plataforma. Tienen una integración, pero como desarrollaré no, nosotros solamente escribimos nuestra prueba a través de Júpiter.

Luego tenemos **JUnit Vintage** es un componente que nos permite integrar la versión 4 o versión 3 en un proyecto que conviva en conjunto con Pruebas Unitarias de la versión 5.

Es decir, podríamos tener un proyecto que ya tiene Pruebas Unitarias con la versión antigua, pero además queremos implementar Pruebas Unitarias con versión 5, las que ya existían las dejamos tal cual con la 4 y no tenemos que reescribir todo a la versión 5.

<img width="873" alt="image" src="https://user-images.githubusercontent.com/23094588/195631713-a1d826b9-bd2a-4d49-8a4d-e6105074937f.png">

Veamoslo un poco más detallado, tenemos **JUnit Platform** que es el Core donde esta todo el test engine o el motor de ejecución de test es donde se lanzan nuestras Pruebas Unitarias, pero nos permite lanzar Pruebas Unitarias con **JUnit Platform**, que es la versión 5, pero también nos permite ejecutar Pruebas Unitarias de versiones más antiguas, la 3 y la 4, pero además nos permite ejecutar, lanzar otros test de otro framework, de terceros.

Incluso si no nos gusta utilizar Júpiter, ni tampoco JUnit 4, podrían utilizar un externo de otro proveedor de un tercero o incluso desarrollarlo nosotros. JUnit trae integración para ejecutar otro framework de testing. Nos provee un API para poder desarrollar nuestro propio framework de JUnit Test y puede lanzarlo en la plataforma, ejecútalo.

Y ¿Qué pasa con los IDEs? Por ejemplo, IntelliJ, Eclipse, con NetBeans, todos traen integración con **JUnit Platform**.

Si desarrollamos una aplicación en IntelliJ, podemos ejecutar las Pruebas Unitarias porque trae integración. Claro, entonces podemos ejecutar nuestro test con los diferentes IDEs, que tenga soporte a **JUnit Platform** y no solamente con IDEs, sino también con herramientas de construcción, de despliegue. Cuando generamos el JAR por ejemplo, con Gradle, con Maven, con Ant también o incluso desde consola y al fin y al cabo cada uno de esos componente corresponde a un JAR.

No es una librería, un framework monolítico, no está todo dentro de una misma librería, no, cada uno de los componentes tiene su propio JAR.

Por ejemplo **JUnit Platform** es con la librería el JAR **`junitEngine`**.

Luego tenemos **JUnit Jupiter** que es el API. 

Luego tenemos **JUnit Vintage** para poder tener dentro de un mismo proyecto pruebas unitaria con versión 5, pero también con versiones antiguas.

<img width="757" alt="image" src="https://user-images.githubusercontent.com/23094588/195637821-262e21f8-ef9e-4067-92e9-2a4d2efab0fd.png">

Y para finalizar **JUnit Jupiter** es un API que nos permite escribir nuestros test. Nosotros como desarrolladores interactuamos de forma directa con esta API, agrega un nuevo modelo de programación nuevas características, todo lo de Java 8, todo lo de expresiones Lambda, todo lo de programación funcional, todo esto se incluye en esta nueva versión.

Además, incorporan y se reescriben las anotaciones que ya venían de la versión 4. Se reescriben, se le cambian el nombre a algunas y también se agregan muchas anotaciones nuevas y estilo de testing de prueba. Y aparte también Júpiter trae un nuevo modelo de extensiones que nos permiten agregar o extender Júpiter, por ejemplo, para trabajar con Spring Framework no se refiere a implementar un nuevo framework, no, sino utilizando Júpiter lo extendemos para agregar mayor funcionalidad, mayor característica.

<img width="647" alt="image" src="https://user-images.githubusercontent.com/23094588/195639590-06503160-3e2c-4b2f-b8f7-2bac211ad6e8.png">

Las anotaciones que se incluyen, **`@Test`** ya existía en versión 4.

**`@DisplayName`** tal como dice el nombre despliega o muestra el nombre del test, no existía en la versión anterior, entonces podemos incluir un nombre mucho más descriptivo del test.

También se incluye **`@Nested`**  para dar una jerarquía de nuestra prubas unitaria, es decir, tener una clase de prueba y dentro de la clase, otra clase y así, y dar un orden, una jerarquía.

Etiquetar o categorizar con **`@Tag`** tampoco existía.

**`@ExtendWith`** son nuevas características de este nuevo modelo que propone Jupiter.

**`@BeforeEach`** ejecutar antes de cada metodo de test.

**`@AfterEach`** ejecutar después de cada método de test.

Estas dos anotaciones existían pero se llamaban distinto, sin el **`Each`**, con el **`Each`** es más descriptivo, Antes de cada y Después de cada método.

Después tenemos **`@BeforeAll`** que antes se llamaba **`@BeforeClass`**, que se ejecute antes de que se ejecute cualquier método, pero se ejecuta una sola vez. No es por cada método, sino una sola vez.

Todos estos métodos son del ciclo de vida de una clase de test.

Lo mismo con **`@AfterAll`** al final, después de que se hayan ejecutado todos los métodos de prueba, se ejecuta este método, por ejemplo, para finalizar algo, para cerrar un recurso.

Finalmente tenemos **`@Disable`** para deshabilitar un método, una prueba unitaria, también existía, pero antes se llamaba en la versión 4, se llamaba **`Ignor`** de ignorar, pero el resultado es el mismo.

## Actualización creando proyecto Maven en IntelliJ 01:31

Esta es una lección de actualización sobre IntellliJ de como crear un proyecto, pero con Maven, porque durante el curso vamos a crear algunos proyectos en Maven y es importante que veamos esta actualización porque cambió un poco.

Necesito que solamente pongan atención, no es necesario que hagan nada, esto solamente para después, cuando creamos los proyectos en Maven, para que lo tengan en cuenta de como cambio un poco en IntelliJ.

<img width="804" alt="image" src="https://user-images.githubusercontent.com/23094588/195646515-3714a48d-4395-4344-b3c2-c65ac3057189.png">

Tengo la última versión de IntelliJ IDEA 2022.2.3 hasta la fecha, nos vamos a ir **New Project**.

<img width="802" alt="image" src="https://user-images.githubusercontent.com/23094588/195647071-d2dde09d-dc43-4dc4-b002-3ae22e298f42.png">

Aquí es donde cambia, se fijan, aparece una ventana un poco distinta a la del curso. A la izquierda dice ***Maven Archetype***, pero no, no tenemos que seleccionar ***Maven Archetype***, sino nos vamos a **New Project** y en **Name** le damos el nombre tal cual, según corresponda al curso, el **Location** indicamos la carpeta del curso en cuestión. Seleccionamos ***Java*** que el lenguaje que vamos a utiliza y en **Build system** seleccionamos **Maven**, en **JDK** tenemos la versión del JDK.

En **Advanced Settings** configuramos el **GroupId** y el **ArtifactId** del proyecto maven que configuraremos mas adelante.

Una vez llenado todos los datos presionamos el botón **Create**.

<img width="801" alt="image" src="https://user-images.githubusercontent.com/23094588/195651645-792aad3f-caf8-4d75-a96b-268a4cd03a97.png">

Si no queremos tener un proyecto Maven dejamos donde dice IntelliJ, pero si es Maven nunca seleccionamos en ***Maven Archetype***, sino como lo acabamos de explicar.

Esto solo es una actualización por que en los videos se ve una versión anterior de IntelliJ.

## 💻 Creando y configurando el proyecto con JUnit 5 08:48

Vamos a crear una carpeta donde vamos a almacenar los proyectos del curso.

<img width="804" alt="image" src="https://user-images.githubusercontent.com/23094588/195704259-773dd104-ac31-4507-a62d-2ac90d0ca4a7.png">

Ahora en IntelliJ vamos a pulsar en **New Proyect** y vamos a rellenar la siguiente información.

<img width="870" alt="image" src="https://user-images.githubusercontent.com/23094588/195705221-1d16f7dd-6d22-4ac1-b064-00d1acb92cd9.png">

Vamos a pulsar en el botón **Create**.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195705558-14cde848-303e-4b46-8452-329d551c618e.png">

Se crea nuestro proyecto, en el cual se genera el archivo **`pom.xml`**, que es un archivo donde se configuran las dependencias de Maven, las librerías que vamos a incluir en el proyecto.

Recuerden que JUnit 5 no es parte de Java, no forma parte del JDK, sino que es un framework externo que lo tenemos que incluir, como librería con una dependencia en nuestro proyecto. Justamente **`pom.xml`** nos permite agregar dependencias.

De hecho, después, cuando trabajamos con Spring Framework, vamos a trabajar con un proyecto, que tenga su **`pom.xml`** y allí vamos a tener nuestra dependencia.

Se automatiza mucho más las librerías, las versiones, además, permite construir el proyecto, generar el proyecto, el JAR, por ejemplo, para producción.

Nuestro siguiente paso es agregar dependencias, librerías en el **`pom.xml`**.

Podemos ayudarnos de las herramientas del IDE para generar las dependencia, vamos a dar un click derecho en la línea 16 después de **`</properties>`** y pulsamos en **Generate...**.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195708555-2513b223-5ffc-40fe-846a-89d018d483c4.png">

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195708707-ee80c96d-ce93-4578-bedc-7fd0483775fc.png">

Vamos a pinchar en **Dependency Template**

<img width="249" alt="image" src="https://user-images.githubusercontent.com/23094588/195709879-8ea6a5fe-f12d-418a-90ef-4096443368fb.png">

Y nos inserta la sección **`<dependencies>`**.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195710361-9adc3ec7-d1b0-45e5-8ed8-51bc1108aaf4.png">

```xml
<dependencies>
   <dependency>
      <groupId></groupId>
      <artifactId></artifactId>
      <version></version>
    </dependency>
</dependencies>
```

Nesesitamos insertar la dependencia para **junit-jupiter**, pero necesitamos datos más concretos de la dependencia el **`groupId`**, **`artifactId`** y **`artifactId`**.

Existe la página [Maven Repository](https://mvnrepository.com/) la cual es un repositorio de dependencias donde podemos buscar las dependencias que nos interesen.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195712318-ff73ebd7-eac4-4085-ba88-a7f362e17f49.png">

Tiene una barra de busqueda donde vamos a insertar **junit-jupiter**.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195712706-a5065544-8b91-43c8-9390-11de0de45f09.png">

Como podemos observar existen varios tipos de dependencias con el nombre **junit-jupiter** como **JUnit Jupiter API**, **JUnit Jupiter Engine**, **JUnit Jupiter Params**, pero nos interesa la de **JUnit Jupiter** a secas (la cual no veo) ya que es la que incluye todo.

Vamos a utilizar la que se esta usando en el curso, cuando empezamos a escribir en el IDE este nos ayuda suguiriendo información:

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195715107-6d2ae653-5931-45d3-bbfc-c18c3f7fe57d.png">

Aun que la versión más reciente que existe es la 5.9.0 vamos a usar la 5.6.3 que es la que se usa en los ejemplos del curso.

<img width="1114" alt="image" src="https://user-images.githubusercontent.com/23094588/195716499-0dc50943-0bf5-488f-bf6c-6f108f2432ea.png">

Por ahora la dependencia nos esta marcando un error vamos a dejarlo así por ahora, ya veremos.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195717175-0960b051-5a81-4971-b132-c6af22dc061d.png">

¿El siguiente paso?

Bueno, tenemos que recargar o actualizar la dependencia en el proyecto para eso aparece un **Load Maven Changes** (pequeño botón de refresh) es decir, cargar los cambios de Maven, hacemos un clic y eso lo que hace es descargar la librería, la dependencia.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195717864-cb449c38-ad25-4bce-ad36-1bc58f7c0a4b.png">

Al presionar el botón se han descargado las dependencias y han desaparecido los errores la dependecia.

Teniamos un fallo en el **`artifactId`** el correcto es el siguiente:

<img width="1022" alt="image" src="https://user-images.githubusercontent.com/23094588/195718749-a4a7081a-3593-4034-98e5-a8a8105757fa.png">

Pulsamos nuevamente el botón **Load Maven Changes**. 

Si revisamos en **External Libraries** vemos que las librerias se han descargado.

<img width="442" alt="image" src="https://user-images.githubusercontent.com/23094588/195718498-5fead6d4-35c0-44d4-89a5-018c8746dd6e.png">

Las librerias que necesitamos para trabajar con Pruebas Unitarias, estamos listo. Por ahora podríamos cerrar el **`pom.xml`**.

<img width="1490" alt="image" src="https://user-images.githubusercontent.com/23094588/195720131-dc3becb4-6908-4f1f-8df3-9b7b5a8258ea.png">

En nuestro proyecto tenemos la carpeta **`src`** la cual tiene la carpeta **`java`** donde tenemos nuestras clases Java del proyecto y otra carpeta **`test`**, que es el contexto de Pruebas Unitarias, cuando queramos crear una clase test para realizar nuestras pruebas lo hacemos aquí
En nuestro proyecto tenemos la carpeta **`src`** la cual tiene la carpeta **`java`** donde tenemos nuestras clases Java del proyecto y otra carpeta **`test`**, que es el contexto de Pruebas Unitarias.

En la sección de clases del proyecto vemos que se nos creo un paquete **`org.javaocio.junitapp.ejemplo`** y dentro de este se creo la clase **`Main`**.

<img width="750" alt="image" src="https://user-images.githubusercontent.com/23094588/195721787-8781be6a-8167-4ad9-a632-e8d58332158e.png">

Esto paso por que a la hora de crear el proyecto teniamos un check marcado para que lo hiciera si esto no lo ubiera creado automáticamente lo creamos nosotros a mano.

Vamos a crear una clase POJO y esto lo haremos en un nuevo paquete llamado **`org.javaocio.junitapp.ejemplo.models`** y dentro la clase **`Cuenta`**.

<img width="1493" alt="image" src="https://user-images.githubusercontent.com/23094588/195722263-9c64e681-e832-475a-82ea-6744cd7020ad.png">

A esta clase le vamos a insertar sus atributos y sus métodos getters y setters.

<img width="463" alt="image" src="https://user-images.githubusercontent.com/23094588/195722936-c2e975fb-773f-42ab-b839-9658a06ba90b.png">

Esta clase si se fija en bastante simple, pero recuerden, después vamos a ir ampliandola y vamos a colocar algunos método un poco más sofisticados, por ejemplo, el método **`debito`** y **`credito`** para la transferencia.

El ***débito*** es cuando nosotros o la cuenta realiza una transferencia, por lo tanto tenemos que *restar del saldo* lo que vamos a transferir y el ***crédito*** al revés cuando nos transfieren a nosotros y tenemos que *sumar ese monto a nuestro saldo*, después vamos a tener la clase **`Banco`**, la que realiza la transferencia y tiene una asociación, una lista de cuentas bancarias y la cuenta, por supuesto, va a tener un atributo que apunta hacia el banco, pero la idea de probar de a poco.

### BigDecimal

Hemos declarado al **`saldo`** como **`BigDecimal`**, podríamos haber usado un **`integer`** manejar al saldo como un entero, pero en realidad el saldo podría ser decimal, de hecho, podría ser un **`float`** o un **`double`** con punto flotante.

Pero cuando trabajamos con Monedas, todo lo que es finanzas, con dinero es mucho mejor y mucho más preciso trabajar con la clase **`BigDecimal`** que pertenece al paqute **`java.math.BigDecimal`**, un **`BigDecimal`**, es un decimal de alta precisión y además inmutable. Es un punto importante, que está diseñado para trabajar con números decimales.

La diferencia cuando tratamos con **`float`** o con **`double`** todos los cómputos o cálculos matemáticos en realidad se realizan a nivel binario, con ceros y uno. Y eso al final afecta la precisión.

No es tan preciso cuando queremos trabajar con dinero, como trabaja la industria financiera, por ejemplo, es mucho más eficiente y mejor, mucho más preciso trabajar con **`BigInteger`** o **`BigDecimal`**. Además que todos los computos y todos los cálculos se realizan de forma aritmética, pero con el cálculo humano y no con el cálculo de computadora binario, es una diferencia importante.

### Imagenes de los Videos.

![image](https://user-images.githubusercontent.com/23094588/130349155-dda27d56-36e2-4fba-a3af-a0f39182adf2.png)

![image](https://user-images.githubusercontent.com/23094588/130349165-aba4c3f8-7671-4b6a-9bdc-db6f6d913687.png)

![image](https://user-images.githubusercontent.com/23094588/130349323-21141834-49d9-4f7c-8425-0f0eb98a3108.png)

![image](https://user-images.githubusercontent.com/23094588/130349395-a24bf3bb-2db4-4de9-a889-aae0258b93b7.png)

Con ALT + Insert podemos añadir dependencias o visitar https://mvnrepository.com/

![image](https://user-images.githubusercontent.com/23094588/130349558-14dd16a1-1304-4ae3-93e0-5d239333faaa.png)

![image](https://user-images.githubusercontent.com/23094588/130349591-969f5a05-1bf5-49a8-9cd1-2f522841ad1c.png)

![image](https://user-images.githubusercontent.com/23094588/130349764-4519465e-b8e7-4628-9538-b6cc7be00cea.png)

![image](https://user-images.githubusercontent.com/23094588/130349888-232e1cfd-a6bc-4ab4-bde1-060c3a9c208f.png)

Con IJ hay que presionar un botón para descargar las dependenias añadidas de Maven.

![image](https://user-images.githubusercontent.com/23094588/130349977-fc32802e-db9b-41a9-b263-088d77f9bab3.png)

![image](https://user-images.githubusercontent.com/23094588/130349998-904f1176-abee-4884-a3d2-8c23ac946838.png)

Dentro de **`src/main/java`** vamos a crear el paquete **`org.javaocio.junit5app.model`** y dentro la clase **`Cuenta`**

![image](https://user-images.githubusercontent.com/23094588/130350227-7fdeb62f-f767-4d83-9bfa-1d8eda87aae6.png)

Vamos a meter algunos atributos y sus métodos getters y setters:

![image](https://user-images.githubusercontent.com/23094588/130350318-96ad7db4-90c3-4509-8535-ee1cb2e424d4.png)

![image](https://user-images.githubusercontent.com/23094588/130350400-ab8ec228-9272-4c97-bbcb-70ddae9508b7.png)

![image](https://user-images.githubusercontent.com/23094588/130350408-357ee4a0-cf67-4ec1-929e-5b5e94ad765e.png)

![image](https://user-images.githubusercontent.com/23094588/130350444-76c232ff-fb24-4a48-bff2-11d82ee7138e.png)

![image](https://user-images.githubusercontent.com/23094588/130350457-b4368475-7bc6-42eb-871f-f882f72e3b35.png)

## Escribiendo y ejecutando primeras Pruebas Unitarias con Assertions 17:19

En esta lección vamos a probar la clase **`Cuenta`**.

<img width="421" alt="image" src="https://user-images.githubusercontent.com/23094588/195782760-eac3c455-1973-4c4b-b140-1b5aa80a9d68.png">

La clase **`Cuenta`**, una clase bastante simple con algunos atributos y metodos. La idea es probar lo simple que tenemos, no solamente los métodos **`get`**, también podría implementar un **`constructor`** o como se están pasando los parámetros.

Y así vamos complementando y probando estos métodos en nuestra clase **`Cuenta`**.

Comencemos con ***verificar***, ***afirmar*** que esté funcionando bien.

En **`src/main/java`** vamos a tener el código Java y en **`src/test/java`** tendremos las clases de Test, podemos crear manualmente el paquete y la clase de Test o podemos usar las ventajas del IDE para crear la clase de Test, nos colocamos en el nombre de la Clase y pulsamos botón derecho y GENERATE en MAC (Command + N) otra forma es con ALT + ENTER

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195785400-e059392e-7a77-4a83-aa70-498768f2945d.png">

Y seleccionar **`Test...`**

<img width="535" alt="image" src="https://user-images.githubusercontent.com/23094588/195785926-657e3d52-82d7-42f1-9ad2-79e2ce9c69b0.png">

Aceptamos sin agregar nada.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195786425-45924662-05ea-4d4e-91c2-c1fb4866ae0a.png">

Por convensión la clase de Test lleva el mismo nombre de la clase original mas la palabra test en este caso el nombre es **`cuentaTest`**.

Vamos a añadir un método **`test()`** y lo vamos a anotar con **`@Test`** del paquete **`org.junit.jupiter.api`** la cual indica que este método se debe ejecutar.

![image](https://user-images.githubusercontent.com/23094588/130351204-3fad0ea7-1347-4db6-9d21-54a85518cda4.png)

La clase creada tiene un modificador **`defaul`** (no hay modificador) como buena practica debe ser de este tipo también conocida como ***Clase de Paquete*** (No **`public`**) lo que significa que solo se puede ejecutar dentro de los paquetes de Test. 

Otra forma de insertar un método de Test es usar en Mac COMMAND + N y en Windows ALT + Insert 

![image](https://user-images.githubusercontent.com/23094588/130351307-e29315e1-eb24-4f82-ac96-54fc6a0d69bf.png)

y seleccionar **`Test method`**

![image](https://user-images.githubusercontent.com/23094588/130351327-7154557e-afe2-4e2a-b342-7d8b263e8443.png)

Ya solo le ponemos el nombre que queramos. En algunos casos los nombres se ponen así **`test_nombre_cuenta`** para que en los reportes sea más facíl saber que método se ejecuto aun que hay otra opción mejor que es usar la anotación **`@DisplayName`** en la cual en lugar de usar el nombre del método podemos poner un texto más descriptivo, nosotros le pondremos por ahora **`testNombreCuenta`**, vamos a meter el siguiente código.

![image](https://user-images.githubusercontent.com/23094588/130351587-8814d66a-2fcf-4ea4-9077-475f24c9ff73.png)

Mientras que no afirmemos nada el Test va a ser correcto, es decir si no se utilizan las Assertions el Test va a ser correcto.

Vamos a ejecutar hay varias formas con la flecha verde del método ejecutará solo ese método, con las dos flechas en el nombre de la clase ejecutará todos los métodos de la clase o con la combinación de teclas CTRL + SHIFT + R o CTRL + SHIFT + F10.
 
<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195791990-ab8745ed-d753-41de-af49-afa6c6c3d1a3.png">

Esta todo correcto, no hay ninguna noticia de que ha pasado algo, pero en realidad no estamos probando nada. Tenemos que usar el API, los métodos estáticos de la clase **`Assertions`**, por eso cuando se creo la clase se importo de manera estática la clase **`Assertions`**, **`import static org.junit.jupiter.api.Assertions.*;`** y simplemente podemos acceder a los métodos de **`Assertions`** de una forma directa como si fueran parte de nuestra clase **`Cuenta`**

#### `assertEquals`

Se ejecuto la clase sin ningún error, hemos ejecutado la clase Test pero realmente no estamos probando nada, debemos usar el API y los métodos estáticos de la clase **`Assertions`** (comentamos la clase importada para usarla como clase estática).

**`assertEquals`** recibe dos parámetros el **`expected`** y **`actual`**.

![image](https://user-images.githubusercontent.com/23094588/130352791-a7b908af-4ca5-44a0-96bf-aa2eb0ff1189.png)

Si ejecutamos nuevamente la Clase Test tenemos:

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195794530-88c32da2-4485-44b6-b2aa-dcc61b78143e.png">

La ejecución ha ido bien, vamos a cambiar el código comentando el **`set`** y ejecutamos nuevamente:

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195796098-00ca9b52-5f78-4adf-88bf-7212c9194b47.png">

Aquí la cosa ya ha ido mal ya que el valor esperado (**`Expected :Adolfo`**) no es igual al valor real (**`Actual :null`**) es decir que comparamos **`Adolfo`** con **`null`** y como no son iguales no pasan la prueba de Test.

Vamos a regresar a la clase **`Cuenta`** con CTRL + TAB y le vamos a crear un constructor con COMMAND + N o ALT + Insert.

![image](https://user-images.githubusercontent.com/23094588/130353058-a118c011-53c8-44ea-9918-f6186c73415c.png)
 
Pero solo con el atributo **`saldo`**

![image](https://user-images.githubusercontent.com/23094588/130353070-a61f26fe-970a-40fa-b08f-ec709caafd00.png)

<img width="1022" alt="image" src="https://user-images.githubusercontent.com/23094588/195797240-9e508ea5-15f3-4b89-a2bd-e2ccfcf8fd66.png">

![image](https://user-images.githubusercontent.com/23094588/130353070-a61f26fe-970a-40fa-b08f-ec709caafd00.png)

Si ahora manualmente metemos en los parámetros a **`persona`** tenemos:

![image](https://user-images.githubusercontent.com/23094588/130353160-776417e4-9634-4399-940b-7e566d61e288.png)

Hemos olvidado asignar el parámetro **`persona`** al atributo **`persona`**(intencionalmente). El error que nos marca es por que en la clase Test estamos usando un constructor sin argumentos. Vamos a regresar a la clase **`CuentaTest`** y pasamos los valores en el Constructor.

![image](https://user-images.githubusercontent.com/23094588/130353444-429100f2-2f79-42b9-9632-377ef31554cd.png)

Observemos que el número que estamos poniendo dentro de **`BigDecimal`** lo estamos poniendo entre comillas, lo podemos poner sin las comillas, es una cosa de presición, al no poner las comillas lo que estamos pasando es un Punto Flotante un **`Double`** el cual tiene sus limitaciones en tamaño, en capacidad y también en presión, por esta razón es mejor el número como un **`String`** como una cadena.

Vamos a ejecutar el Test.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195799719-e8700f23-3676-4afd-be0c-6e2685ff5d34.png">

Seguimos teniendo el mismo problema, pero en este caso es pq el código del Constructor de nuestr clase **`Cuenta`** no esta del todo bien, vamos a modificarlo.

![image](https://user-images.githubusercontent.com/23094588/130353525-be7c7554-f51c-4b2e-8d86-df9542c59cad.png)

Y volvemos a ejecutar el Test.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195800219-9ee01b1e-7507-4927-a3b6-f1c1a54e1e58.png">

Ahora el Test a pasado la prueba.

Ahora estamos probado el método **`set`** pero además estamos probando el **`constructor`**. Tuvimos un fallo y lo corregimos 

Vamos a dejar de usar la clase estática **`Assertions`** y vamos a importala como estática para solo usar **`assertEquals`**.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195801291-5ae9b765-531d-48c5-a0b7-1fa8fb494ef9.png">

#### `assertTrue`

**`assertTrue`** recibe solo un parámetro que será una expresión booleana. Esto nos va a servir para evaluar una expresión booleana. 

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195804012-a027d396-2ae6-4d75-a31e-3aef2e66ac46.png">

Todo ha ido bien, vamos a cambiar el valor para que falle.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195804282-f2bf2157-5d51-4175-83b9-acb797fb7155.png">

Cuando tenemos varias **Assertions** todas ellas deben cumplirse para que pase el Test, en este caso el **`assertTrue`** falla ya que se esperaba verdadero(**`Expected :true`**) pero obtuvimos falso(**`Actual   :false`**).

Vamos a hacer una última modificación en la clase **`Cuenta`** para que regrese la **`persona`** siempre en mayúsculas.

![image](https://user-images.githubusercontent.com/23094588/130353975-b875303e-9503-4e18-95dc-aa3a80914456.png)

Y en Test ponemos lo siguiente y lo ejecutamos:

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195804791-f6d37ee7-e99c-4ce3-9b4c-b24c381ee536.png">

Aquí tenemos mal tanto el **`assertEquals`** y **`assertTrue`** pero solo nos esta marcando el primer fallo el de el **`assertEquals`** vamos a dejar el constructor como lo teniamos.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195805498-6839e672-f190-45e7-92f3-44607b9dc850.png">

Ahora el error nos lo manda en el **`assertTrue`**, vamos a modificar el código para que pase la prueba

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195807369-4c6ab726-9eea-4222-8dd6-12889f9e97fc.png">

Pero realmente estamos modificando el valor que se espera en el Test para que lo pase, normalmente lo que vamos a corregir es el código para que retorne lo que se espera, en este caso vamos a quitar el **`.toUpperCase()`** en la clase **`Cuenta`** y también vamos a modificar lo cambiado en el Test.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195807794-cf8367e9-7c3f-4e51-b501-a6cdde3fdb22.png">

## 💻 Escribiendo Test para el `saldo` 11:44

Ahora nos vamos a enfocar al **`saldo`**, para lo cual vamos a crear un nuevo método llamado **`testSaldoCuenta()`**

![image](https://user-images.githubusercontent.com/23094588/130355165-5ea6b3c5-afb5-4576-8b68-717767ae9baa.png)

Para que IntelliJ nos cree la variable.

![image](https://user-images.githubusercontent.com/23094588/130355208-74bcdc59-a45a-4d3d-ba44-f9a17ee03d86.png)

El método nos queda así:

![image](https://user-images.githubusercontent.com/23094588/130364731-2307c804-33b6-4db6-891a-b5ca209562e2.png)

Con **`assertEquals`** estamos comparado un valor esperado y un valor actual, el valor esperado lo estamos poniendo como una literar **`double`**, el valor actual sabemos que es **`Bigdecimal`** y con **`.doubleValue()`** lo que hacemos es que lo trate como **`double`** para evitar compatibilidad de tipos, vamos a ejecutar la Clase:

Vamos a ejecutar solo el método **`testSaldoCuenta()`** damos un click dentro del método y pulsamos CONTROL + SHIFT + R:

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195821217-ab067dda-4cf0-4985-ad78-2056fb23e7e0.png">

La prueba la ha pasado para el método **`testSaldoCuenta()`**, ahora lo que vamos a hacer es ejecutar toda la clase para lo cual hacemos un click en la clase y damos CONTROL + SHIFT + R o el botón ejecutar:

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195822054-a5a374a9-f95c-49a0-93a4-2f0889bd61d6.png">

Los dos métodos pasan el Test. Observese el orden en que ejecuta los métodos no ha sido en el orden en que estan colocados esto lo determina el Motor de JUnit5 pero realmente para nuestras pruebas esto no debería tener ninguna importancia ya que nosotros estamos probando piezas de código independientes. ***No debería manejar estados*** en los diferentes métodos, no deben estar relacionados. En este caso se crea una instancia diferente de **`CuentaTest`** para cada método que tengamos.

Después vamos a ver cómo ordenar los métodos por ejemplo, si queremos organizar y que se vean visualmente ordenados, ya sea utilizando el nombre método y que ordene de forma alfabética o utilizando la anotación **`@Order`**, alfabética o con un número, le podemos dar un orden.

### `assertFalse`

Con **`assertFalse`** va evaluar una expresión booleana y va a esperar que el resultado sea **`false`**. En este caso yo lo que quiero es confirmar que mi **`saldo`** nunca sea negativo. Recordemos que **`saldo`** es un **`BigDecimal`** por lo que no podemos comparar directamente con un **`0`**, vamos a utilizar el método **`compareTo(...)`** el cual devuelve un número que puede ser **`1`**, **`-1`** ó **`0`**, ahora vemos que significa esto el código queda así:

![image](https://user-images.githubusercontent.com/23094588/130366190-738db5ab-3fad-4bd7-a25a-6b4739341022.png)

La expresión **`cuenta.getSaldo().compareTo(BigDecimal.ZERO)`** esta comparando el **`BigDecimal saldo`** con el **`BigDecimal.ZERO`**, como mencionamos antes esto me va a dar como resultado un **`1`**, **`-1`** ó **`0`**, cuando el resultado es **`-1`** significa que **`saldo`** es menor que **`BigDecimal.ZERO`** o dicho de otra forma que **`BigDecimal.ZERO`** es mayor que el saldo o sea que el saldo es NEGATIVO.

![image](https://user-images.githubusercontent.com/23094588/130366294-3c217aee-3ba5-4f86-b1dd-fb94b6b90403.png)

y preguntamos si el resultado que nos devuelve, es decir (**`-1`**) es **`< 0`**. Esta condición va a devolver un **`true`** por lo que el **`assertFalse`** fallará y el Test no lo pasaremos. 

Si el el **`saldo`** es positivo el **`compareTo`** devolverá un **`1`**, comparará **`1 < 0`** esto nos devolvera **`false`** y el **`assertFalse`** será correcto por lo que el Test lo pasaremos.

Si el el **`saldo`** es **`0`** el **`compareTo`** devolverá un **`0`**, comparará **`0 < 0`** esto nos devolvera **`false` y el  **`assertFalse`** será correcto por lo que el Test lo pasaremos. 

El Test será correcto mientras el **`saldo`** sea positivo o **`0`**.

Vamos a probar la clase entera.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195825546-1833e2e2-8214-496a-8085-5004bd63a449.png">

Paso la prueba, solo por probar vamos a poner el **`saldo`** negativo para que falle la prueba.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195825772-91703af6-c324-4ecd-9eda-933fcb310f75.png">

La prueba falla por que el **`saldo`** es negativo en el segundo método. Un método paso la prueba pero el otro no. Por lo que el Test no pasa la prueba.

Pero ¿Qué pasa si pongo el saldo negativo en el primer método?

![image](https://user-images.githubusercontent.com/23094588/130366686-deb0716e-d8cd-461b-84bf-c97b17673ed0.png)

El Test es aprobado por que en el primer método no estamos comparando el **`saldo`** y no importa su valor. Vamos a dejar ambos métodos con el saldo positivo.

La sentencia:

```java
assertFalse(cuenta.getSaldo().compareTo(BigDecimal.ZERO) < 0);
```

es quivalente a 

```java
assertTrue(cuenta.getSaldo().compareTo(BigDecimal.ZERO) > 0);
```

Podemos ponerla de una o de otra forma, volvemos a poner el saldo positivo:

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195826824-45c48829-df0b-49ec-ac89-9ebb497fbac0.png">

Validamos lo mismo pero con la lógica inversa.


## 💻 Test Driven Development TDD con JUnit 10:13

En esta lección vamos a ver el Desarrollo en Base a Pruebas Unitarias mejor conocido como **TDD Test-Driven Development**(Desarrollo Dirigido por Tests), esta técnica involucra dos partes:

1. Escribir primero las Pruebas Unitarias
2. Implemnentar el Código, mediante refactorización implementamos los métodos.

Vamos a tener el método sin nada de código, al probar el Test obviamente fallará, empezaremos a meter toda la lógica de negocio en el método volvemos a probar hasta que supere el Test.

El código se desarrollará siempre en base a las Pruebas Orientadas.

### Primer Ejemplo TDD

El ejemplo que vamos a querer implementar es la comparación de dos Objetos **`Cuenta`** para ver si son el mismo (las referencias deben ser las mismas). Cuando comparo dos objetos con el método **`equals`** estoy comparando las referencias. Pero puedo modificar este comportamiento y comparar por Valor en vez de por Referencia, o comparar los Atributos de los objetos si estos son iguales puedo indicar que los Objetos sean considerados iguales aun que realmente sean objetos distintos(por las referencias).

COMMAND + D o CTRL + D: Duplica una línea

Como vamos a usar TDD primero agregagos el método en **`CuentaTest`** y después debemos implementar en **`Cuenta`** el código necesario. 

Vamos a añadir en **`CuentaTest`** el método **`testReferenciaCuenta`** con el siguiente código y ejecutamos todos los método del Test:

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195829798-340e04d8-50da-4367-9016-ceb09a476564.png">

En este nuevo método estamos creando dos Objetos **`cuenta`** con los mismos valores y el test pregunta si estos Objetos son diferentes, cosa que es afirmativa por que los métodos tienen diferentes referencias, el Test lo ha pasado.

Pero que pasa si ***cambia la regla de negocio***, ahora no quiero comparar las instancias de los objetos, sus referencias, lo que queremos ahora es comparar por valor, a nivel de código vamos a comentar el **`assertNotEquals(cuenta2, cuenta);`** y vamos a poner un **`assertEquals(cuenta2, cuenta);`** y vamos a ejecutar el Test.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195831155-3a6781d8-23e8-4384-bf6a-53e13532dfc6.png">

Logicamente la prueba falla por que esta comparando dos instancias, dos referencias distintas, a pesar de que tienen los mismos valores en sus atributos.

Para comparar por valor debemos implementar código en la clase **`Cuenta`**, debemos sobreescribir el método **`equals`** comparando los atributos de la clase **`saldo`** y **`persona`**.

Presionamos COMMAND + N o ALT + INSERT nos aparece el menú contextual

<img width="219" alt="image" src="https://user-images.githubusercontent.com/23094588/195832359-45dd5653-ba8c-44ca-bf01-1b92c038c4ca.png">

donde vamos a seleccionar **Override Methods..** y nos aparece lo siguiente:

<img width="390" alt="image" src="https://user-images.githubusercontent.com/23094588/195833078-f38f269f-cdc7-495e-ab4f-8449c9a13010.png">

Seleccionamos **`equals`** y nos inserta lo siguiente:

<img width="299" alt="image" src="https://user-images.githubusercontent.com/23094588/195833255-c61fc502-402e-4887-80c4-0662bb15f25b.png">

Como podemos ver compara por defefecto por objeto, por instancias. Pero la idea es comparar por atributos **`saldo`** y **`persona`**.

Entonces podríamos hacer lo siguiente:

* Lo primero que deberíamos hacer es validar que el objeto que nos llega no sea **`null`** en caso de serlo retornamos **`false`** que indica que no son iguales.
  ```java
  if (obj == null) {
     return false;
  }
  ```
  
  También deberiamos validar que el objeto que recibimos es una instancia de **`Cuenta`** de no ser así retornamos **`false`**, esto lo hacemos con:
  
  ```java
  if (obj == null || !(obj instanceof Cuenta)) {
     return false;
  }
  ```
  
  pero si vemos lo que nos indica el IDE nos ide que la validación de **`null`** esta cubierta con el **`instanceof`**.
  
  <img width="771" alt="image" src="https://user-images.githubusercontent.com/23094588/195941756-09b15260-0dea-46de-8f09-3d3a0cb95f37.png">

   por lo que finalmente la validación nos basta con lo siguiente:
   
   ```java
  if (!(obj instanceof Cuenta)) {
     return false;
  }
  ```
  
* Lo siguiente es hacer un Cast al **`Object`** que se esta pasando para recibirlo como un objeto de tipo **`Cuenta`**.

  ```java
  if (!(obj instanceof Cuenta)) {
     return false;
  }
  Cuenta c = (Cuenta) obj;
  ```
  
* Lo siguiente que vamos a hacer es validar que los atributos no sean nulos, en caso de que alguno de los dos lo sea nulo devolveremos **`false`** que indica que no son iguales.

  ```java
  // Validaciones
  if (!(obj instanceof Cuenta)) {
     return false;
  }
  Cuenta c = (Cuenta) obj;
  if (this.persona == null || this.saldo == null) {
     return false;
  }
  ```

* Una vez hechas las validaciones ya estamos en capacidad de hacer la comparación de los atributos del Objeto que llega y los atributos de la clase, todo esto lo podemos hacer en el **`retur`** del método.

  ```java
  // Validaciones
  if (!(obj instanceof Cuenta)) {
     return false;
  }
  Cuenta c = (Cuenta) obj;
  if (this.persona == null || this.saldo == null) {
     return false;
  }
  
  return this.persona.equals(c.getPersona()) && this.saldo.equals(c.getSaldo());
  ```
  
El método completo nos ha quedado así:

<img width="681" alt="image" src="https://user-images.githubusercontent.com/23094588/195942754-f29ce9d8-f05f-460c-a066-d01ff9188794.png">
  
Ahora ya podemos retornar a nuestro Test para probarlo.

<img width="712" alt="image" src="https://user-images.githubusercontent.com/23094588/195942898-4533b189-5d86-4007-ace9-cd13a7b4d156.png">

Recordemos que con el **`assertEquals(cuenta2, cuenta);`** el Test fallaba ya que comparaba instancias de Objetos, referencias, pero una vez refactorizado el código implementando el método **`equals`** lo que estamos comparamos ahora son los atributos. Ahora el Test lo debería pasar, vamos a ejecutarlo.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195943981-2b9cf127-c60e-4c13-b128-f81e703d23ef.png">

A pasado el Test.

Vamos a modificar el valor del atributo **`persona`** de uno de los objetos y ejecutamos de nuevo la prueba.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195944188-a9095bf4-b609-43d9-8878-8277d8b4cbc5.png">

El Test ya no lo pasa ya que los atributos no son iguales.

Ahora vamos a poner diferente el atributo **`saldo`** y ejecutamos el Test.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195944560-623efd76-a2d4-4ec0-a34e-0eac19d4c906.png">

El Test sigue sin pasarlo ya que los atributos siguen siendo diferentes.

Vamos a dejar los atributos iguales nuevamente y el Test vemos que lo pasa.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195945022-c739ff97-0990-4faf-b623-df580f74f68f.png">

## 💻 TDD para `debito` y `credito` 11:12

Ahora vamos a crear dos métodos en la clase **`Cuenta`**. 

Definiremos el método **`debito`**, que nos servira para restar un monto del **`saldo`**, por ejemplo cuando realizamos una transferencia.

Definiremos el método **`credito`**, que nos servira para cuando otra cuenta nos realiza una transferencia y agregamos el monto al **`saldo`**, es como sumar y restar.

<img width="331" alt="image" src="https://user-images.githubusercontent.com/23094588/195952977-ce1c4a7c-72fc-4072-9dd5-da36db37d7f1.png">

Solamente los vamos a definir, pero sin implementar. Recuerden que la idea de desarrollar con TDD primero implementamos las pruebas y después implementamos todo nuestro código, pero debemos por lo menos definirlos para poderlos usar en la implementación de las pruebas.

Con TDD primeros debemos escribir las pruebas antes de implementar los métodos en la clase **`Cuenta`**.

### `assertNotNull`

Verifica que el parámetro que se le pasa no sea **`null`**.

En nuestra clase **`CuentaTest`** vamos a implementar el método **`testDebitoCuenta`** como sigue:

<img width="693" alt="image" src="https://user-images.githubusercontent.com/23094588/195954163-03d2dd17-9923-4d00-b437-845cca68a67b.png">

Estamos creando una cuenta para **`Adolfo`** con un **`saldo = 1000.12345`**. Después llamamos al método **`debito`** pasandole como parametro una cantidad **`cuenta.debito(new BigDecimal(100));`**, esto quiere decir que a nuestra cuenta corriente se le van a restar **`100`** de los **`1000.12345`**, por lo tanto, debería tener **`900.12345`**, eso es lo que tenemos que probar.

Lo primero que vamos a probar es que el **`saldo`** no sea **`null`** para esto usamos **`assertNotNull`**, esto es importante por que en métodos anteriores sobre el **`saldo`** aplicamos métodos como **`.equals`** o **`cuenta.getSaldo().doubleValue()`** y si **`saldo`** tiene un valor **`null`** esto nos lanza una excepción. Esto también lo podemos poner en los métodos que ya habiamos codificado.

La segunda prueba que hacemos es comparar solo la parte entera de nuestro saldo con el valor esperado que es **`900`**, los **`BigDecimal`** tienen el método **`.intValue()`** para tratarlo como un número sin sus decimales, un entero.

Finalmente en la tercer prueba lo que estamos haciendo es comparar el valor espado **`"900.12345"`**, observemos que como valor esperado estamos poniendo un **`String`** por lo que en el valor real tenemos que poner también un valor **`String`**, recordemos que el saldo es un **`BigDecimal`**, los **`BigDecimal`** tienen el método **`.toPlainString()`** para tratarlo como texto plano por eso en el valor real estamos colocando **`cuenta.getSaldo().toPlainString()`**.

Vamos a probar este método.

<img width="1507" alt="image" src="https://user-images.githubusercontent.com/23094588/195981052-c1996a27-495f-4d8d-912e-33a1a6c23b32.png">

El Test del método **`testDebitoCuenta()`** nos falla ya que realmente el método **`debito`** no esta implementado y no esta restando el **`monto`** del **`saldo`**, por lo que en **`saldo`** seguimos teniendo **`1000.12345`** y no el valor esperado.

Si ejecutamos todos los métodos los pasa menos el de **`testDebitoCuenta()`**.

<img width="1510" alt="image" src="https://user-images.githubusercontent.com/23094588/195981216-54c58cae-d271-4494-a04c-066a31c2b0cd.png">

Vamos a escribir en nuestro Test el método **`testCreditoCuenta()`** que es muy parecido a **`testDebitoCuenta()`**.

<img width="689" alt="image" src="https://user-images.githubusercontent.com/23094588/195981428-83758b30-ae38-4fc2-97c8-d9870a779a47.png">

Si ejecutamos solo el método **`testCreditoCuenta()`** tenemos:

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195981480-af328b77-524b-4d3b-853d-a08bed0d0884.png">

Vemos que no lo pasa por que esperamos un valor de **`1100`** y al valor actual es de **`1000`**, esto es por que el método **`credito`** no esta implementado y no esta sumando el **`monto`** al **`saldo`**.

Si ejecutamos todos los métodos del Test tenemos que no pasan las pruebas los métodos **`testDebitoCuenta()`** y **`testCreditoCuenta()`** por que no hemos implementado los métodos **`debito`** y **`credito`**, pero recordemos que así trabaja TDD.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195981643-fd38eaed-645f-4f5d-a920-ab1ca57e9f1f.png">

Vamos a implemenar en la clase **`Cuenta`** los métodos **`debito`** y **`credito`**.

Si no conocemos el API de **`BigDecimal`** se podría pensar que es súper simple restando y sumando el monto de la siguiente forma:

<img width="357" alt="image" src="https://user-images.githubusercontent.com/23094588/195982158-fcec9b3c-7bd0-4d6f-985e-f85501bb9bff.png">

Ya hemos implementado los métodos vamos a ejecutar nuevamente el Test.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195982256-a1f0f176-268f-4e25-81d3-383431a571bd.png">

El Test sigue fallando en los métodos **`testDebitoCuenta()`** y **`testCreditoCuenta()`**. 

¿Por qué? 

Recordemos que **`BigDecimal`** es **INMUTABLE** esta es la razón de que siga fallando, cada que se cambia el estado interno de un **`BigDecimal`**, lo que hace no es modificar, no es cambiar el valor del atributo, lo que hace es devolver una nueva instancia, un nuevo **`BigDecimal`** con ese cambio.

<img width="601" alt="image" src="https://user-images.githubusercontent.com/23094588/195982492-367a3db4-fc43-43ee-9fb6-ee7ba82096e0.png">

Pero **`this.saldo`** se mantiene inalterable, inmutable. Entonces, para que esto funcione tenemos que asignar ese valor de la nueva instancia a **`this.saldo`** eso es todo.

<img width="392" alt="image" src="https://user-images.githubusercontent.com/23094588/195982666-a6eb7a42-a658-492f-94e1-d749ad23d4de.png">

Volvemos a ejecutar el Test.

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195982698-325bce7b-11f9-44b3-8e24-a488efa8d6d6.png">

Ahora si pasan todos los métodos del Test.

## Probando y afirmando excepciones con **`assertThrows`** en JUnit 5 12:14

¿Cuál será el siguiente paso?

Bueno, en **`Cuenta`** podemos manejar excepciones, sobre todo en **`debito`** cuando nosotros realizamos una trasferencia. Porque claro, ¿Qué pasa si nuestra **`Cuenta`** tiene menos cantidad de dinero de la que vamos a transferir?

Nuestro **`saldo`** no puede llegar a **`0`** si llega a **`0`**, o incluso peor todavía, si llega a un valor negativo, estaría rompiendo una Regla de Negocio que sería importante para nuestra aplicación, tenemos que manejar esta posibilidad, tenemos que validar que una vez que hayamos restado, recuerden que esto **`this.saldo.subtract(monto)`** es inmutable. Una vez que hayamos restado saldo, antes de asignar el valor **`this.saldo = this.saldo.subtract(monto)`**, poder comprobar que lo que está devolviendo siga siendo mayor que cero y no quedarnos con un monto negativo. Por ejemplo, en caso de quedarnos con un monto negativo, antes de asignar el valor, podemos lanzar una excepción para que no se realice la transacción, lanzamos una excepción, ***No tiene suficiente dinero en la cuenta*** por ejemplo, exception y listo.

Después mediante Pruebas Unitarias también podemos probar, testear con Assertions el manejo de error, más especificamente con **`assertionThrow`**.

### `assertionThrow`

Así como podemos esperar que un método devuelve un valor, también podemos esperar un tipo de error, ***una exception*** que lance un método y evaluar si efectivamente se lanzó la excepción. Por ejemplo, **`nullpointerexception`** o **`numberformatexception`**, validar que ese método en cierto escenario, con ciertos parámetros de entrada, debiese lanzar un error, una excepción.

Y es lo que vamos a probar y que sea de un tipo específico.

En nuestro proyecto vamos a crear un nuevo **`package`** para las excepciones llamado **`org.javaocio.junitapp.ejemplo.exceptions`**.

<img width="341" alt="image" src="https://user-images.githubusercontent.com/23094588/195987970-710c9a07-acc5-46d2-bf26-402152cb6b89.png">

<img width="424" alt="image" src="https://user-images.githubusercontent.com/23094588/195987984-54560ee9-6cfa-4480-b0ba-3bd7eb914013.png">

Dentro de este nuevo paquete vamos a crear la clase **`DineroInsuficienteException`**

<img width="343" alt="image" src="https://user-images.githubusercontent.com/23094588/195988148-473f7645-1a12-48e1-858d-1c4535d57b7f.png">

<img width="909" alt="image" src="https://user-images.githubusercontent.com/23094588/195988170-352cc20d-5691-43e2-ad1a-4a08be3e3dfd.png">

Esta clase debe extender de **`runtimeexception`**, además vamos a insertar un **`constructor`**.

<img width="625" alt="image" src="https://user-images.githubusercontent.com/23094588/195988389-dbb0ae38-cf28-43e9-9bdf-20fa80958d11.png">

Un **`constructor`** que pase un mensaje, para poder personalizar el error con el mensaje.

<img width="560" alt="image" src="https://user-images.githubusercontent.com/23094588/195988501-f3a2deae-993b-4187-96e3-fadbc8067dab.png">

<img width="553" alt="image" src="https://user-images.githubusercontent.com/23094588/195988558-84d5e473-df68-49bb-a2d2-196cb438f13c.png">

Recuerden como estamos trabajando con TDD, primero creamos el Test, después implementamos el lanzamiento excepción en la clase **`Cuenta`** en su método **`debito`** como ya hemos comentado antes, para que a la hora de hacer una transferencia y restar el **`monto`** es decir **`this.saldo.subtract(monto)`** no sea negativo.

Es decir, la persona que está realizando un pago o una transferencia tiene que tener ese monto en la cuenta, de lo contrario no lo va a poder realizar y eso tenemos que validar.

Bien, volvemos a **`CuentaTest`**, vamos a escribir un nuevo método llamado **`testDineroInsuficienteExceptionCuenta()`** con el siguinte código:

<img width="696" alt="image" src="https://user-images.githubusercontent.com/23094588/195989870-0bf034d3-7537-46c5-bb79-37b87cd59709.png">


Al probar tenemos:

<img width="1512" alt="image" src="https://user-images.githubusercontent.com/23094588/195989944-e6fdd791-45b5-411e-846d-0d5b41ac694d.png">

AQUIIIIIIIIIII VOY

texto dinero insuficiente.

Exception

cuenta.

Bajamos.

Cuenta.

Viven Cemal Bueno y pude mantener una misma línea con los mismos datos mil coma un dos tres cuatro cinco

electo.

Hoy tenemos el dato de prueba.

Bien, entonces ahora tenemos que probar que efectivamente, cuando invocamos el método débito, vamos

a realizar una trasferencia y si colocamos un monto mayor al saldo que tenemos en la cuenta, debiese

lanzar esa excepción.

Dinero es suficiente, septón?

Si no la lanza es porque algo está mal y no pasa la prueba.

Bien, para eso tienes que utilizar hacer John's invocando el método hacer frus que para manejo acepcion.

Entonces hacer por acá tenemos frus de lanzar una excepción.

Perfecto.

Lleca recibe dos argumentos primero en nombre de la clase Exception que esperamos se dejan cualquier

clase que sea del tipo cero labelo.

Ese lánzale cualquier objeto exepciones del tipo fuuu label lánzale está mareando de ranka excepto uno

cierto?

Nuestra clase escepción por lo tanto es una sala, así que estaría perfecto.

Vamos a colocar el nombre de nuestra clase Deception, la cual esperamos que se lance dinero insuficiente.

Ahí tenemos dinero suficiente excepto punto class coma y acá una pezon lanta.

Se acuerdan que haya mencionado que Yunis 5 Júpiter?

Una de las características versus la versión anterior es que soporta presiones lanta y después vamos

a ver más ejemplos, pero por ahora todo lo que sea manejo de excepción lo implementamos dentro de esta

expresión.

Lanta una función de fecha.

Entonces vamos a colocar los argumentos que en realidad es sin argumentos.

Lo hagamos vacío flecha.

Y acá las llaves de nuestra persona lanta punto y coma.

Cerramos con punto y coma.

Bien Lleca, invocamos el método que va a lanzar esta acepción, es decir, que supuestamente debería

lanzar esta sección en cierto escenario.

Entonces cuenta punto débito, no?

Cierto.

Vamos a transferir un monto new být décima.

Tiene que ser un monto mayor al que tenemos en la cuenta.

En la cuenta tenemos 1000 reseque, cuyo monto sobre 1000, por ejemplo 1001 mil dos mil quinientos,

por ejemplo mil quinientos debiese lanzar esta sección.

Punto y coma a otro punto importante es que esté haced cruz.

En realidad también devuelve el objeto, excepción que se lanza.

Por lo tanto, acá se captura internamente, sea capturar, sea manejar.

Y este método acá va a devolver el tipo, la instancia que del tipo dinero aceptó.

Aquí lo podremos colocar del tipo más genérico, no lo mismo, porque la idea es obtener el mensaje

error.

Y también podremos comparar el mensaje error y asegurarnos que el mensaje que se lanza sea a uno que

estemos esperando.

Y lo mismo eso lo podemos comparar con el hacer y quarts con el valor esperado y compararlo con el valor

real o actual.

Vamos a colocar Exception Exception igual esto se fijan.

Este método devuelve el objeto escepción y un objeto cesion obtenemos el mensaje.

Entonces del tipo string, por ejemplo string actual o real como le queramos llamar exception punto

get mensajea get me sacho

es trenca, por ejemplo esperado.

No sé, vamos a colocar que nuestro mensaje esperado va a ser dinero insuficiente.

Hacer cuales esperado versus actuar?

Vamos a comparar entonces final se fijan en el mismo método.

Estamos haciendo dos pruebas unitarias que está muy relacionada.

Primero evaluamos el lanzamiento escepcion que es el mismo.

Luego lo capturamos perfecto.

Si todo sale bien, vamos a evaluar.

Vamos a probar el mensaje error que sea el correcto el que efectivamente se viviese de incluir a cocinar.

Cuando se lanza la acepción.

Pero ojo, todavía no hemos implementado ningún método en cuenta.

Todavía no estamos implementando este código de manejo decepción en el método débito.

Solamente estamos probando.

Entonces ahora vamos a ejecutar como siempre.

Y ver qué pasa.

Obviamente la falla no es cierto.

Primero dice que se esperaba dinero es suficiente Сервис John's, pero no devolvió nada.

En realidad se fijan, no pasó nada y el otro error ni siquiera se ejecuta.

Acá el assert y quĂŠ?

Porque ni siquiera está lanzando la excepción.

Entonces eso es lo que vamos a implementar ahora.

Primero probamos, luego implementamos.

Bien.

Vamos a cerrar.

Vamos a quitar esto.

Ocultar.

Y continuamos.

No vamos a ir a cuenta por acá en débito.

Bueno, lo primero antes de asignar acá vamos a modificar y vamos a asignar a una variable local del

método que vamos a colocar el tipo, porque es una variable local.

Bit decimal.

Nombre variable.

Nuevo saldo.

Bajamos y comparamos nuevo salto.

Entonces con un NIF.

Nuevo salto.

Punto.

Recuerden que es un bidé.

Si comparamos con él.

Comparto comparto.

Lo comparamos con bit decimal cero bit decimal punto cero.

Entonces, si esta expresión devuelve un valor negativo, es decir menos 1, significa que cero es mayor

que nuevo saldo.

Por lo tanto, la persona no tiene dinero suficiente para realizar la transacción y viviésemos de lanzar

una excepción.

Entonces acá comparamos.

Entonces, si en menor o negativo, en menor que 0 perfecto, lanzamos la excepción throw new dinero

insuficiente excepto por constructor.

Recuerden que implementamos el constructor para pasar un mensaje personalizado.

Vamos a colocar el mismo mensaje con el cual queremos probar.

Dinero es suficiente.

Dinero insuficiente.

Perfecto.

Pero si todo sale bien y no lanza ninguna excepción, entonces tenga que asignar 10 puntos.

Saldo igual.

Nuevo saldo.

Perfecto.

Importante asignar, cierto.

Si no se cumple la regla negocio lanzar una sesión.

Y esto no se realiza.

No se realiza la transacción.

No se le asigna el valor del nuevo saldo a saldo.

No se resta.

Se fijan.

Simplemente no se lleva a cabo la transacción.

Y estamos listos.

Ahora tenemos implementado manejo acepcion.

Bien.

Volvamos entonces con control TAF.

Volvemos a cuenta test y vamos a probar un clic afuera.

Ejecutamos como siempre.

De ahí pasó, pasó todas las pruebas.

Dinero suficiente.

Está perfecto, en realidad, claro, pusimos 1500.

La persona no tiene dinero suficiente.

Lanza excepcion, que está bien, porque acá estamos simulando el error.

Lo queremos producir de forma intencional para ver cómo se está manejando el error.

La acepción dinero suficiente se lanza y es la esperada.

Así que está bien.

Luego probamos el mensaje error.

Ahora, qué pasa si cambiamos el dinero suficiente y le ponemos otro texto?

Por ejemplo, dinero suficiente.

En la cuenta se fijan este de acá va a pasar, pero este de acá no pasa.

Se fijan porque no es igual, no va a ser igual a la esperada.

Ejecutamos y la prueba falla.

Esperado dinero suficiente en la cuenta actual dinero suficiente.

Este es el mensaje real que se lanza en la acepción Yetta.

Qué estamos esperando?

No es igual.

Así que está bien.

Estamos probando correctamente.

Lo dejamos como estaba.

Ahora, qué pasa si en vez de dinero es suficiente?

Exception?

No sé.

Vamos a colocar otro tipo de clase.

Decepción, por poner un ejemplo, un number forma exception y ni siquiera necessary que lo hagan solamente

para que lo tengan en cuenta y lo vean.

Un Amper formato Exception.

Acá lo tenemos.

Triplican.

Supongamos que esperábamos este error, pero en realidad se va a lanzar el dinero suficiente.

Entonces esta prueba no se pasa.

Veamos.

Inesperada acepción del tipo.

Bueno, se espera un ámber format exception, pero finalmente se lanza un dinero en suficiente septón.

No coinciden los tipos.

Entonces esta forma podemos evaluar, probar el manejo de error.

Hará lo correcto en la prueba.

Es como lo teníamos con el dinero suficiente.

Eso es lo correcto.

Ahora, bueno, va a pasar esta prueba.

Pero qué pasaría si Test Débito está acá?

Lo modificamos y en débito colocamos, no sé, 2000 por ejemplo, tampoco en esa serie que lo hagas

solamente para que lo vean y lo tengan en cuenta.

Voy a colocar dos mil.

Obviamente va a fallar porque lanza la selección y ni siquiera va a pasar el test, ni siquiera se va

a ejecutar esto de acá, porque primero se lanza la excepción en débito y falla.

Lo demás ni siquiera se evalúa bien.

Veamos un clic afuera.

Ejecutamos todo.

Se fijan, pasa dinero suficiente porque está bien, acepto, pero débito no pasa porque se lanza la

excepción, dinero insuficiente, excepto se fijan, se lanza recesión, falla la prueba automáticamente.

Entonces todo bien, lo están probando con nuestra regla negocio.

Bien.

Vamos a dejar como estaba Conscien.

Volvemos a probar y está todo bien.

Perfecto.

Bueno, eso sería manejo decepciones co Cunit, pruebas unitarias.

Lo dejamos hasta acá y nos vemos en la siguiente clase.

## Añadiendo la clase Banco y la realcion con las cuentas 09:55
## Probando y afirmando las realciones entre Banco y Cuenta 08:01
## Usando el método **`assertAll`** 07:10
## Agregando mensajes de falla en los métodos **`assertions`** 07:29
## Usando anotaciones **`@DisplayName`** y **`@Disabled`** 08:02
## Ciclo de vida anotaciones **`@AfterEach`** y **`@BeforeEach`** 11:12
## Ciclo de vida anotaciones **`@AfterAll`** y **`@BeforeAll`** 07:25
## Test condicionales **`@EnabledOnOs`**, **`@EnabledOnJre`**, **`@EnabledIfSystemProperty`** etc. 18:13
## Ejecuciones de test condicionales con **`@EnabledIfEnvironmentVariable`** 08:45
## Ejecución de test condicional con **`Assumptions`** programáticamente 10:17
## Clases de test anidadas usando **`@Nested`** 11:03
## Repitiendo pruebas con **`@RepeatedTest`** 08:36
## Escribiendo pruebas parametrizadas con **`@ParameterizedTest`** 10:50
## Pruebas parametrizadas con **`@ParameterizedTest`** parte 2 08:03
## Pruebas parametrizadas con **`@ParameterizedTest`** parte 3 11:15
## Tagging tests con anotación **`@Tag`** 06:47
## Inyección de Dependencia & componentes **`testInfo`** y **`TestReporter`** 12:38
## **`Timeout`** en JUnit 5 09:06
## Maven surefire plugin 09:50
## Descargar Código Fuente 00:03

