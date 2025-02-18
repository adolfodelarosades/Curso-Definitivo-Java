# 1. Introducción  60m

* 01 Presentación 4:31 
* 02 Introducción a Spring 14:25 
* 03 Instalación del entorno de trabajo 13:55 
* 04 Estructura de una aplicación empresarial y patrones de diseño 12:55 
* 05 Inversión de control e inyección de dependencias 15:09 
* Contenido adicional  5

# 01 Presentación 4:31

[PDF 1-1_Presentacion_del_curso.pdf](pdfs/1-1_Presentacion_del_curso.pdf)

## Transcripción

<img src="images/1-01.png">

Vengo a presentaros el curso de Spring core 5.

<img src="images/1-02.png">

Si quieren realizar el curso de Spring 5 tendríamos algunos requisitos iniciales importante y otros que son deseables, es necesario conocer Java para poder trabajar con Spring os recomiendo que además que trabajen con Java con soltura sobre todo Java SE 8, también sería necesario que manejemos conceptos de metodología de programación orientado a objetos como pueden ser clase, interfaz, herencia, polimorfismo, etc., además estaría muy bien si sabéis algo sobre Java EE versión 7, Maven y también de patrones de diseño aunque no es estrictamente obligatorio.

<img src="images/1-03.png">

¿Qué me va aportar a mí el poder saber sobre Spring? si hacemos este curso de Spring y aprendemos esta tecnología sobre todo en la parte Core vamos a poder conocer algunos de los patrones de diseño que se utilizan más en el desarrollo de aplicaciones empresariales, también vamos a poder reconocer la utilidad del uso de la **inversión de control y la inyección de dependencias** como mecanismo que nos permiten desacoplar nuestro software y hacer un software que sea robusto, refactorisable, reutilizable y en definitiva manejar unos conceptos que son transversales al resto de proyectos y de módulos de Spring.

<img src="images/1-04.png">

Los contenidos del curso son sencillos comenzaremos haciendo una introducción a esta tecnología por poder centrarnos y aterrizar, instalaremos el entorno de desarrollo, a partir de ahí empezamos a conocer el contenedor de inversión de control de Spring, las interfaces y clases que orbitan alrededor de este concepto, que es un *bean*, veremos también cual es su ámbito, su ciclo de vida y por último terminaremos con dos secciones en las que aprenderemos además de lo que ya hayamos aprendido a configurar nuestras aplicaciones vía XML, lo aprenderemos a hacer a través de anotaciones con las distintas anotaciones que podemos utilizar y por último la configuración a través de Java, finalizaremos el curso con un ejemplo que integre la gran mayoría de los conceptos que hayamos visto lo largo de las diferentes lecciones. 

<img src="images/1-05.png">

Si bien es cierto en cada una de las lecciones podemos ir viendo un pequeño proyecto, uno o varios pequeños proyectos de ejemplos que ponen en practica esos conceptos.

<img src="images/1-06.png">

<img src="images/1-07.png">

¿Qué seré capaz al finalizar el curso? serás capaz de haber aprendido el uso de algunos patrones de diseño, serás capaz de diferenciar cuáles son los módulos más típicos de Spring y algunos de los proyectos que existen, también en reconocer la importancia de la inversión de control y de inyección de dependencia, habrás instalado el entorno de desarrollo con el cual vamos a poder trabajar con tus proyectos de Spring, el mecanismo de creación de beans y la configuración a través de las distintas tecnología y utilizar el contenedor de inversión de control y toda la potencia que ofrece.

<img src="images/1-08.png">

Una vez finalizado el curso te puedo recomendar algunos otro de los que tenemos en nuestro catálogo por si quieres continuar en tecnologías que son adyacentes.

# 02 Introducción a Spring 14:25

[PDF 1-2_Introduccion_a_Spring.pdf](pdfs/1-2_Introduccion_a_Spring.pdf)

## Resumen Profesor

### Proyectos que conforman Spring

A día de hoy, Spring se considera algo más que un framework, ya que incluye una gran cantidad de proyectos que abarcan ámbitos diferentes. En este enlace, puedes encontrar un listado completo:

https://spring.io/projects

### Novedades de Spring 5

En este enlace podrás encontrar todas las novedades incluidas en las últimas versiones de Spring.

https://github.com/spring-projects/spring-framework/wiki/What's-New-in-Spring-Framework-5.x

## Transcripción

<img src="images/2-01.png">

Hola a todos y bienvenidos a esta primera lección de este curso de Spring Core en el que vamos a introducirnos en esta apasionante tecnología.

<img src="images/2-02.png">

Bueno y vamos a conocer un poco cuando hablamos de Spring a qué nos referimos exactamente. 
Comencemos con esta parte que para los que se introducen en esta tecnología a veces le causa algo de desazón porque cuando decimos Spring a que nos estamos refiriendo.

<img src="images/2-03.png">

Vamos a tratar de dar una definición sencilla y después vamos a ir conociendolo poco a poco. Spring es un framework, de código abierto, para la creación de aplicaciones empresariales Java, que además tiene soporte para otros lenguaje como Grovy y Kotlin, con estructura modular que tiene una gran flexibilidad y que nos permite implementar diferentes tipos de arquitecturas según las necesidades que tenga la aplicación.

Podremos decir que esto es a vista de pajaro una definición de lo que podría ser Spring.

<img src="images/2-04.png">

Lo que pasa que sobre Spring hay mucha literatura encontrarán toneladas de artículos de post en algunos blogs pueden encontrar de todo y cuando hacen referencia a Spring pueden estar haciendo referencia a muchas cosas. La mayoría de ellas harán referencia al proyecto Spring Framework qué dentro del ecosistema de Spring es el proyecto nuclear, sobre todo en la parte de dentro de este núcleo, bueno pues el contenedor de inversión de control, el que nos va a permitir gestionar la inyección de dependencias, pero también por muchas más funcionalidades como Programación Orientada a Aspectos, el uso de recursos, la validación de datos, etc.

Cuando también leamos por ahí Spring quiza se estarán refiriendo a toda la familia de proyectos que vamos a ver que son muchos los proyectos que han ido creciendo en ese en este ecosistema, algunos de ellos con con varios módulos.

Incluso haciendo un abuso del lenguaje pues también podríamos encontrar con que alguien habla de Spring y se está refiriendo a un entorno de desarrollo, que instalaremos en las próximas lesiones basada en Eclipse y que se llama Spring Tool Suite.

Ya nos vamos haciendo poco a poco una idea.

<img src="images/2-05.png">

Dentro de cualquier rama de conocimiento, dentro de la computación también no está mal conocer algo historia y porque surge Spring. 

<img src="images/2-06.png">

A finales de los noventa en el inicio del siglo 21 las aplicaciones empresariales desarrolladas sobre Java, se desarrollaba dentro de la especificación de Java EE, que en aquel entonces se le conocía como Java2EE y utilizando un modelo conocido como los **EJB los Enterprise Java Bean** en el que bueno una aplicación podía tener esta arquitectura, una base de datos que normalmente era relacional, sobre esa base de datos descansaba el servidor de Java EE, en ocaciones podríamos encontrar esta capa también fragmentada en dos servidores distintos incluso podrían estar en máquinas distintas, el contenedor de EJB en el servidor de Java EE empresarial propiamente dicho donde tendríamos los Enterprise JavaBeans que son unas clases especiales gestionadas por este contenedor sobre el encontraríamos el contenedor Web el contenedor de Servlets donde podíamos trabajar con Servlets o directamente con páginas JSP y en la capa cliente podríamos encontrar aplicaciones web que se conectaría a este contenedor Web directamente o incluso podríamos tener también desarrolladas aplicaciones que mediante algún tipo de tecnología por ejemplo RMI se podían conectar directamente a los objetos del contenedor de EJBs.

<img src="images/2-07.png">

Este modelo es ciertamente complejo tenía sus ventajas y sobre todo sus desventajas y Spring surge como respuesta a este modelo. Algunas desventajas del modelo de EJB es que es altamente complejo, la productividad no era para nada buena, es decir que el programador tenía que invertir mucho trabajo en programar y configurar para poder hacer algo de código que diera rendimiento, la conectividad entre componentez está basada en la tecnología RMI, que ya digo también que es un protocolo complejo, frente a otros protocolos mucho más extendidos como HTTP, estaba basada en el uso de componentes remotos cuando hay aplicaciones de tamaño mediano o pequeño incluso que no necesitan que esos componentes residan en un servidor independiente sin embargo el modelo de EJB nos obligaba al uso de componentes remotos, una gran dificultad a la hora de depurar y buscar errores y un mapeo objeto-relacional pues ciertamente limitado. 

<img src="images/2-08.png">

Toda esta serie de dificultades dan como respuesta que en el año 2003 y gracias al trabajo de un señor llamado Rob Jonson naciera Spring como respuesta a su experiencia, con su mala experiencia sobre todo con el desarrollo de aplicaciones empresariales Java. Spring nace como un complemento a esa especificación, a la especificación de Java EE y no como un sustituto directo sino que bueno pues dentro de esa gran especificación de Java EE va integrando algunos componentes como el API de Servlets, el API de los WebSocket,  el bloque de concurrencia, el Binding de JSON, la validación de beans, la API de persistencia, los va integrando de tal manera que incluso nosotros podremos hacer uso de algunos de estos componentes sueltos sin tener que utilizar todos los demás.

<img src="images/2-09.png">

Ya hemos visto algo sobre los orígenes de Sprint vamos a conocerlo los proyectos y algunos de los módulos que lo integran.

<img src="images/2-10.png">

Si entráis en la página de Spring https://spring.io/ podemos comprobar que en el apartado de proyectos https://spring.io/projects hay bastantes, les presento aquí algunos.

El nucleo es Spring Framework es el primero que surgio y sobre el cual han nacido los demás y que el que nos da soporte para la inyección de dependencia, gestión de transacciones, aplicaciones web, recursos, mensajería, programación orientada a aspectos, todo ello va incluido en el proyecto de Spring Framework.

Hace algunos años surgio Spring Boot que es un acelerador de construcción de aplicaciones de Spring, comprobaremos en este curso que configurara una aplicación de Spring no es precisamente trivial si bien es bastante más facíl que otros sistemas como el de Java EE y Spring Boot utiliza el principio de convensión sobre configuración para que el programador se centre en programar y ofrecerle una configuración acelerada de nuestro proyecto para que tenga todo disponible lo antes posible.

Spring Data nos ofrece una interfaz común de acceso a datos ya sea relacionales o no relacionales e integra yo diría que entre 20 y 25 subproyectos diferentes para poder acceder a datos.

Spring Security nos permite proteger nuestra aplicaciones dando un soporte sencillo y extensible para realizar la operación básicas de autenticación y autorización con diferente esquemas.

Spring Cloud es un conjunto de herramienta para muchas tareas de sistemas distribuidos y que están bastante de moda ahora asociada al concepto de construcción y despliegue de microservicios. 

Spring HATEOAS simplifica la construcción de representaciones REST siguiendo el principio del hypertexto como  el motor del estado de la aplicación, es lo que significa ese acrónimo y que en definitiva que nos va a permitir crear servicios WEB REST de una manera francamente sencilla y con un esquema muy definido.

Spring Batch es otro proyecto que nos permite simplificar y optimizar el procesamiento de grandes volúmenes de operaciones por lotes.

Spring for Android qué es una extensión de Spring para simplificar el desarrollo de aplicaciones Android nátivas y los puntos suspensivos cita algunos proyectos más que si queréis conocer podéis visitar la página de Spring.

### [Spring Projects](https://spring.io/projects)

### [Spring Framework](https://spring.io/projects/spring-framework)

Spring Framework proporciona un modelo integral de programación y configuración para aplicaciones empresariales modernas basadas en Java, en cualquier tipo de plataforma de implementación.

Un elemento clave de Spring es el soporte de infraestructura a nivel de aplicación: Spring se enfoca en la "plomería" de aplicaciones empresariales para que los equipos puedan enfocarse en la lógica de negocios a nivel de aplicación, sin vínculos innecesarios con entornos de implementación específicos.

#### Caracteristicas

* [Core technologies](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html): dependency injection, events, resources, i18n, validation, data binding, type conversion, SpEL, AOP.
* [Testing](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/testing.html): mock objects, TestContext framework, Spring MVC Test, WebTestClient.
* [Data Access](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/data-access.html): transactions, DAO support, JDBC, ORM, Marshalling XML.
* [Spring MVC](https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html) y [Spring WebFlux](https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html) web frameworks.
* [Integration](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/integration.html): remoting, JMS, JCA, JMX, email, tasks, scheduling, cache.
* [Languages](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/languages.html): Kotlin, Groovy, dynamic languages.

### [Spring Boot](https://spring.io/projects/spring-boot)

Spring Boot facilita la creación de aplicaciones independientes basadas en Spring de grado de producción que puede "simplemente ejecutar".

Tomamos una visión obstinada de la plataforma Spring y las bibliotecas de terceros para que pueda comenzar con un mínimo de alboroto. La mayoría de las aplicaciones Spring Boot necesitan una configuración mínima de Spring.

#### Caracteristicas

* Crear aplicaciones independientes de Spring
* Incrusta Tomcat, Jetty o Undertow directamente (no es necesario implementar archivos WAR)
* Proporcione dependencias "iniciales" obstinadas para simplificar su configuración de compilación
* Configura automáticamente las bibliotecas de Spring y de terceros siempre que sea posible
* Proporciona funciones listas para producción, como métricas, comprobaciones de estado y configuración externa.
* Absolutamente ninguna generación de código y ningún requisito para la configuración XML

### [Spring Data](https://spring.io/projects/spring-data)

La misión de Spring Data es proporcionar un modelo de programación familiar y consistente basado en Spring para el acceso a los datos, al mismo tiempo que conserva los rasgos especiales del almacén de datos subyacente.

Facilita el uso de tecnologías de acceso a datos, bases de datos relacionales y no relacionales, marcos de reducción de mapas y servicios de datos basados en la nube. Este es un proyecto general que contiene muchos subproyectos que son específicos de una base de datos determinada. Los proyectos se desarrollan trabajando en conjunto con muchas de las compañías y desarrolladores que están detrás de estas emocionantes tecnologías.

#### Caracteristicas

* Potente repositorio y abstracciones personalizadas de mapeo de objetos
* Consulta dinámica derivada de nombres de métodos de repositorio
* Implementación de clases base de dominio que proporcionan propiedades básicas
* Soporte para auditorías transparentes (creado, modificado por última vez)
* Posibilidad de integrar código de repositorio personalizado
* Fácil integración de Spring a través de JavaConfig y espacios de nombres XML personalizados
* Integración avanzada con controladores Spring MVC
* Soporte experimental para la persistencia entre tiendas

### [Spring Security](https://spring.io/projects/spring-security) 

Spring Security es un marco de autenticación y control de acceso potente y altamente personalizable. Es el estándar de facto para asegurar aplicaciones basadas en Spring.

Spring Security es un marco que se centra en proporcionar autenticación y autorización a las aplicaciones Java. Al igual que todos los proyectos de Spring, el verdadero poder de Spring Security se encuentra en la facilidad con que se puede extender para cumplir con los requisitos personalizados.

#### Caracteristicas

* Soporte completo y extensible para autenticación y autorización
* Protección contra ataques como session fixation, clickjacking, cross site request forgery, etc
* Integración API Servlet
* Integración opcional con Spring Web MVC
* Mucho más...

<img src="images/2-11.png">

[Framework Modules](https://docs.spring.io/spring/docs/5.0.0.RC2/spring-framework-reference/overview.html#overview-modules)

Dentro de del proyecto de Spring Framework lo que está más cercano a él encontramos que también tiene algunos de los módulos, estos módulos también están integrados dentro de algunos paquetes que a su vez tienen otros módulos dentro como es el caso del **Core Container** que a su vez ya digo tiene dentro de algunos submodulos como el Context, Core, Beans, el expresión language. Tambien tenemos el de acceso a datos, el web nos vamos a ir haciendo un poco una idea. De todas formas ya veremos como en este curso trabajamos los proyectos sobre todo dentro del  **Core Container** nos vamos a centrar en aprender lo que transversalmente vamos a necesitar para cualquier tipo de otro proyecto de Spring ya sea Web, de acceso a datos, de seguridad, un servicio web REST, etc.

<img src="images/2-12.png">

<img src="images/2-13.png">

Spring como hemos visto desde 2003 a día de hoy ha tenido un largo recorrido, hace algunos meses veíamos el lanzamiento de  Spring 5, de hecho nos encontramos con que la versión actual, la que está publicada con disponibilidad general en la 5.0.8 si bien ya tenemos una versión candidata de la 5.1 como podéis ver en el gráfico de la derecha, también tenemos ya versiones no liberadas algunas SNAPSHOT como la 5.0.9, sigue estando también la posibilidad de usar la versión se Spring 4, la última versión publicada es la 4.3.18 por si queremos seguir teniendo algo de compatibilidad con otras librerías que todavía no estén preparadas para para Spring 5.

Actualmente 5.2.6 GA 27/05/2020

Actualmente 5.2.8 CURRENT GA 21/08/2020

<img src="images/2-14.png">

Qué novedades nos trae Spring 5 para aquellos que no conozcáis algo sobre Spring. El primero es el uso totalmente adaptado del JDK 8 y de la especificación 7 de Java EE, ya Spring 5 viene totalmente adaptado a Java 8, incluso se plantearon el que estuviese totalmente adaptado JDK 9 pero bueno la diferencia de velocidades en la publicación de ambos no lo ha permitido, si es verdad que nos permite tener compatibilidad con el JDK 9 y con la especificación 8.  Las versiones mínimas de algunos APIs que utilizamos son Servlet 3.1 en el caso de los servlets, Validation 1.1 en la validación, JPA 2.1 si bien entre Spring 5 y la versión candidata que tenemos de la 5.1 vamos a poder ver como ya podremos tener acceso a las APIs de Servlet 4.0, Validation 2.0, JPA 2.2 que trae algunas novedades interesantes etc. Con respecto a los servidores sobre los cuales puede correr pues también tenéis aquí algunas versiones con las que ya es disponible y bueno podríamos decir que dos de las novedades más interesantes que ofrece es la posibilidad de **hacer aplicaciones web reactivas mediante el módulo webflux** y también **la programación funcional con Kotlin** este otro nuevo lenguaje de programación este superset de Java serían las dos novedades más interesantes que nos ofrece Spring 5.

Nosotros en este curso nos centraremos en la parte Core y en comprender todo lo transversal a Sprint que podemos utilizar con el resto de módulos, una vez que ya conocemos qué es Spring las versiones y las novedades que nos ofrecen, en la próxima lección vamos a instalar nuestro entorno de desarrollo para poder comenzar a trabajar.

# 03 Instalación del entorno de trabajo 13:55

[PDF 1-3_Instalacion_del_entorno.pdf](pdfs/1-3_Instalacion_del_entorno.pdf)

## Resumen Profesor

### Requisitos

Para poder trabajar con Spring, necesitamos que nuestro ordenador disponga de una instalación de JDK 8. Puedes encontrar la información en https://www.oracle.com/technetwork/java/javase/downloads/index.html.

### Spring Tool Suite

Nuestro IDE será Spring Tool Suite, que está basado en Eclipse, e incluye muchas facilidades para trabajar con la familia de proyectos de Spring. Se puede instalar como un plugin sobre una instalación de eclipse, pero si vas a comenzar desde cero, te recomendamos descargar el *bundle* disponible en https://spring.io/tools/sts.

### Instalación vía ubuntu-make

Ubuntu Make es una herramienta de línea de comandos que permite descargar e instalar la última versión de varios entornos de desarrollo. También se encarga de instalar las dependencias necesarias, así como de crear los lanzadores (accesos directos). Básicamente, se trata de un comando para poner tu sistema a punto para desarrollar.

Entre otras herramientas, *Ubuntu make* permite la instalación de *Spring Tool Suite*. Para ello, debemos seguir los siguientes pasos:

### Instalar Ubuntu Make

Añadimos el repositorio PPA a nuestro sistema

```sh
$ sudo add-apt-repository ppa:lyzardking/ubuntu-make
$ sudo apt-get update
```

Instalamos ubuntu make

```sh
$ sudo apt-get install ubuntu-make
```

Ya podemos usarlo para instalar Spring Tool Suite

```sh
$ umake ide spring-tools-suite
```

## Transcripción

<img src="images/3-01.png">

Hola a todos vamos a lanzarnos de lleno ahora a poder utilizar Spring y lo vamos a hacer instalando nuestro entorno de trabajo.

<img src="images/3-02.png">

Lo primero que tenemos que ver es que requisitos previos necesitamos para poder trabajar con Spring.

<img src="images/3-03.png">

Deberíamos manejar la tecnología Java, el lenguaje de programación Java, sobre todo la parte de metodología de programación, sintaxis básica y orientación a objetos con eso será suficiente si bien nos vendrá bien que sepamos algo de Java EE o por lo menos algunos conceptos saber que puede llegar a ser un Servlet, algunos elementos de aplicaciones empresariales, es verdad que hemos nombrado lo de EJB, también vendría bien manejar algunos de esos conceptos, también algo sobre patrones de diseño y el manejo de Maven que lo utilizaremos para gestionar las dependencias asociadas a Spring, veremos cuál es la dependencia que tenemos que añadir en nuestro proyecto para poder utilizar al menos el contenedor de inversión de control de Spring. 

<img src="images/3-04.png">

<img src="images/3-05.png">

Para poder trabajar con Spring realmente podríamos trabajar con cualquier entorno de desarrollo integrado que nos permita trabajar con Java y con Maven preferentemente con que nos permitierá trabajar con Java sería suficiente. Podemos optar por usar Gradle en lugar de Maven para la gestión de dependencia también podríamos trabajar con el. El primero que siempre surge cuando hablamos de programación Java es Eclipse porque es un IDE de que está ampliamente extendido su uso por la comunidad y francamente es uno de los más potentes del mercado, también tenemos otras alternativas como son Netbeans, IntelliJ Idea de la empresa Jetbrains, Visual Studio Code cualquiera de ellos nos permitiría trabajar con Sprint por el mero hecho de que podemos trabajar directamente con JAVA.

<img src="images/3-06.png">

Si es verdad que vamos a escojer como IDE preferido uno que nos propone directamente desde la comunidad de desarrolladores de Spring que es **Spring Tool Suite** se trata de un plugin para Eclipse o que también vamos a comprobar que lo podemos encontrar como un Bundle como un integrado que nos podemos descargar. Viene ya perfectamente listo para poder utilizar Spring con un monton de capacidades tiene todas las ventajas del uso de Eclipse de hecho si ya has trabajado con Eclipse todo lo que sepa se podrá aplicar con Spring Tool Suite pero añade también algunas ventajas algunas de ellas son el soporte para **Cloud Foundry** o para **Pivotal tc Server**. [Cloud Foundry](https://www.cloudfoundry.org/) nos permite acceder a una infraestructura una plataforma como un servicio donde podemos construir, probar y desplegar nuestras aplicaciones que además serán escalable y [Pivotal tc Server](https://www.vmware.com/products/pivotal-tcserver.html) es un servidor de Tomcat tuneado, es un servidor que tiene lo mejor de Tomcat y algunos elementos de servidores de aplicaciones que no incluye Tomcat. Incluye soporte para Maven y Gradle.

<img src="images/3-07.png">

https://spring.io/tools

A día de hoy la última versión publicada en la 3.9.5 (The all-new Spring Tool Suite 4. Free. el 21/08/2020) está basada en Eclipse Oxygen la versión 4.8 con la versión del Pivotal TC Server 3.2.6 y ofrece ya soporte para Java 9. Es gratuito lo podemos descargar desde la misma Web de Spring y vamos a aprender en esta lección 
cómo podemos instalarla.

<img src="images/3-08.png">

Para ello vamos a realizar la instalación yo este curso ya lo habéis comprobado que estoy trabajando sobre Ubuntu y vamos a hacer o voy a hacer una demostración de la instalaciones en una máquina virtual que tengo por aquí.

<img src="images/3-09.png">

Lo primero que tendríamos que hacer es comprobar si tenemos instalada la máquina virtual de Java JDK y comprobar la versión que tenemos para ver que sea la 1.8 que será la que utilizamos a lo largo de este curso podemos comprobar con

```sh
java -version
```

En caso de no tenerla instalada os recomiendo que con el gestor de paquetes que utililicen habitualmente la puedan descargar.

<img src="images/3-10.png">

Una vez que hemos comprobado que tenemos Java listo nos descargamos el Bundle de Spring Tool Suite en https://spring.io/tools para nuestro sistema operativo en nuestro caso la versión de 64 bits de Linux. Si trabajan sobre Windows o sobre Mac también lo podéis descargar directamente, incluso si queréis alguna versión más antigua también hay posibilidad de descargarla, una vez descargado en la carpeta de descarga es un fichero tar.gz el que se descargaría, en el caso de una Mac es `spring-tool-suite-4-4.7.1.RELEASE-e4.16.0-macosx.cocoa.x86_64.dmg`.

<img src="images/3-11.png">

A partir de allí lo que lo que vamos a hacer un sencillo proceso de instalación nos vamos a ir a la carpeta de descarga, vamos a mover este fichero a la carpeta `opt` y allí lo vamos a descomprimir, el nombre de fichero que se van descomponiendo.

<img src="images/3-12.png">

Una vez que se hayan descomprimido vamos a referenciar el ejecutable, la verdad es que sería tan sencillo como que descomprimiéramos la carpeta aquellos que se sienten menos cómodos por la línea comando a través del gestor de archivos comprimidos y la tuvieramos por ejemplo en el escritorio y con un doble click pudiéramos iniciar nuestro entorno de desarrollo, haciendo doble clic sobre el fichero STS, pero bueno vamos a tratar de hacer una instalación un poco mejor y que la tengamos con un Launcher. Vamos a crear un enlace simbólico para tenerlo dentro de la carpeta de binarios locales. Lo hacemos directamente con la ruta completa.

<img src="images/3-13.png">

Realmente ya podríamos invocar a Spring Tool Suite desde la línea de comandos pero bueno si nos interesa incluir algún tipo de icono, lanzador para poder utilizarlo pues sería interesante que lo pudiéramos hacer, para ello vamos a incluir esto dentro de la carpeta application, dónde vamos a crear un lanzador habitual de los lanzadores, podéis copiar directamente y de esa manera vamos a poder utilizar el lanzador.

<img src="images/3-14.png">

Se crearían nuestro lanzador ya lo tenemos por aquí, lo podríamos ejecutar, incluso si queremos lo podemos anclar al dock para tenerlo más a la vista si lo vamos a utilizar con frecuencia.

La primera vez que arranca nos pregunta donde queremos situar el Workspace esto es una característica básica de Eclipse, podemos seleccionar el Workspace que queramos.

<img src="images/3-15.png">

<img src="images/3-16.png">

Por último decir que está es la dependencia Maven que vamos a utilizar, siempre que vayamos a trabajar con proyectos de Spring Core, la dependencia Spring Context comprobaremos que se descarga varios jars por detrás varias "librerías" de manera que nos da todo lo necesario para poder trabajar con Spring Context, si no recordamos esto que cuando va a ir trabajando poco a poco la verdad es que no es tan difícil de recordar, el grupo y el artefacto, siempre podemos acceder a la web del repositorio Maven https://mvnrepository.com/ para poder copiar la dependencia, primero la buscamos en su buscador seleccionamos la versión, estamos trabajando con la 5.0.8

<img src="images/3-100.png">

<img src="images/3-101.png">

<img src="images/3-102.png">

y directamente tendríamos la dependencia Maven para poder copiarla y pegarla directamente en nuestro archivo `pom.xml`

```sh
<!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.2.8.RELEASE</version>
</dependency>
```

Con esto terminamos la lección de la instalación del entorno.

# 04 Estructura de una aplicación empresarial y patrones de diseño 12:55

[PDF 1-4_Estructura_y_patrones.pdf](pdfs/1-4_Estructura_y_patrones.pdf)

## Transcripción

<img src="images/4-01.png">

Hola a todos vamos a continuar en esta lección hablando un poco sobre la estructura de una aplicación empresarial y también sobre patrones de diseño que es otro ámbito que se suele trabajar y que nos servirá como base también para poder utilizar un buen diseño en nuestros proyectos de Spring.

<img src="images/4-02.png">

<img src="images/4-03.png">

Hablemos primero de **Aplicaciones Empresariales**, podemos decir que una aplicación empresarial es una *gran* aplicación comercial y pongo en cursiva *gran* porque normalmente podríamos decir que una aplicación empresarial es cualquier aplicación dedicada a una empresa, si bien está fuera pequeña, mediana o grand, pero en el mundo de la programación se habla sobre todo de aplicaciones que requieren de un gran número de módulos que normalmente van a integrar diferentes apartados de una misma empresa, nos van a permitir gestionar todo el flujo de trabajo de la misma, van a manejar un gran volumen de datos, etc. 

Normalmente se espera de las aplicaciones empresariales que sean complejas por todas las razones anteriores, que sean escalables, no es lo mismo diseñar una aplicación para una empresa que tiene mil clientes que para una que tiene un millón, que sea distribuida es decir que no tenga porqué recibir solamente en un único servidor, por ejemplo tener los datos alojados en un único sitio y también que sea crítica, es decir que no  tengas fallos, eso se espera de cualquiera pero que en el caso de que de que sucediera algún tipo de fallo, conectividad pueda reaccionar rápidamente.

Las aplicaciones empresariales se suelen desplegar tanto en internet como en ocasiones también dentro de redes corporativas, suelen estar centradas entorno a los datos, los datos suelen ser la parte nuclear, el manejo de la información. Requiere de que sea intuitiva y de un uso fácil sobre todo para evitar el rechazo de trabajadores de empresa, en definitiva los usuarios y también que tenga unos ciertos requisitos de seguridad y de mantenibilidad, porque como decía son aplicacionen normalmente grandes, algunas de ellas requieren de adaptaciones a lo largo del tiempo, pensemos en aquellas que tienes que tener algún tipo de adaptación a la ley, que van a lo largo de su vida útil van variando y tienen que ser mantenibles y extensibles, podremos decir que esta sería la radiografía de una aplicación empresarial.

<img src="images/4-04.png">

Algunos autores dividen una aplicación empresarial en varios bloques elementales, en particular en cinco para poder definir un sistema de información en general y las aplicación empresariales en particular cómo serían:

* Personas
   * Propietarios del sistema
   * Usuarios
   * Diseñadores
   * Desarrolladores
   * ...
* Datos: Materia prima usada por el sistema de información, permiten
   * Crear información útil
   * Toma de desiciones
   * etc.
* Procesos: Todas aquellas actividades que se llevan dentro de la empresa 
   * Procesan datos
   * Generan información
* Redes: Aquellos canales que vamos a utilizar y diferenciar a la hora de desplegar y utilizar la aplicación
   * Centros de produción
   * Oficinas
   * Delegaciones
   * etc.
* Tecnología que soporta nuestro sistema de información
   * Hardware
   * Software

Estos serían de alguna manera los cinco bloques que la conformarían.

<img src="images/4-05.png">

A nivel de capas una aplicación empresarial esta formada por:

* Infraestructura del hardware
   * En que servidor va a recidir nuestra aplicación
   * Que tipo de conectividad tenemos para acceder a el
   * Plataforma que hay sobre ese hardware
* Infraestructura de la plataforma
   * Sistema operativo, versión de maquinas virtuales, versiones del software, servidores, etc.
* Datos
   * Relacional o no relacional
* Aplicación
* Interfaz de usuario

**Spring trabaja sobre todo el nivel de aplicación y con sus fronteras**, interfaz de usuario y acceso a datos.

<img src="images/4-06.png">

Aquí podemos verlo desglosado en aquellos componentes que seran habituales en Spring.

### Datos

Para acceder a la capa de Datos existen varias formas de hacerlo, existen dos grandes tipos:

* Relacionales MySQL, Oracle
* NoSQL, no solo son SQL por ejemplo MongoDB.

En la frontera ente la capa de *Datos* y *Aplicación* vamos a tener unas clases especiales lladados ***Repositorios*** que nos permiten acceder a los datos desde la *Aplicación*.

### Aplicación

Dentro de la aplicación podemos dividirlos en tres grandes tipos:

* **Entidades** modelan el dominio de nuestra aplicación.
* **Servicios** son los que realmente trabajan, tienen la lógica de negocio de nuestra aplicación 
* **Controladores** son los que reciben las peticiones y son capaces de despacharlas, delegando el trabajo en los servicios que sean necesarios.

### Interfaz de Usuario

Los *sistemas de plantillas* conectan la *Aplicación* y la *Interfaz de Usuario*, nuestra parte visual.

<img src="images/4-07.png">

<img src="images/4-08.png">

Pasemos ahora hablar de **patrones de diseño**, un patrón de diseño lo vamos a poder definir a partir de esta idea, cuando nos encontramos antes el análisis y diseño de una aplicación podemos encontrarnos con algunos problemas, cuando nos encontramos con un problema nos podríamos hacer la pregunta ¿seguro que esto no le ha pasado a nadie más? probablemente la respuesta es que si le haya pasado alguien más, de hecho en un alto porcentaje de situaciones alguien ya tuvo ese problema y lo resolvió, de hecho nos podríamos plantear sí esa solución que esa persona aplico la podríamos adaptar a nuestra versión del problema, pues bien podríamos decir que **un patrón de diseño es una solución a un problema dado que ya ha sido utilizada, cuya efectividad ha sido probada y que además será reutilizable en problemas que tengan circunstancias similares**, los patrones de diseño los podemos pensar en el caso del software y en otros ámbitos científico como podría ser la arquitectura, cómo podemos salvar un tipo de terreno que tengo una pendiente, ya alguien se inventó que la idea podría ser un puente o un túnel, pues nosotros podemos aplicar algo por el estilo al mundo del software.

<img src="images/4-09.png">

En los años 90 cuatro autores llamados la banda de los cuatro se plantearon sobre todo, sobre C++ en plantear una serie de patrones de diseño, son veintitantos patrones 23, 24 patrones que son altamente reutilizable y aplicables en múltiples situaciones. Los podríamos estructurar si su ámbito al cual afectan a una clase en  general o a un objeto en particular y según su propósito en patrones creacionales, estructurales o de comportamiento. Lo dejo por aquí porque es posible que a la hora de desarrollar aplicaciones empresariales o si hay algo de literatura haga referencia a alguno de estos patrones y también por que de aquí en adelante en este propio curso podemos nombrar alguno de ellos. Algunos de ellos los podremos usar con Spring por eso esta bien que nos vayan sonando.

<img src="images/4-10.png">

Otros autores proponen otros patrones como el de la **Inversión de Control** de ese sí hablaremos en una lección posterior porque **es la parte nuclear de Spring**, la inversión de control a través de la inyección de dependencias, patrones como el Modelo Vista Controlador MVC, el View Helper, el patrón DAO Objeto de Acceso a Datos, Front-Controller son otros que también nos pueden sonar.

<img src="images/4-11.png">

**¿Qué patrones utiliza Spring? principalmente el patrón de la Inyección de Dependencia**.

Otros patrones que utilizamos en el en los ámbitos de un *Bean* serían *el patrón Singleton* o *Prototype*,  **el patrón Singleton es aquel que nos va a permitir que hagamos** una restricción sobre la creación de objetos de un tipo para que tengamos de una clase particula,r de un tipo particular **un solo objeto**. Frente a Singleton no tenemos **Prototype que nos va a permitir la creación de nuevos objetos duplicándolos**, ambos los veremos como la posibilidad de definir un ámbito de un Bean.

**El patrón Proxy que nos va a permitir definir un representante de un objeto para controlar el acceso a este**, la palabra Proxy es posible que la conozcamos, nos suene por la navegación a Internet y de interponer una máquina en nuestra navegación, pues algo parecido podemos hacer a nivel de objeto y de hecho se utiliza en la programación orientada a aspectos.

<img src="images/4-12.png">

Otros patrones cómo el **patrón Modelo Vista Controlador que nos va a permitir separar la lógica y los datos de la interfaz y del control de peticiones** o **el patrón FrontController que nos va a permitir tener un solo controlador de fachada** que está al frente de la aplicación para manejar todas las peticiones, todo ellos lo utiliza Spring Web MVC. El **patrón Factory que centraliza la construcción de objetos lo utilizaría el Contenedor de Inversión de Control de Spring**.

Ya podéis comprobar como algunos de estos patrones son los utilizados, no nos podemos parar a conocerlo en profundidad pero si es verdad que estaría bien que no suenen para motivar el porqué Spring realiza así las cosas y también el cómo diseñar nuestras aplicaciones empresariales.

# 05 Inversión de control e inyección de dependencias 15:09 

[PDF 1-5_Inversion_de_Control_e_Inyeccion_de_Dependencias.pdf](pdfs/1-5_Inversion_de_Control_e_Inyeccion_de_Dependencias.pdf)

## Transcripción

<img src="images/5-01.png">

Hola todos vamos a continuar hablando sobre el meollo de la cuestión, es decir **la inversión de control** y **la inyección de dependencia**, dos conceptos que en ocaciones pueden ocacionar un quebradero de cabeza y que esperemos que con esta explicación queden aclarados.

<img src="images/5-02.png">

<img src="images/5-03.png">

Primero hablemos de la **Inversión de Control IoC**, la Inversión de Control IoC es un principio de diseño incluso lo podríamos llamar patrón aunque no haya entrado dentro del lote de los patrones de la banda de los cuatro. **El objetivo de la Inversión de Control principal es desacoplar objetos**, es decir que dos objetos pueda colaborar entre si pero que la interferencia que el uno haga sobre el otro seá la mínima de forma que si en un futuro tenemos que refactorizar, es decir cambiar alguno de esos objetos o al menos como funcionan por dentro no tengamos que cambiar ambos, sino refactorizar solo uno. 

Podríamos dar un ejemplo de un sistema en el cual la lógica de negocio es decir los procesos, los datos que se manejan no cambian a lo largo del tiempo pero su interfaz gráfica sí que lo ha hecho, pensado en un sistema que comenzó trabajando desde la línea de comando, una aplicación de las clásicas de escritorio antiguas que utilizaban en los 80 y los 90, pasando de allí a un sistema de ventanas primigenio por ejemplo con Java o algún sistema como el que tenemos en pantalla, Motif o alguno parecido y que de ahí pasara a un tipo de interfaz web, nos interesaría que solo tuviéramos que cambiar la parte de la interfaz y lo mínimo necesario para que nuestra lógica de negocio no se viera alterada, con eso estaríamos hablando de desacoplamiento. Bueno pues la inversión de control es un principio que tiene como objetivo conseguir que los objetos estén poco acoplados. 

*Desacoplar objetos* quiere decir que dos objetos puedan colaborar entre si, pero que la interferencia que uno haga sobre el otro sea la mínima, de forma que si en un futuro tenemos que refactorizar, es decir cambiar alguno de esos objetos en su funcionamiento, **NO TENGAMOS QUE CAMBIAR AMBOS**, solo refactorizar uno.

En la imagen vemos un ejemplo de un sistema donde los datos no cambian, pero la interfaz de usuario si que lo ha hecho. Nos interesaria que solo tuvieramos que cambiar la parte de la interfaz gráfica y que nuestra lógica de negocio no se viera alterada. Con esto estamos hablando de **DESACOPLAMIENTO**.

***La Inversion de Control es un pricipio que tiene como objetivo que los objetos esten poco acoplados***

<img src="images/5-04.png">

Y que se rige por un principio llamado el principio de Hollywood en el que bueno podríamos decir "No nos llames. Nosotros te llamaremos a ti" qué es una frase que se acuño en Hollywood cuando algunos actores hacían las pruebas y los productores directores y los actores se interesaban ellos decían "buenos llamaré para ver qué tal ha salido la prueba para ver si me va a contratar o no" y los productores y directores decian "No nos llames. Nosotros te llamaremos a ti", pues la inversión de control se va a regir mediante este principio.

La Inversion de Control se rige bajo este principio de Hollywood.

<img src="images/5-05.png">

El concepto de Inversión de Contro (IoC) lo acuño Martin Fowler. La idea es dejar que sea otro el que controle el flujo del programa, por ejemplo un Framework será lo más usual.

### Diferencia entre Librería y Framework

Una librería es un conjuto de algoritmos, de utilidades que ayudan a realizar algunas operaciones por ejemplo una librería matématica que ayudan a realizar todo tipo de calculos matématicos. Sin embargo un Framework ademas de proporcionar un conjunto de algoritmos y de utilidades suele proporcionar **una metodología de trabajo, una forma de trabajar y una estructura en donde nosotros vamos a encajar nuestro código** 

Veamos estos dos mini programas que están escritos en Rubí y están propuestos por Martín Fowler ya digo que es un poco el padre de la inversión de control propuesta como tal, el progrma de la izquierda sería un programa de la línea de comandos en la cual nosotros como programadores vamos indicando exactamente los pasos que sigue el programa de forma imperativa, mostramos un mensaje, esperamos que el usuario introduzca un nombre por teclado, lo procesamos, cuando a terminado de procesarse pedimos otro dato por consola, esperamos que el usuario escriba otra cadena por teclado y la procesamos. Sin embargo en el código de la derecha realmente nosotros perdemos el control por que sería el Framework TK el que se encargaríam, si nos fijamos en el último método `Tk.mainloop()` es un método que se va a encargar de ejecutar un código por detrás, de hecho es una especie de bucle infinito en el que está esperando a que nosotros interactuemos con el programa, si lo vemos las dos líneas que están señaladas con una flecha, podemos ver como el procesamiento del nombre o del resto de cadenas se realiza cuando dejamos el foco sobre un campo, en este caso sería un campo de entrada de texto name y de alguna manera nosotros no estamos invocando directamente de forma imperativa el procesamiento del nombre o el procesamiento de la cadena quest, sino que sería asociado a un evento y cuándo surga ese evento será el Framework el que se encargue de invocar nuestro código, con lo cual nosotros hemos perdido la digamos que, esa dirección imperativa de cuándo va a suceder la ejecución del código, delegandola en el Framework.

<img src="images/5-06.png">

Dos autores como son Ralph Johnson y Brian Foote en una revista de programación orientado a objetos ya en el año 88, proponían este principio y es que una característica importante de un framework es que los métodos definidos por el usuario para adaptar el mismo a menudo serán en lugar de invocados por el código del programador serán invocados por el propio framework, el framework por tanto desempeña el papel de programa principal a la hora de coordinar y secuenciar qué pasos sigue la aplicación, la Inversión de Control es decir delegar el control en el framework proporciona al mismo la posibilidad de servir como una especie de esqueleto extensible donde nosotros vamos a proporcionar métodos que van a adaptar algoritmos genéricos que nos proporciona el propio framework, sería un primer acercamiento de una manera más profunda a lo que sería el principio de inversión de control.

**Nosostros perdemos ese control, lo delegamos en un Framework y es ese Framework el que va a llevar, va a orquestar los pasos que se tienen que seguir**. Nosotros solo incrustamos ciertos bloques de código que el Framework ejecutara en cierto momento.

<img src="images/5-07.png">

Algunos ejemplos de control los tenemos en la diapositiva como la suscripción o manejo de eventos que tenemos en .Net o en Java, por ejemplo podríamos pensar en eventos sobre widget gráfico como pulsar un botón, que sucede cuando se pulsa el botón, nosotros decimos que sucedería cuando se pulse el botón, pero no controlamos imperativamente el momento en el que va a suceder. Los beans de sesión con sus métodos asociados al ciclo de vida, que sucede cuando se activa, cuando pasa a un almacenamiento secundario, cuando se rescata del almacenamiento secundario, nosotros podríamos decir que sucede asociado a esos eventos pero no cuando se tiene que invocar, parecido sería con los test unitarios de JUnit y otro ejemplo sería la **inyección de dependencias** es decir es solo una forma más de **Inversión de Control** es decir que inversión de control e inyección de dependencia no son dos elementos que sean equivalentes, dos conceptos que son estrictamente equivalentes sino que la inyección de dependencia es una forma de inversión de control.

**La Inyección de Dependencias es una forma de Inversión de Control** 

<img src="images/5-08.png">

<img src="images/5-09.png">

Qué es eso de la inyección de dependencia es una forma de inversión de control en la cual frente al modelo tradicional en el que bueno si un objeto necesita de otros dos o los que sean, el mismo puede invocar la construcción de los mismos, en un esquema de inversión de control con inyección de dependencia lo único que haríamos sería declarar en nuestro objeto aquellos objetos que van a ser necesarios, es decir nuestras dependencias de forma que alguien un componente exterior se encargará de proporcionarnos la referencia a los mismos, es decir que nos proporcionará esas dependencias.

Un ejemplo podría ser pulsar un botón, *qué sucede cuando se pulsa el botón*, nosostros decimos que sucede al pulsar el botón pero no controlamos imperativamente el momento en que va a suceder. 

En un *Modelo tradicional* si un objeto necesita de otros dos o *n* objetos, el mismo pude invocar la construcción de los mismos.

En un *Modelo de Inversión de Control con Inyección de Dependencias* lo único que hariamos es declar en nuestro objeto aquellos objetos que van a ser necesarios, es decir nuestras dependencias, de forma que un componente exterior se encargara de proporcionarnos las referencias a los mismos, es decir que nos proporcinara esas dependencias.

<img src="images/5-10.png">

Pensemos en un software como este, si este ejemplo también es de Martín Fowler en el qué podríamos tener la posibilidad de que listaramos una serie de películas siempre y cuando hayan sido dirigida por un director en particular, no nos centremos para nada en la eficiencia de este bloque de código que es muy baja, aquí el meollo se encuentra en el objeto `finder` o cómo podemos conectar nuestra clase `MovieLister` con este objeto `finder` en particular sería maravilloso que esta manera de listar las películas fuera independiente de cómo han sido almacenadas, de forma que nuestro método hiciera referencia a `finder` que supiéramos que `finder` lo único que tiene es un método llamado `findAll()` definido en una interfaz pero no dijera nada más sobre este objeto ni dónde están almacenadas.

*En este ejemplo de Martin Fowler podemos tener la posibilidad de listar una serie de películas siempre y cuando hayan sido dirigidas por un director en partícular. El *meolloo* se encuentra en el objeto `finder` o como podemos conectar nuestra clase `MovieLister` con el objeto `finder` en particular.* 

*Sería maravilloso que la manera de listar las películas fuera independiente de como han sido almacenadas, de forma que nuestro método hiciera referencia a `finder`, que lo único que tiene es un método llamado `findAll()`, definido en una interfaz, pero que no dijera nada más sobre este objeto ni donde estan almacenadas.*  

<img src="images/5-11.png">

Como podríamos nosotros después tener un objeto en partícular, lo tendríamos por ejemplo que crear en el constructor de `MovieLister`  donde en este caso si las películas las tenemos almacenadas en un fichero de valores separados por comas tendríamos que invocar el constructor de la clase `CSVMovieFinder("movies.txt")`, qué sucede si el día de mañana nosotros pasamos a tener la cantidad de datos que tenemos es grande y queremos utilizar una base de datos SQL o queremos estructurarlo en un fichero XML pues que tendríamos que proporcionar otra clase pero a su vez la clase `MovieLister` y la tendríamos que modificar también para cambiar este constructor, esto hace que se genera un gran acoplamiento entre la clase `MovieLister` y el `finder` en particular que nosotros vamos a utilizar.

*Esto lo hacemos creando una Interfaz donde nos comprometemos a tener el método `findAll()`. De manera que la referencia en nuestra clase `MovieLister` es de tipo `finder`.*

*¿Cómo podriamos después nosotros tener un objeto en partícular? Lo podemos crear en el constructor de `MovieLister` que lo lee de un archivo `txt`. Si en un futuro quisieramos usar una BD como MySQL o en un fichero XML. Por un lado tendríamos que crear una clase alternativa a `CSVMovieFinder` por ejemplo `BDMovieFinder` o `XMLMovieFinder` y como segundo paso tendrámos que modificar la clase `MovieLister` para cambiar este constructor.*

***Esto hace que se genere un gran acoplamiento entre la clase `MovieLister` y `finder` en particular que vamos a utilizar.***

<img src="images/5-12.png">

Como podemos comprobar en este diagrama de clases, aun que `MovieLister` ocupa la Interfaz `MovieFinder`, en definitiva tiene que crear esa clase.  

<img src="images/5-13.png">

Sin embargo si pasamos a un esquema de *Inyección de dependencias* podriamos tener otro objeto, al cual podríamos llamar `Ensamblador` o `Contenedor`. Ese `Ensamblador` es el que se encargaría de crear el objeto que implemente el interfaz `MovieFinder` y de proporcionar la referencia de ese objeto a la clase `MovieLister`, de manera nosotros solo hemos dicho que tenemos una dependencia con un objeto de tipo `MovieFinder`, pero que no dice nada de como se va a crear. De esta manera se nos estará *inyectando* esa dependencia que nosotros queremos. 

<img src="images/5-14.png">

De esa forma nuestra clase `MovieLister` se quedaría *totalmente desacoplada* de lo que sería la implementación final del acceso a datos. El código muestra una posible implementación de la clase `CSVMovieFinder` que implementaria esa Interfaz y la dependencia `MovieLister` la proporcionariamos a través de Setters, del método `setFinder()`.

<img src="images/5-15.png">

Como se orquesta esto a través de este `Ensamblador`, Spring lo hace de varias formas, una es a traves de un fichero XML que permite indicar que objetos existen, en el mundo de Spring esos objetos se llamarán *beans* y como se pueden referenciar los unos con los otros.

En el código podemos comprobar que el objeto `MovieLister` dice que tiene una propiedad llamada `finder` y hace referencia a otro objeto (`MovieFinder`) que es una clase de tipo `CSVMovieFinder`.

Si el día de mañana quisieramos implementar la clase `SQLMovieFinder` solamente tedríamos que cambiar esta inyeccción en el fichero XML, indicando que la clase es de otro tipo, darle las propiedades adecuadas y de esa manera nuestro objeto desacoplado seguira teniendo sus dependencias satisfechas y hemos podido cambiar nuestro código de una manera bastante menos costosa y elegante. De esta forma hemos podido hacer la Inyección de Dependencia a un nivel teórico de ahora en adelante pasaremos a ver ejemplo más prácticos de cómo realizar

# Contenido adicional  5

* [PDF 1-1_Presentacion_del_curso.pdf](pdfs/1-1_Presentacion_del_curso.pdf)
* [PDF 1-2_Introduccion_a_Spring.pdf](pdfs/1-2_Introduccion_a_Spring.pdf)
* [PDF 1-3_Instalacion_del_entorno.pdf](pdfs/1-3_Instalacion_del_entorno.pdf)
* [PDF 1-4_Estructura_y_patrones.pdf](pdfs/1-4_Estructura_y_patrones.pdf)
* [PDF 1-5_Inversion_de_Control_e_Inyeccion_de_Dependencias.pdf](pdfs/1-5_Inversion_de_Control_e_Inyeccion_de_Dependencias.pdf)
