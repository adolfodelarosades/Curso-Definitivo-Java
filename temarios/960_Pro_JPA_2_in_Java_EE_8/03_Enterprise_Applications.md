### Chapter 3: Enterprise Applications

* Application Component Models
* Session Beans
   * Stateless Session Beans
   * Stateful Session Beans
   * Singleton Session Beans
* Servlets
* Dependency Management and CDI
   * Dependency Lookup
   * Dependency Injection
   * Declaring Dependencies
* CDI and Contextual Injection
   * CDI Beans
   * Injection and Resolution
   * Scopes and Contexts
   * Qualified Injection
   * Producer Methods and Fields
   * Using Producer Methods with JPA Resources
* Transaction Management
   * Transaction Review
   * Enterprise Transactions in Java
* Putting It All Together
   * Defining the Component
   * Defining the User Interface
   * Packaging It Up
* Summary

No existe tecnología en el vacío, y JPA no es diferente en este sentido. Aunque el estilo de aplicación de cliente pesado que se demostró en el capítulo anterior es un uso viable de JPA, la mayoría de las aplicaciones Java empresariales se implementan en un servidor de aplicaciones, normalmente utilizando tecnologías web Java EE y posiblemente también otras tecnologías. Por lo tanto, es esencial comprender los componentes que componen una aplicación implementada y el papel de JPA en este entorno.

Este libro trata sobre JPA en Java EE 8, cuyas principales características nuevas son compatibles con HTML5, el estándar HTTP/2 y la integración de beans, y una infraestructura mejorada para aplicaciones que se ejecutan en la nube.

Comenzamos con una descripción general de las principales tecnologías Java EE relevantes para la persistencia. Como parte de esta descripción general, describimos el modelo de componente EJB, demostrando la sintaxis básica para algunos de los diferentes tipos de EJB.

Luego pasaremos a cubrir brevemente el mecanismo estándar de inyección de dependencia (DI), principalmente utilizando el enfoque de Contextos e Inyección de dependencia (CDI) de Java EE. Este capítulo no pretende ser una exploración completa o detallada de Java EE o marcos de componentes, y posiblemente no podamos entrar en todos los marcos de DI en la esfera de DI, o incluso en las facilidades que ofrece CDI. Sin embargo, los ejemplos de CDI y EJB son bastante típicos de DI en general y deberían dar a los lectores una idea general de cómo se puede usar JPA con componentes habilitados para DI, ya sean de la variedad Java EE o algún otro componente de contenedor DI, como Spring o Guice.

A continuación, analizamos las transacciones, otra tecnología de servidor de aplicaciones que ha tenido un gran impacto en las aplicaciones que utilizan JPA. Las transacciones son un elemento fundamental de cualquier aplicación empresarial que necesite garantizar la integridad de los datos.

Finalmente, explicamos cómo utilizar las tecnologías descritas en este capítulo dentro del contexto de cómo la persistencia se integra en cada tecnología componente. También revisamos la aplicación Java SE del capítulo anterior y la redirigimos a la plataforma empresarial.

Aquí hay una lista de los cambios más importantes en Java EE 8:

* Java EE 8 platform

* JSON-B 1.0

* JSON-P 1.1

* JAX-RS 2.1

* MVC 1.0

* Java Servlet 4.0

* JSF 2.3

* JMS 2.1

* CDI 2.0

* Java EE Security 1.0

* Java EE Management 2.0

* Concurrency utilities

* Connector architecture

* WebSocket

* JPA

* EJB

* JTA

* JCache

* JavaMail

La figura 3-1 muestra los componentes principales de Java EE 8.

![03-01](images/03-01.png)

La figura 3-2 muestra el Java EE 8 stack.

![03-02](images/03-02.png)


## Application Component Models (Modelos de Componentes de Aplicación)

La palabra "component" ha adquirido muchos significados en el desarrollo de software, así que comencemos con una definición. Un componente es una unidad de software autónoma y reutilizable que se puede integrar en una aplicación. Los clientes interactúan con los componentes a través de un contrato bien definido. En Java, la forma más simple de componente de software es ***JavaBean***, comúnmente conocido como solo un **bean**. Los beans son componentes implementados en términos de una sola clase cuyo contrato está definido por los patrones de denominación de los métodos en el bean. Los patrones de nomenclatura de JavaBean son tan comunes ahora que es fácil olvidar que originalmente estaban destinados a brindar a los constructores de interfaces de usuario una forma estándar de tratar con componentes de terceros.

En el espacio empresarial, los componentes se enfocan más en implementar servicios comerciales, con el contrato del componente definido en términos de las operaciones comerciales que puede llevar a cabo ese componente. El modelo de componente tradicional para Java EE siempre ha sido el modelo EJB, que define formas de empaquetar, implementar e interactuar con servicios empresariales autónomos. Luego apareció CDI y trajo un modelo más poderoso y flexible del componente de bean administrado, con beans de CDI que son clases de Java EJB o no EJB.

La elección de utilizar un modelo de componente en su aplicación es en gran medida una preferencia personal, pero generalmente es una buena elección de diseño. El uso de componentes requiere organizar la aplicación en capas, con los servicios empresariales que viven en el modelo de componentes y los servicios de presentación superpuestos.

Históricamente, uno de los desafíos en la adopción de componentes en Java EE fue la complejidad de implementarlos. Con ese problema resuelto en gran medida, nos quedamos con los siguientes beneficios que un conjunto bien definido de servicios comerciales aporta a una aplicación:

* ***Loose coupling*** (Acoplamiento flexible): El uso de componentes para implementar servicios fomenta el acoplamiento flexible entre las capas de una aplicación. La implementación de un componente puede cambiar sin ningún impacto para los clientes u otros componentes que dependen de él.

* ***Dependency management*** (Gestión de dependencias): Las dependencias de un componente se pueden declarar en metadatos y el contenedor las puede resolver automáticamente.

* ***Lifecycle management*** (Gestión del ciclo de vida): El ciclo de vida de los componentes está bien definido y gestionado por el servidor de aplicaciones. Las implementaciones de componentes pueden participar en las operaciones del ciclo de vida para adquirir y liberar recursos, o realizar otro comportamiento de inicialización y apagado.

* ***Declarative container services*** (Servicios de contenedor declarativos): El servidor de aplicaciones intercepta los métodos comerciales de los componentes para aplicar servicios como la concurrencia, la gestión de transacciones, la seguridad y la comunicación remota.

* ***Portability*** (Portabilidad): Los componentes que cumplen con los estándares Java EE y que se implementan en servidores basados en estándares se pueden transferir más fácilmente de un servidor compatible a otro.

* ***Scalability and reliability*** (Escalabilidad y confiabilidad): Los servidores de aplicaciones están diseñados para garantizar que los componentes se administren de manera eficiente con miras a la escalabilidad. Según el tipo de componente y la configuración del servidor, las operaciones comerciales implementadas mediante componentes pueden reintentar llamadas de método fallidas o incluso conmutar por error a otro servidor en un clúster.

A medida que lea este libro, notará que, en algunos casos, un ejemplo utilizará un componente para albergar la lógica empresarial e invocará la API de persistencia de Java. En muchos casos será un bean de sesión, en otros casos será un bean CDI que no es EJB, y en otros será un bean de sesión gestionado por CDI (bean de sesión con alcance). Los beans de sesión son el componente preferido porque son los más sencillos de escribir y configurar y se adaptan naturalmente a la interacción con JPA. El tipo de componente real que utilizamos es menos importante (el tipo de componente es en gran medida sustituible siempre que sea gestionado por un contenedor que admita JPA y transacciones) que ilustrar cómo los componentes encajan en el JPA y pueden invocarlo.

La Figura 3-3 muestra el modelo de componentes de la aplicación Java.

![03-03](images/03-03.png)

## STATELESS SESSION BEANS

Los beans de sesión son una tecnología de componentes diseñada para encapsular servicios empresariales. Las operaciones accesibles al cliente soportadas por el servicio pueden definirse usando una interfaz Java, o en ausencia de una interfaz, solo el conjunto de métodos públicos en la clase de implementación del bean. La clase bean es poco más que una clase Java normal y, sin embargo, en virtud de ser parte del modelo de componente EJB, el bean tiene acceso a una amplia gama de servicios de contenedor. El significado del nombre "session bean" tiene que ver con la forma en que los clientes acceden e interactúan con ellos. Una vez que un cliente adquiere una referencia a un bean de sesión del servidor, inicia una sesión con ese bean y puede invocar operaciones comerciales en él.

**Hay tres tipos de beans de sesión: sin *estado(stateless)*, con *estado(stateful)* y *singleton***. La interacción con un bean de sesión sin estado comienza al inicio de una llamada a un método empresarial y finaliza cuando finaliza la llamada al método. No hay ningún estado que se transfiera de una operación comercial a otra. Una interacción con beans de sesión con estado se convierte más en una conversación que comienza desde el momento en que el cliente adquiere una referencia al bean de sesión y finaliza cuando el cliente lo devuelve explícitamente al servidor. Las operaciones comerciales en un bean de sesión con estado pueden mantener el estado en la instancia del bean en todas las llamadas. Proporcionamos más detalles sobre las consideraciones de implementación de esta diferencia en el estilo de interacción a medida que describimos cada tipo de bean de sesión.

***Los beans de sesión singleton*** pueden considerarse un híbrido de beans de sesión sin estado y con estado. Todos los clientes comparten la misma instancia de bean singleton, por lo que es posible compartir el estado entre las invocaciones de métodos, pero los beans de sesión singleton carecen del contrato conversacional y la movilidad de los beans de sesión con estado. El estado de un bean de sesión singleton también plantea problemas de simultaneidad que deben tenerse en cuenta al decidir si utilizar este estilo de bean de sesión.

Como ocurre con la mayoría de los contenedores de componentes, los clientes de un contenedor EJB no interactúan directamente con una instancia de bean de sesión. El cliente hace referencia e invoca una implementación de la interfaz comercial o la clase de bean proporcionada por el servidor. Esta clase de implementación actúa como un proxy para la implementación del bean subyacente. Este desacoplamiento del cliente del bean permite que el servidor intercepte las llamadas a métodos para proporcionar los servicios requeridos por el bean, como la gestión de transacciones. También permite que el servidor optimice y reutilice instancias de la clase de bean de sesión según sea necesario.

En las siguientes secciones, analizamos los beans de sesión que utilizan invocaciones de métodos de negocio sincrónicos. Los métodos comerciales asincrónicos ofrecen un patrón de invocación alternativo que involucra futuros, pero están más allá del alcance de este libro.

### STATELESS SESSION BEANS

Como se mencionó, un bean de sesión sin estado se propone completar una operación dentro de la vida útil de un solo método. Los beans sin estado pueden implementar muchas operaciones comerciales, pero cada método no puede asumir que se invocó otro antes.

Esto puede parecer una limitación del bean sin estado, pero es, con mucho, la forma más común de implementación de servicios empresariales. A diferencia de los beans de sesión con estado, que son buenos para acumular estado durante una conversación (como el carro de la compra de una aplicación minorista), los beans de sesión sin estado están diseñados para realizar operaciones independientes de manera muy eficiente. Los beans de sesión sin estado pueden escalar a una gran cantidad de clientes con un impacto mínimo en los recursos generales del servidor.

#### ***Definición de un Stateless Session Bean (Bean de Sesión sin Estado)***

Un bean de sesión se define en dos partes:

* Cero o más interfaces comerciales que definan qué métodos puede invocar un cliente en el bean. Cuando no se define ninguna interfaz, el conjunto de métodos públicos en la clase de implementación del bean forma una interfaz lógica de cliente.

* Una clase que implementa estas interfaces, llamada clase bean, que está marcada con la anotación `@Stateless`.

Si desea hacer frente a su bean de sesión con una interfaz real o no, es una cuestión de preferencia. Mostramos ejemplos de ambos, pero generalmente no usamos la interfaz en ejemplos posteriores.

Veamos primero una versión interconectada de un bean de sesión sin estado. El Listado 3-1 muestra la interfaz empresarial que es compatible con este bean de sesión. En este ejemplo, el servicio consta de un solo método, `sayHello()`, que acepta un argumento `String` correspondiente al nombre de una persona y devuelve una respuesta `String`. No hay ninguna anotación o interfaz principal que indique que se trata de una interfaz empresarial. Cuando se implementa mediante el bean de sesión, se tratará automáticamente como una interfaz comercial local, lo que significa que solo es accesible para los clientes dentro del mismo servidor de aplicaciones. También es posible un segundo tipo de interfaz empresarial para clientes remotos, pero no se utiliza con frecuencia.

***Listado 3-1*** La Interfaz Empresarial para un Session Bean (Bean de Sesión)

```java
public interface HelloService {
   public String sayHello(String name);
}
```

Ahora consideremos la implementación, que se muestra en el Listado 3-2. Esta es una clase Java normal que implementa la interfaz empresarial `HelloService`. Lo único que es único sobre esta clase es la anotación `@Stateless` <sup>1</sup> que la marca como un bean de sesión sin estado. El método comercial se implementa sin restricciones o requisitos especiales. Esta es una clase normal que resulta ser un EJB.

***Listado 3-2*** La clase Bean que implementa la interfaz `HelloService`

```java
@Stateless
public class HelloServiceBean implements HelloService {
   public String sayHello(String name) {
      return "Hello, "  + name;
   }
}
```

En términos de diseño de API, usar una interfaz es probablemente la mejor manera de exponer las operaciones de un bean de sesión, ya que separa la interfaz de la implementación. Sin embargo, la norma actual es implementar componentes como clases simples que contienen la lógica empresarial sin una interfaz. Usando este enfoque, el bean de sesión sería simplemente como se muestra en el Listado 3-3.


***Listado 3-3*** Un Session Bean sin Interfaz

```java
@Stateless
public class HelloService {
   public String sayHello(String name) {
      return “Hello, “ + name;
   }
}
```

La interfaz lógica del bean de sesión consta de sus métodos públicos; en este caso, el método `sayHello()`. Los clientes usan la clase `HelloServiceBean` como si fuera una interfaz y deben ignorar cualquier método no público o detalles de la implementación. Bajo las sábanas, el cliente interactuará con un proxy que extiende la clase de bean y anula los métodos comerciales para proporcionar los servicios de contenedor estándar.

#### ***Lifecycle Callbacks***

A diferencia de una clase Java normal utilizada en el código de la aplicación, el servidor gestiona el ciclo de vida de un bean de sesión sin estado. El servidor decide cuándo crear y eliminar instancias de bean y tiene que inicializar los servicios para una instancia de bean después de que se construya, pero antes de que se invoque la lógica empresarial del bean. Del mismo modo, es posible que el bean tenga que adquirir un recurso, como una fuente de datos JDBC, antes de poder utilizar los métodos comerciales. Sin embargo, para que el bean adquiera un recurso, el servidor primero debe haber completado la inicialización de sus servicios para el bean. Esto limita la utilidad del constructor para la clase porque el bean no tendrá acceso a ningún recurso hasta que se complete la inicialización del servidor.

Para permitir que tanto el servidor como el bean logren sus requisitos de inicialización, los EJB admiten métodos de devolución de llamada del ciclo de vida que el servidor invoca en varios puntos del ciclo de vida del bean. Para los beans de sesión sin estado, hay dos devoluciones de llamada de ciclo de vida: `PostConstruct` y `PreDestroy`. El servidor invocará la devolución de llamada de `PostConstruct` tan pronto como haya completado la inicialización de todos los servicios de contenedor para el bean. En efecto, esto reemplaza al constructor como la ubicación para la lógica de inicialización porque es solo aquí donde se garantiza la disponibilidad de los servicios de contenedor. El servidor invoca la devolución de llamada `PreDestroy` inmediatamente antes de que el servidor libere la instancia del bean para que sea recolectada como basura. Cualquier recurso adquirido durante `PostConstruct` que requiera un cierre explícito debe liberarse durante `PreDestroy`.

El Listado 3-4 muestra un bean de sesión sin estado que adquiere una referencia a una instancia `java.util.logging.Logger` durante la devolución de llamada de `PostConstruct`, identificada por la anotación de marcador `@PostConstruct`. Asimismo, una devolución de llamada `PreDestroy` se identifica mediante la anotación `@PreDestroy`.


***Listado 3-4*** Uso de `PostConstruct ` Callback para Adquirir un Logger

```java
@Stateless
public class LoggerBean {
   private Logger logger;
   
   @PostConstruct
   void init() {
      logger = Logger.getLogger("notification");
   }
   public void logMessage(String message) {
      logger .info(message);
   }
}
```

### STATEFUL SESSION BEANS

En la introducción a los beans de sesión, describimos la diferencia entre beans sin estado y con estado como basada en el estilo de interacción entre cliente y servidor. En el caso de los beans de sesión sin estado, esa interacción comenzó y terminó con una única llamada al método. A veces, los clientes necesitan emitir varias solicitudes y que cada solicitud pueda acceder o considerar los resultados de solicitudes anteriores. Los beans de sesión con estado están diseñados para manejar este escenario proporcionando un servicio dedicado a un cliente que comienza cuando el cliente obtiene una referencia al bean y finaliza solo cuando el cliente elige terminar la conversación.

El ejemplo por excelencia del bean de sesión con estado es el carrito de compras de una aplicación de comercio electrónico. El cliente obtiene una referencia al carrito de la compra, iniciando la conversación. Durante el lapso de la sesión del usuario, el cliente agrega o elimina artículos del carrito de compras, que mantiene el estado específico del cliente. Luego, cuando finaliza la sesión, el cliente completa la compra, provocando que se retire el carrito de la compra.

Esto no es diferente al uso de un objeto Java no administrado en el código de la aplicación. Usted crea una instancia, invoca operaciones en el objeto que acumulan estado y luego desecha el objeto cuando ya no lo necesita. La única diferencia con el bean de sesión con estado es que el servidor administra la instancia del objeto real y el cliente interactúa con esa instancia indirectamente a través de un objeto proxy.

Los beans de sesión con estado ofrecen un superconjunto de la funcionalidad disponible en los beans de sesión sin estado. Las características que cubrimos para los beans de sesión sin estado se aplican igualmente a los beans de sesión con estado.

#### ***Definición de un Stateful Session Bean (Bean de Sesión con Estado)***

Ahora que hemos establecido el caso de uso para un bean de sesión con estado, veamos cómo definir uno. De manera similar al bean de sesión sin estado, un bean de sesión con estado puede tener o no una interfaz implementada por una sola clase de bean. El Listado 3-5 muestra la clase de bean para el bean de sesión con estado `ShoppingCart`. La clase de bean se ha marcado con la anotación `@Stateful` para indicar al servidor que la clase es un bean de sesión con estado.

***Listado 3-5*** Implementación de un Carrito de la Compra mediante un Stateful Session Bean (Bean de Sesión con Estado)

```java
@Stateful
public class ShoppingCart {
   
   private HashMap<String,Integer> items = new HashMap<String,Integer>();
   
   public void addItem(String item, int quantity) {
      Integer orderQuantity = items.get(item);
      if (orderQuantity == null) {
         orderQuantity = 0;
      }
      orderQuantity += quantity;
      items.put(item, orderQuantity);
   }
    
   public void removeItem(String item, int quantity) {
      // ...
   }
   
   public Map<String,Integer> getItems() {
      // ...
   }
   
   // ...
   @Remove
   public void checkout(int paymentId) {
      // store items to database
      // ...
   }
    
   @Remove
   public void cancel() {
   }
}
```


Hay dos cosas diferentes en este bean en comparación con los beans de sesión sin estado con los que hemos estado tratando hasta ahora.

La primera diferencia es que la clase de bean tiene campos de estado que son modificados por los métodos comerciales del bean. Esto está permitido porque el cliente que utiliza el bean de manera efectiva tiene acceso a una instancia privada del bean de sesión en la que realizar cambios.

La segunda diferencia es que hay métodos marcados con la anotación `@Remove`. Estos son los métodos que utilizará el cliente para finalizar la conversación con el bean. Después de que se haya llamado a uno de estos métodos, el servidor destruirá la instancia del bean y la referencia del cliente lanzará una excepción si se hace algún intento adicional para invocar métodos comerciales. Cada bean de sesión con estado debe definir al menos un método marcado con la anotación `@Remove`, incluso si el método no hace nada más que servir como final de la conversación. En el Listado 3-5, se llama al método `checkout()` si el usuario completa la transacción de compra, aunque se llama a `cancel()` si el usuario decide no continuar. El bean de sesión se elimina en cualquier caso.

#### ***Lifecycle Callbacks***

Al igual que el bean de sesión sin estado, el bean de sesión con estado también admite devoluciones de llamada del ciclo de vida para facilitar la inicialización y limpieza del bean. También admite dos devoluciones de llamada adicionales para permitir que el bean maneje con gracia la pasivación y activación de la instancia del bean. ***La pasivación es el proceso mediante el cual el servidor serializa la instancia de bean para que pueda almacenarse fuera de línea para liberar recursos o replicarse en otro servidor en un clúster***. ***La activación es el proceso de deserialización de una instancia de bean de sesión pasivada y volverla a activar en el servidor***. Debido a que los beans de sesión con estado mantienen el estado en nombre de un cliente y no se eliminan hasta que el cliente invoca uno de los métodos de eliminación en el bean, el servidor no puede destruir una instancia de bean para liberar recursos. La pasivación permite que el servidor recupere recursos temporalmente mientras conserva el estado de la sesión.

Antes de que un bean sea pasivado, el servidor invocará la devolución de llamada `PrePassivate`. El bean usa esta devolución de llamada para preparar el bean para la serialización, generalmente cerrando cualquier conexión activa a otros recursos del servidor. El método `PrePassivate` se identifica mediante la anotación de marcador `@PrePassivate`. Después de que se haya activado un bean, el servidor invocará la devolución de llamada `PostActivate`. Con la instancia serializada restaurada, el bean debe volver a adquirir cualquier conexión a otros recursos de los que dependan los métodos comerciales del bean. El método `PostActivate` se identifica mediante la anotación de marcador `@PostActivate`.

El Listado 3-6 muestra un bean de sesión que usa las devoluciones de llamada del ciclo de vida para mantener una conexión JDBC `Connection`. Tenga en cuenta que solo la conexión JDBC se gestiona explícitamente. Como fábrica de conexiones de recursos, el servidor guarda y restaura automáticamente la fuente de datos durante la pasivación y activación.

***Listado 3-6*** Uso de Lifecycle Callbacks en un Stateful Session Bean

```java
@Stateful
public class OrderBrowser {
   
   DataSource ds;
   Connection conn;
    
   @PostConstruct
   void init() {
      // acquire the data source
      // ...
      acquireConnection();
   }

   @PrePassivate
   void passivate() { releaseConnection(); }
   
   @PostActivate
   void activate() { acquireConnection(); }
    
   @PreDestroy
   void shutdown() { releaseConnection(); }
    
   private void acquireConnection() {
      try {
         conn = ds.getConnection();
      } catch (SQLException e) {
         throw new EJBException(e);
      }
   }
    
   private void releaseConnection() {
      try {
         conn.close();
      } catch (SQLException e) {
      } finally {
         conn = null;
      }
   }
    
   public Collection<Order> listOrders() {
      // ...
   }
    
   @Remove
   public void remove() {}
   
}
```

### SINGLETON SESSION BEANS

Dos de las críticas más comunes al bean de sesión sin estado han sido la sobrecarga percibida de la agrupación de beans y la incapacidad de compartir el estado a través de campos estáticos. El bean de sesión singleton intenta proporcionar una solución a ambas preocupaciones proporcionando una única instancia de bean compartida a la que se puede acceder simultáneamente y utilizar como mecanismo para el estado compartido. Los beans de sesión singleton comparten las mismas devoluciones de llamada del ciclo de vida que un bean de sesión sin estado, y los recursos gestionados por el servidor, como los contextos de persistencia, se comportan de la misma manera que si fueran parte de un bean de sesión sin estado. Pero las similitudes terminan ahí porque los beans de sesión singleton tienen un ciclo de vida general diferente al de los beans de sesión sin estado y tienen la complejidad adicional del bloqueo controlado por el desarrollador para la sincronización.

A diferencia de otros beans de sesión, se puede declarar que el singleton se crea con entusiasmo durante la inicialización de la aplicación y que existe hasta que se apaga. Una vez creado, seguirá existiendo hasta que el contenedor lo elimine, independientemente de las excepciones que se produzcan durante la ejecución del método comercial. Esta es una diferencia clave con respecto a otros tipos de bean de sesión porque la instancia de bean nunca se volverá a crear en caso de una excepción del sistema.

La larga vida y la instancia compartida del bean de sesión singleton lo convierten en el lugar ideal para almacenar el estado común de la aplicación, ya sea de solo lectura o lectura-escritura. Para proteger el acceso a este estado, el bean de sesión singleton proporciona una serie de opciones de simultaneidad en función de las necesidades del desarrollador de la aplicación. Los métodos pueden desincronizarse por completo para mejorar el rendimiento, o el contenedor los puede bloquear y administrar automáticamente.

#### ***Definición de un Singleton Session Bean***

Siguiendo el patrón de los beans de sesión sin estado y con estado, los beans de sesión singleton se definen mediante la anotación `@Singleton`. Los beans de sesión singleton pueden incluir una interfaz o utilizar una vista sin interfaz. El Listado 3-7 muestra un bean de sesión singleton simple con una vista sin interfaz para rastrear el número de visitas a un sitio web.


***Listado 3-7*** Implementación de un Singleton Session Bean

```java
@Singleton
public class HitCounter {
   int count;
   public void increment() { ++count; }
   public void getCount() { return count; }
   public void reset() { count = 0; }
}
```

Si compara el bean `HitCounter` del Listado 3-7 con los beans de sesión sin estado y con estado definidos anteriormente, puede ver dos diferencias inmediatas. A diferencia del bean de sesión sin estado, existe un estado en forma de campo de recuento que se utiliza para capturar el recuento de visitas. Pero a diferencia del bean de sesión con estado, no hay una anotación `@Remove` para identificar el método de negocio que completará la sesión.

De forma predeterminada, el contenedor administrará la sincronización de los métodos comerciales para garantizar que no se produzcan daños en los datos. En este ejemplo, eso significa que todo el acceso al bean se serializa para que solo un cliente invoque un método comercial en la instancia en cualquier momento.

El ciclo de vida del bean de sesión singleton está vinculado al ciclo de vida de la aplicación general. El contenedor determina el punto en el que se crea la instancia de singleton a menos que la clase de bean esté anotada con la anotación `@Startup` para forzar la inicialización ansiosa cuando se inicia la aplicación. El contenedor puede crear singletons que no especifican la inicialización ansiosa de manera perezosa, pero esto es específico del proveedor y no se puede asumir.

#### ***Lifecycle Callbacks***

Las devoluciones de llamada del ciclo de vida de los beans de sesión singleton son las mismas que las de los beans de sesión sin estado: `PostConstruct` y `PreDestroy`. El contenedor invocará la devolución de llamada de `PostConstruct` después de la inicialización del servidor de la instancia de bean y de la misma manera invocará la devolución de llamada de `PreDestroy` antes de eliminar la instancia de bean. La diferencia clave aquí con respecto a los beans de sesión sin estado es que `PreDestroy` se invoca solo cuando la aplicación se apaga como un todo. Por lo tanto, se llamará solo una vez, mientras que las devoluciones de llamada del ciclo de vida de los beans de sesión sin estado se invocan con frecuencia a medida que se crean y destruyen las instancias de bean.

## Servlets

Los servlets son una tecnología de componentes diseñada para satisfacer las necesidades de los desarrolladores web que necesitan responder a las solicitudes HTTP y generar contenido dinámico a cambio. Los servlets son la tecnología más antigua y popular introducida como parte de la plataforma Java EE. Son la base de tecnologías como JavaServer Pages (JSP) y la columna vertebral de marcos web como JavaServer Faces (JSF).

Aunque es posible que tenga algo de experiencia con servlets, vale la pena describir el impacto que los modelos de aplicaciones web han tenido en el desarrollo de aplicaciones empresariales. Debido a su dependencia del protocolo HTTP, la Web es inherentemente un medio sin estado. Al igual que los beans de sesión sin estado descritos anteriormente, un cliente realiza una solicitud, el servidor activa el método de servicio apropiado en el servlet y el contenido se genera y se devuelve al cliente. Cada solicitud es completamente independiente de la anterior.

Esto presenta un desafío porque muchas aplicaciones web implican algún tipo de conversación entre el cliente y el servidor en el que las acciones previas del usuario influyen en los resultados devueltos en las páginas siguientes. Para mantener ese estado de conversación, muchas aplicaciones tempranas intentaron incrustar dinámicamente información de contexto en URL. Desafortunadamente, esta técnica no solo no escala muy bien, sino que también requiere un elemento dinámico para toda la generación de contenido que dificulta que los no desarrolladores escriban contenido para una aplicación web.

Los servlets resuelven el problema del estado conversacional con la sesión. No debe confundirse con el bean de sesión, la sesión HTTP es un mapa de datos asociados con un ID de sesión. Cuando la aplicación solicita que se cree una sesión, el servidor genera un nuevo ID y devuelve un objeto `HTTPSession` que la aplicación puede usar para almacenar pares de datos key/value. Luego utiliza técnicas como las cookies del navegador para vincular el ID de sesión con el cliente, uniendo los dos en una conversación. Para las aplicaciones web, el cliente ignora en gran medida el estado conversacional que rastrea el servidor.

El uso eficaz de la sesión HTTP es un elemento importante del desarrollo de servlets. El Listado 3-8 muestra los pasos necesarios para solicitar una sesión y almacenar datos de conversación en ella. En este ejemplo, asumiendo que el usuario ha iniciado sesión, el servlet almacena el ID de usuario en la sesión, haciéndolo disponible para su uso en todas las solicitudes posteriores del mismo cliente. La llamada `getSession()` en el objeto `HttpServletRequest` devolverá la sesión activa o creará una nueva si no existe una. Una vez obtenida, la sesión actúa como un mapa, con pares key/value establecidos y recuperados con los métodos `setAttribute()` y `getAttribute()`, respectivamente. Como verá más adelante en este capítulo, la sesión del servlet, que almacena datos no estructurados, a veces se empareja con un bean de sesión con estado para administrar la información de la sesión con el beneficio de una interfaz comercial bien definida.

***Listado 3-8*** Mantenimiento del Estado Conversacional con un Servlet

```java
public class LoginServlet extends HttpServlet {
   protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
      String userId = request.getParameter("user");
      HttpSession session = request.getSession();
      session.setAttribute("user", userId);
      // ...
   }
}
```

El auge de los frameworks de aplicaciones dirigidos a la Web también ha cambiado la forma en que desarrollamos aplicaciones web. El código de aplicación escrito en servlets se está reemplazando rápidamente con código de aplicación extraído del modelo base utilizando marcos como JSF. Cuando se trabaja en un entorno como este, los problemas básicos de persistencia de la aplicación, como dónde adquirir y almacenar el administrador de la entidad y cómo utilizar las transacciones de manera eficaz y rápida, se vuelven relevantes.

Aunque exploramos algunos de estos problemas, la persistencia en el contexto de un marco como JSF está más allá del alcance de este libro. Como solución general, recomendamos adoptar un modelo de componentes en el que centrar las operaciones de persistencia. Los beans de sesión, por ejemplo, son fácilmente accesibles desde cualquier lugar dentro de una aplicación Java EE, lo que los convierte en un terreno neutral perfecto para los servicios empresariales. La capacidad de intercambiar entidades dentro y fuera del modelo de bean de sesión significa que los resultados de las operaciones de persistencia se podrán utilizar directamente en marcos web sin tener que acoplar estrechamente el código de presentación a la API de persistencia.

## Dependency Management y CDI

La lógica empresarial de un componente Java EE no suele ser completamente autónoma. La mayoría de las veces, la implementación depende de otros recursos. Esto puede incluir recursos del servidor, como una fuente de datos JDBC, o recursos definidos por la aplicación, como otro componente o administrador de entidad para una unidad de persistencia específica. La plataforma central Java EE contiene un soporte bastante limitado para inyectar dependencias en una cantidad limitada de recursos de servidor predefinidos, como fuentes de datos, transacciones administradas y otros. Sin embargo, el estándar CDI va mucho más allá de la simple inyección de dependencia (DI) y proporciona un marco extenso para soportar una amplia gama de requisitos, desde los más triviales hasta los exóticos. Comenzamos describiendo los conceptos básicos y el soporte contenido dentro de la plataforma desde antes de CDI y luego pasamos a CDI y su modelo de DI contextual.

Los componentes de Java EE admiten la noción de referencias a recursos. Una referencia es un enlace con nombre a un recurso que puede resolverse dinámicamente en tiempo de ejecución desde el código de la aplicación o resolverse automáticamente por el contenedor cuando se crea la instancia del componente. Cubrimos cada uno de estos escenarios en breve.

Una referencia consta de dos partes: un nombre y un objetivo. El código de la aplicación usa el nombre para resolver la referencia de forma dinámica, mientras que el servidor usa la información de destino para encontrar el recurso que la aplicación está buscando. El tipo de recurso que se ubicará determina el tipo de información requerida para coincidir con el objetivo. Cada referencia de recurso requiere un conjunto diferente de información específica del tipo de recurso al que se refiere.

Una referencia se declara utilizando una de las anotaciones de referencia de recursos: `@Resource`, `@EJB`, `@PersistenceContext` o `@PersistenceUnit`. Estas anotaciones se pueden colocar en una clase, campo o método de establecimiento. La elección de la ubicación determina el nombre predeterminado de la referencia y si el servidor resuelve o no la referencia automáticamente.

### DEPENDENCY LOOKUP

La primera estrategia para resolver dependencias en el código de la aplicación que discutimos se llama búsqueda de dependencias. Esta es la forma tradicional de gestión de dependencias en Java EE, en la que el código de la aplicación es responsable de buscar una referencia con nombre utilizando la Interfaz de directorio y nombres de Java (JNDI) Java Naming and Directory Interface.

Todas las anotaciones de recursos admiten un atributo llamado `name` que define el nombre JNDI de la referencia. Cuando la anotación de recurso se coloca en la definición de clase, este atributo es obligatorio. Si la anotación de recursos se coloca en un campo o en un método de establecimiento, el servidor generará un nombre predeterminado. Cuando se usa la búsqueda de dependencias, las anotaciones generalmente se colocan en el nivel de clase y el nombre se especifica explícitamente. Colocar una referencia de recurso en un campo o método de establecimiento tiene otros efectos además de generar un nombre predeterminado que discutiremos en la siguiente sección.

La función del nombre es proporcionar una forma para que el cliente resuelva la referencia de forma dinámica. Cada servidor de aplicaciones Java EE admite JNDI, aunque las aplicaciones lo utilizan con menos frecuencia desde el advenimiento de la inyección de dependencias, y cada componente Java EE tiene su propio contexto de denominación JNDI de ámbito local denominado contexto de denominación del entorno. El nombre de la referencia está vinculado al contexto de nomenclatura del entorno y, cuando se busca mediante la API JNDI, el servidor resuelve la referencia y devuelve el destino de la referencia.

Considere el bean de sesión `DeptService` que se muestra en el Listado 3-9. Ha declarado una dependencia de un bean de sesión utilizando la anotación `@EJB` y le ha dado el nombre `deptAudit`. El elemento `beanInterface` de la anotación `@EJB` hace referencia a la clase de bean de sesión. En el `PostConstruct` callback, el bean de auditoría se busca y se almacena en el campo `audit`. Las interfaces `Context` e `InitialContext` están definidas por la API JNDI. El método `lookup()` de la interfaz de contexto es la forma tradicional de recuperar objetos de un contexto JNDI. Para encontrar la referencia denominada `deptAudit`, la aplicación busca el nombre `java:comp/env/deptAudit` y envía el resultado a `AuditService`. El prefijo `java:comp/env/` que se agregó al nombre de la referencia indica al servidor que se debe buscar el contexto de nomenclatura del entorno para encontrar la referencia. Si el nombre se especifica incorrectamente, la búsqueda fallará.

***Listado 3-9*** Buscando una Dependencia de EJB

```java
@Stateless
@EJB(name="deptAudit", beanInterface=AuditService.class)
public class DeptService {
   private AuditService audit;
    
   @PostConstruct
   void init() {
      try {
         Context ctx = new InitialContext();
         audit = (AuditService) ctx.lookup("java:comp/env/deptAudit");
      } catch (NamingException e) {
         throw new EJBException(e);
      }
   }
   // ...
}
```

Todos los componentes de Java EE admiten el uso de la API JNDI para buscar referencias de recursos en el contexto de denominación del entorno. Sin embargo, es un método algo engorroso para encontrar un recurso debido a los requisitos de manejo de excepciones de JNDI. Los EJB también admiten una sintaxis alternativa utilizando el método `lookup()` de la interfaz `EJBContext`. La interfaz `EJBContext` (y subinterfaces como `SessionContext`) está disponible para cualquier EJB y proporciona al bean acceso a servicios de tiempo de ejecución como el servicio de temporizador. El Listado 3-10 muestra el mismo ejemplo que el Listado 3-9 usando el método `lookup()`. La instancia de `SessionContext` en este ejemplo se proporciona a través de un método de establecimiento que es llamado por el contenedor. Volvemos a visitar este ejemplo más adelante en la sección denominada "Referencias a los recursos del servidor" para ver cómo se invoca.


***Listado 3-10*** Usando el método `EJBContext lookup()`

```java
@Stateless
@EJB(name="deptAudit", beanInterface=AuditService.class)
public class DeptService  {
   SessionContext context;
   AuditService audit;
    
   public void setSessionContext(SessionContext context) {
      this.context = context;
   }
    
   @PostConstruct
   public void init() {
      audit = (AuditService) context.lookup("deptAudit");
   }
   // ...
}
```

El método `EJBContext lookup()` tiene dos ventajas sobre la API JNDI. La primera es que el argumento del método es el nombre exactamente como se especificó en la referencia del recurso. La segunda es que solo se lanzan excepciones de tiempo de ejecución desde el método `lookup()` para evitar el manejo de excepciones comprobado de la API JNDI. Detrás de escena, se realiza exactamente la misma secuencia de llamadas a la API JNDI del Listado 3-9, pero las excepciones JNDI se manejan automáticamente.

### DEPENDENCY INJECTION

Cuando se coloca una anotación de recurso en un campo o método de establecimiento, ocurren dos cosas. Primero, se declara una referencia de recurso como si se hubiera colocado en la clase de bean (similar a la forma en que funcionaba la anotación `@EJB` en el ejemplo del Listado 3-9), y el nombre de ese recurso se vinculará al entorno contexto de nomenclatura cuando se crea el componente. En segundo lugar, el servidor realiza la búsqueda automáticamente en su nombre y establece el resultado en la clase instanciada.

El proceso de buscar automáticamente un recurso y configurarlo en la clase se llama inyección de dependencia porque se dice que el servidor inyecta la dependencia resuelta en la clase. Esta técnica, una de las varias comúnmente denominadas inversión de control, elimina la carga de buscar recursos manualmente en el contexto del entorno JNDI.

La inyección de dependencias se considera una práctica recomendada para el desarrollo de aplicaciones, no solo porque reduce la necesidad de búsquedas JNDI sino también porque simplifica las pruebas. Sin ningún código de API JNDI en la clase que tenga dependencias en el entorno de ejecución del servidor de aplicaciones, la clase de bean se puede instanciar directamente en una prueba unitaria. Luego, el desarrollador puede proporcionar manualmente las dependencias necesarias y probar la funcionalidad de la clase en cuestión en lugar de preocuparse por cómo solucionar la búsqueda de JNDI.

#### ***Field Injection***

La primera forma de inyección de dependencia se llama ***Field Injection***. Inyectar una dependencia en un campo significa que después de que el servidor busca la dependencia en el contexto de nomenclatura del entorno, asigna el resultado directamente al campo anotado de la clase. El Listado 3-11 revisita el ejemplo del Listado 3-9 y demuestra un uso más simple de la anotación `@EJB`, esta vez inyectando el resultado en el campo `audit`. El código de la interfaz de directorio utilizado antes desapareció y los métodos de negocio del bean pueden asumir que el campo de auditoría contiene una referencia al bean `AuditService`.

***Listado 3-11*** Uso de Field Injection

```java
@Stateless
public class DeptService {
   @EJB AuditService audit;
   // ...
}
```

La Field Injection (inyección de campo) es sin duda la más fácil de implementar, y los ejemplos de este libro siempre optan por utilizar este formulario en lugar del formulario de búsqueda dinámica. Lo único que se debe tener en cuenta con la inyección de campo es que si está planeando realizar pruebas unitarias, debe agregar un método de establecimiento o hacer que el campo sea accesible para sus pruebas unitarias para satisfacer manualmente la dependencia. Los campos privados, aunque legales, requieren hacks desagradables si no hay una forma accesible de establecer su valor. Considere el alcance del paquete para la inyección de campo si desea realizar una prueba unitaria sin tener que agregar un establecedor.

Mencionamos en la sección anterior que se genera automáticamente un nombre para la referencia cuando se coloca una anotación de recurso en un campo o método de establecimiento. Para completar, describimos el formato de este nombre, pero es poco probable que encuentre muchas oportunidades para usarlo. El nombre generado es el nombre de clase completo, seguido de una barra diagonal y luego el nombre del campo o propiedad. Esto significa que si el bean `DeptService` se encuentra en el paquete `persistence.session`, el EJB inyectado al que se hace referencia en el Listado 3-9 sería accesible en el contexto de nomenclatura del entorno con el nombre `persistence.session.DeptService/audit`. La especificación del elemento `name` para la anotación de recurso anulará este valor predeterminado.

#### ***Setter Injection***

La segunda forma de inyección de dependencia se llama *setter injection* (inyección de setter) e implica anotar un método de setter en lugar de un campo de clase. Cuando el servidor resuelve la referencia, invocará el método de establecimiento anotado con el resultado de la búsqueda. El Listado 3-12 vuelve a visitar el Listado 3-9 una vez más para demostrar el uso de la inyección de setter.

***Listado 3-12*** Uso de Setter Injection

```java
@Stateless
public class DeptService {
   private AuditService audit;
   @EJB
   public void setAuditService(AuditService audit) {
      this.audit = audit;
   }
   // ...
}
```

Este estilo de inyección permite campos privados, pero también funciona bien con pruebas unitarias. Cada prueba puede simplemente instanciar la clase de bean y realizar manualmente la inyección de dependencia invocando el método setter, generalmente proporcionando una implementación del recurso requerido que se adapta a la prueba.

### DECLARACIÓN DE DEPENDENCIAS

Las siguientes secciones describen algunas de las anotaciones de recursos descritas en la especificación Java EE. Cada anotación tiene un atributo de nombre para especificar opcionalmente el nombre de referencia para la dependencia. Otros atributos de las anotaciones son específicos del tipo de recurso que se debe adquirir.

#### ***Referenciar un Persistence Context (Contexto de Persistencia)***

En el capítulo anterior, demostramos cómo crear un administrador de entidad para un contexto de persistencia usando un `EntityManagerFactory` devuelto por la clase `Persistence`. En el entorno Java EE, la anotación `@PersistenceContext` se puede utilizar para declarar una dependencia en un contexto de persistencia y hacer que el administrador de entidades para ese contexto de persistencia se adquiera automáticamente.

El Listado 3-13 demuestra el uso de la anotación `@PersistenceContext` para adquirir un administrador de entidades mediante la inyección de dependencias en un bean de sesión sin estado. El elemento `unitName` especifica el nombre de la unidad de persistencia en la que se basará el contexto de persistencia.

**TIP** ***Si se omite el elemento `unitName`, la forma en que se determina el nombre de la unidad para el contexto de persistencia depende del proveedor. Algunos proveedores pueden proporcionar un valor predeterminado si solo hay una unidad de persistencia para una aplicación, mientras que otros pueden requerir que el nombre de la unidad se especifique en un archivo de configuración específico del proveedor***.

***Listado 3-13*** Inyectando una instancia EntityManager

```java
@Stateless
public class EmployeeService {
   @PersistenceContext(unitName="EmployeeService")
   EntityManager em;
   // ...
}
```

Quizás se pregunte por qué existe un campo de estado en un bean de sesión sin estado; después de todo, los administradores de entidades deben mantener su propio estado para poder administrar un contexto de persistencia específico. La buena noticia es que la especificación se diseñó teniendo en cuenta la integración de contenedores, por lo que lo que realmente se inyecta en el Listado 3-13 no es una instancia de administrador de entidades como las que usamos en el capítulo anterior. El valor inyectado en el bean es un proxy administrado por contenedor que adquiere y libera contextos de persistencia en nombre del código de la aplicación. Esta es una característica poderosa de la API de persistencia de Java en Java EE y se trata ampliamente en el Capítulo 6.

Por ahora, es seguro asumir que el valor inyectado "hará lo correcto". No es necesario eliminarlo y funciona automáticamente con la gestión de transacciones del servidor de aplicaciones. Otros contenedores que admiten JPA, como Spring, ofrecerán una funcionalidad similar, pero generalmente requerirán alguna configuración adicional para que funcione.

#### ***Referenciar una Unidad de Persistencia***

Se puede hacer referencia a `EntityManagerFactory` para una unidad de persistencia usando la anotación `@PersistenceUnit`. Al igual que la anotación `@PersistenceContext`, el elemento `unitName` identifica la unidad de persistencia para la instancia `EntityManagerFactory` a la que queremos acceder. Si el nombre de la unidad persistente no se especifica en la anotación, la forma en que se determina el nombre depende del proveedor.

El listado 3-14 demuestra la inyección de una instancia de `EntityManagerFactory` en un bean de sesión con estado. Luego, el bean crea una instancia de `EntityManager` desde la fábrica durante la devolución de llamada del ciclo de vida de `PostConstruct`. Una instancia de `EntityManagerFactory` inyectada se puede almacenar de forma segura en cualquier instancia de componente. Es seguro para subprocesos y no es necesario eliminarlo cuando se elimina la instancia del bean.


***Listado 3-14*** Inyectando una instancia EntityManagerFactory

```java
@Stateful
public class EmployeeService {
   
   @PersistenceUnit(unitName="EmployeeService")
   private EntityManagerFactory emf;
   
   private EntityManager em;
   
   @PostConstruct
   public void init() {
      em = emf.createEntityManager();
   }
   // ...
}
```

`EntityManagerFactory` para una unidad de persistencia no se usa con tanta frecuencia en el entorno Java EE porque los administradores de entidades inyectados son más fáciles de adquirir y usar. Como verá en el Capítulo 6, existen diferencias importantes entre los administradores de entidades devueltos de fábrica y los proporcionados por el servidor en respuesta a la anotación `@PersistenceContext`.

#### ***Referencia a Server Resources (Recursos del Servidor)***

La anotación `@Resource` es la referencia general para los tipos de recursos de Java EE que no tienen anotaciones dedicadas. Se utiliza para definir referencias a fábricas de recursos, fuentes de datos y otros recursos del servidor. La anotación `@Resource` también es la más simple de definir porque el único elemento adicional es `resourceType`, que le permite especificar el tipo de recurso si el servidor no puede resolverlo automáticamente. Por ejemplo, si el campo en el que está inyectando es de tipo `Object`, entonces el servidor no puede saber que usted desea una fuente de datos. El elemento `resourceType` se puede establecer en `javax.sql.DataSource` para hacer explícita la necesidad.

Una de las características de la anotación `@Resource` es que se utiliza para adquirir recursos lógicos específicos del tipo de componente. Esto incluye implementaciones de `EJBContext`, así como servicios como el servicio de temporizador EJB. Sin definirlo como tal, usamos la inyección de setter para adquirir la instancia `EJBContext` en el Listado 3-10. Para completar ese ejemplo, la anotación `@Resource` podría haberse colocado en el método `setSessionContext()`. El Listado 3-15 revisa el ejemplo del Listado 3-10, esta vez demostrando la inyección de campo con `@Resource` para adquirir una instancia de SessionContext.

***Listado 3-15*** Inyectando una Instancia SessionContext

```java
@Stateless
@EJB(name="audit", beanInterface=AuditService.class)
public class DeptService {
   @Resource
   SessionContext context;
    
   AuditService audit;
    
   @PostConstruct
   public void init() {
      audit = (AuditService) context.lookup("audit");
   }
   // ...
}
```

## CDI y Contextual Injection

Si bien las instalaciones básicas de inyección de la plataforma son útiles, están claramente limitadas tanto en términos de lo que se puede inyectar como de cuánto control se puede ejercer sobre el proceso de inyección. CDI proporciona un estándar de inyección más poderoso que primero amplía la noción de un bean administrado y una inyección de recursos de plataforma y luego pasa a definir un conjunto de servicios de inyección adicionales disponibles para beans administrados por CDI. Por supuesto, la característica clave de la inyección contextual es la capacidad de inyectar una instancia de objeto determinada de acuerdo con el contexto actualmente activo.

Las capacidades de CDI son amplias y extensas, y obviamente más allá del alcance de un libro sobre JPA. Para los propósitos de este libro, solo rascamos la superficie y mostramos cómo crear y usar beans CDI simples con calificadores. Sugerimos que los lectores interesados consulten algunos de los muchos libros escritos sobre CDI para obtener más información sobre interceptores, decoradores, eventos y muchas otras características disponibles dentro de un contenedor CDI.

### CDI BEANS

Uno de los beneficios de los EJB es que brindan todos los servicios que uno pueda necesitar, desde seguridad hasta administración automática de transacciones y control de concurrencia. Sin embargo, el modelo de servicio completo puede verse como un inconveniente si no usa o no desea algunos de los servicios, ya que la percepción es que hay al menos algún costo asociado con tenerlos. Los beans administrados y las extensiones CDI para ellos brindan un modelo más de pago por uso. Solo obtiene los servicios que especifique. Sin embargo, no se deje engañar pensando que los beans CDI son menos voluminosos que un EJB moderno. Cuando se trata de la implementación, ambos tipos de objetos son enviados por el contenedor de la misma manera y los enlaces de servicio se agregarán y activarán según sea necesario.

¿Qué son los CDI beans, de todos modos? Un bean CDI es cualquier clase que califica para los servicios de inyección de CDI, cuyo requisito principal es simplemente que sea una clase concreta. <sup>2</sup> Incluso los beans de sesión pueden ser beans de CDI y, por lo tanto, calificar para los servicios de inyección de CDI, aunque hay algunas advertencias sobre sus contextos de ciclo de vida.

### INYECCION Y RESOLUCION

Un bean puede tener sus campos o propiedades como destino de la inyección si están anotados por `@javax.inject.Inject`. CDI define un algoritmo sofisticado para resolver el tipo correcto de objeto a inyectar, pero en el caso general, si tiene un campo declarado de tipo X, entonces se inyectará una instancia de X en él. Podemos reescribir el ejemplo del Listado 3-10 para usar beans CDI administrados simples en lugar de EJB. El listado 3-16 muestra el uso de la anotación de inyección en un campo. La instancia de `AuditService` se inyectará después de que se instancia la instancia de `DeptService`.

***Listado 3-16*** CDI Bean con Field Injection

```java
public class DeptService {
   @Inject AuditService audit;
   // ...
}
```

La anotación podría colocarse de manera similar en un método de propiedad para emplear la inyección de setter. Otra forma más de lograr el mismo resultado es utilizar un tercer tipo de inyección llamada *constructor injection* (inyección de constructor). Como sugiere el nombre, la inyección del constructor implica que el contenedor invoca el método del constructor e inyecta los argumentos. El listado 3-17 muestra cómo se puede configurar la inyección del constructor. La inyección de constructor es particularmente buena para probar fuera del contenedor, ya que un marco de prueba puede realizar fácilmente la inyección necesaria simplemente llamando al constructor sin tener que usar el proxy del objeto.

***Listing 3-17*** CDI Bean con Constructor Injection

```java
public class DeptService {
   private AuditService audit;
   @Inject DeptService(AuditService audit) {
      this.audit = audit;
   }
   // ...
}
```

### SCOPES Y CONTEXTS

Un alcance(scope) define un par de puntos de demarcación temporal: un principio y un final. Por ejemplo, el alcance de una solicitud comenzaría cuando la solicitud comienza y finalizaría cuando la respuesta se haya devuelto. De manera similar, otros alcances definen duraciones en función de las acciones y condiciones del cliente. Hay cinco ámbitos predefinidos, tres de los cuales (request, session y application) están definidos por la especificación del servlet, uno más (conversación) que fue agregado por CDI y otro que fue agregado por la especificación JTA:

* ***Request***: Delineada por una solicitud de método de cliente específico.

* ***Session***: Comienza al iniciarse desde un cliente HTTP y finaliza al finalizar la sesión HTTP. Compartido por todas las solicitudes en la misma sesión HTTP.

* ***Application***: Global para toda la aplicación mientras esté activa.

* ***Conversation***: Abarca una serie de solicitudes JSF secuenciales.

* ***Transaction***: Se asigna a la vida útil de una transacción JTA activa.

Un tipo de bean se asocia con un alcance anotando la clase de bean con la anotación de alcance deseada. Las instancias administradas de ese bean tendrán un ciclo de vida similar al alcance declarado.

Cada ámbito que esté activo tendrá un contexto actual asociado. Por ejemplo, cuando llega una solicitud, se creará un contexto de solicitud para ese alcance de solicitud. Cada solicitud tendrá su propio contexto de solicitud actual, pero habrá un contexto de ámbito de sesión único para todas las solicitudes provenientes de la misma sesión HTTP. El contexto es solo el lugar donde residen las instancias con ámbito durante la duración del ámbito. Solo puede haber una instancia de cada tipo de bean en cada contexto.

El bean `AuditService` de ámbito de aplicación que se inyectaría en el bean `DeptService` en los listados 3-16 o 3-17 tendría el siguiente aspecto:

```java
@ApplicationScoped
public class AuditService { ... }
```

Debido a que está dentro del ámbito de la aplicación, CDI crearía solo una instancia y residiría en el contexto del ámbito de la aplicación. Tenga en cuenta que esto es similar a un EJB singleton en el sentido de que toda la aplicación crearía y utilizaría una única instancia.

También existe un alcance adicional, dependiente, pero en realidad es la ausencia de alcance. Si no se especifica ningún alcance, se asume el alcance dependiente, lo que significa que no se colocan instancias del bean en ningún contexto y se crea una nueva instancia cada vez que se produce una inyección de ese tipo de bean. La anotación `@Dependent` se puede utilizar para marcar explícitamente un bean como dependiente. Las anotaciones de alcance se definen en el paquete `javax.enterprise.context`.

**TIP** ***Se requería el descriptor `beans.xml` en CDI 1.0. En CDI 1.1, los beans se pueden anotar con una anotación que define el bean (es decir, una anotación de alcance) para evitar que se requiera el descriptor `beans.xml`. En los ejemplos que usan CDI, usamos anotaciones de alcance para evitar tener que especificar un archivo `beans.xml`.

### QUALIFIED INJECTION

Un calificador es una anotación que se utiliza para restringir o distinguir un tipo de bean de otros tipos de bean que tienen el mismo tipo de interfaz heredado o implementado. Un calificador puede ayudar al contenedor a resolver qué tipo de bean inyectar.

La clase de anotación de calificador normalmente la define la aplicación y luego se utiliza para anotar una o más clases bean. La definición de anotación de calificador debe anotarse con la metaanotación `@Qualifier`, definida en el paquete `javax.inject`. Un ejemplo de cómo definir una anotación de calificador se encuentra en el Listado 3-18.

***Listado 3-18*** Definición Qualifier Annotation

```java
@Qualifier
@Target({METHOD, FIELD, PARAMETER, TYPE})
@Retention(RUNTIME)
public @interface Secure { }
```


Esta anotación ahora se puede usar en una clase de bean, como el nuevo bean `SecureDeptService` que se muestra en el Listado 3-19. El calificador indica que el bean es una variedad segura de `DeptService`, a diferencia de uno normal (¿no seguro?).

***Listado 3-19*** Qualified Bean Class

```java
@Secure
public class SecureDeptService extends DeptService { ... }
```

Cuando se va a inyectar un `DeptService` en otro componente, el calificador se puede colocar en el sitio de inyección y CDI activará su algoritmo de resolución para determinar qué instancia crear. Coincidirá con los calificadores para utilizar `SecureDeptService` en lugar de `DeptService`. El Listado 3-20 muestra cómo un servlet, por ejemplo, podría inyectar un `DeptService` seguro sin siquiera tener que saber el nombre de la subclase.

***Listado 3-20*** Qualified Injection

```java
public class LoginServlet extends HttpServlet {
   @Inject @Secure DeptService deptService ;
   // ...
}
```

### PRODUCER METHODS Y FIELDS

Cuando el contenedor necesita inyectar una instancia de un tipo de bean, primero buscará en los contextos actuales para ver si ya existe uno. Si no encuentra uno, debe obtener o crear una nueva instancia. CDI proporciona una forma para que la aplicación controle la instancia que se "crea" para un tipo determinado mediante el uso de un método o campo de productor.

Un método de productor es un método que el contenedor CDI invocará para obtener una nueva instancia de bean. La instancia puede ser instanciada por el método del productor o el productor puede obtenerla por algún otro medio; depende completamente de la implementación del método de productor cómo produce la instancia. Los métodos de productor pueden incluso decidir, basándose en las condiciones de tiempo de ejecución, devolver una subclase de bean diferente.

Un método de productor se puede anotar con un calificador. Luego, cuando un sitio de inyección está calificado de manera similar, el contenedor llamará al método de productor calificado correspondiente para obtener la instancia.

En el Listado 3-21, mostramos un método de productor que devuelve nuevas instancias seguras de `DeptService`. Esto podría ser útil, por ejemplo, si no pudiéramos modificar la clase `SecureDeptService` para anotarla con la anotación `@Secure` como hicimos en el Listado 3-19. En su lugar, podríamos declarar un método de productor que devolviera instancias de `SecureDeptService` y se inyectarían en campos calificados con `@Secure` (como el que se muestra en el Listado 3-20). Tenga en cuenta que un método de productor puede estar en cualquier bean administrado. Puede ser un método estático o de instancia, pero debe estar anotado con `@Produces`. Hemos creado una clase bean llamada `ProducerMethods` para contener el método producer y separarlo del resto de la lógica de la aplicación.

***Listado 3-21*** Producer Method

```java
public class ProducerMethods {
   @Produces @Secure
   DeptService secureDeptServiceProducer() {
      return new SecureDeptService();
   }
}
```

Los campos Producer funcionan de la misma manera que los métodos de productor, excepto que el contenedor accede al campo para obtener la instancia en lugar de invocar un método. Depende de la aplicación asegurarse de que el campo contenga una instancia cuando el contenedor la necesite. Verá un ejemplo del uso de un campo de productor en la siguiente sección.

### USO DE MÉTODOS PRODUCER CON JPA RESOURCES

Ahora que conoce algunos de los conceptos básicos de CDI, es hora de aprender cómo se puede usar CDI para ayudar a administrar las unidades de persistencia y los contextos de JPA. Puede utilizar una combinación de inyección de recursos Java EE con campos de productor de CDI y calificadores para inyectar y mantener sus contextos de persistencia.

Supongamos que tenemos dos unidades de persistencia, una llamada `Employee` y la otra llamada `Audit`. Queremos utilizar beans CDI e inyección contextual. Creamos una clase llamada `EmProducers` y usamos campos de productor e inyección de recursos Java EE para obtener los administradores de entidad. El código de productor y las definiciones de anotación de calificador se encuentran en el Listado 3-22.

***Listado 3-22*** Producer Class y Definiciones Qualifier Annotation  

```java
@RequestScoped
public class EmProducers {
   @Produces @EmployeeEM
   @PersistenceContext(unitName="Employee")
   private EntityManager em1;
    
   @Produces @AuditEM
   @PersistenceContext(unitName="Audit")
   private EntityManager em2;
}

@Qualifier
@Target({METHOD, FIELD, PARAMETER, TYPE})
@Retention(RUNTIME)
public @interface EmployeeEM { }

@Qualifier
@Target({METHOD, FIELD, PARAMETER, TYPE})
@Retention(RUNTIME)
public @interface AuditEM { }
```

La anotación Java EE `@PersistenceContext` hará que el contenedor Java EE inyecte los campos del productor con el administrador de la entidad para la unidad de persistencia dada. Luego, CDI utilizará los campos del productor para obtener el administrador de la entidad con el calificador correspondiente del sitio de inyección. Debido a que la clase de productor tiene un alcance de solicitud, se usarán nuevos administradores de entidad para cada solicitud. El alcance apropiado dependerá obviamente de la arquitectura de la aplicación. Lo único que queda por hacer es tener un bean que se inyecte con estos administradores de entidades. Un bean `DeptService` correspondiente que hace referencia a los administradores de entidades se muestra en el Listado 3-23. Un mismo enfoque podría usarse fácilmente para inyectar una fábrica de administradores de entidades utilizando la anotación de recursos `@PersistenceUnit`.


***Listado 3-23*** DeptService Bean con Injected EntityManager Fields

```java
public class DeptService {
   @Inject @EmployeeEM
   private EntityManager empEM;
   @Inject @AuditEM
   private EntityManager auditEM;
   // ...
}
```

## Transaction Management

Más que cualquier otro tipo de aplicación empresarial, las aplicaciones que utilizan la persistencia requieren una cuidadosa atención a los problemas de gestión de transacciones. Cuándo comienzan las transacciones, cuándo terminan y cómo el administrador de la entidad participa en las transacciones administradas por contenedores son temas esenciales para los desarrolladores que usan JPA. Las siguientes secciones establecen la base para las transacciones en Java EE; volvemos a examinar este tema en detalle en el Capítulo 6 al observar al administrador de la entidad y cómo participa en las transacciones. Los temas de transacciones avanzadas están fuera del alcance de este libro. Recomendamos ***Mastering Java EE 8 Application Development***  <sup>3</sup> para una discusión en profundidad sobre el desarrollo de aplicaciones en Java EE 8.

### TRANSACTION REVIEW

Una ***transacción*** es una abstracción que se utiliza para agrupar una serie de operaciones. Una vez agrupado, el conjunto de operaciones se trata como una sola unidad y todas las operaciones deben tener éxito o ninguna de ellas puede tener éxito. La consecuencia de que solo algunas de las operaciones sean exitosas es producir una vista inconsistente de los datos que será dañina o indeseable para la aplicación. ***El término utilizado para describir si las operaciones tienen éxito juntas o no se llama*** **atomicidad**, y es posiblemente la más importante de las cuatro propiedades básicas que se utilizan para caracterizar cómo se comportan las transacciones. Comprender estas cuatro propiedades es fundamental para comprender las transacciones. La siguiente lista resume estas propiedades:

* ***Atomicity*** (Atomicidad): Ya sea todas las operaciones de una transacción son exitosas o ninguna de ellas. El éxito de cada operación individual está ligado al éxito de todo el grupo.

* ***Consistency*** (Coherencia): El estado resultante al final de la transacción se adhiere a un conjunto de reglas que definen la aceptabilidad de los datos. Los datos de todo el sistema son legales o válidos con respecto al resto de datos del sistema.

* ***Isolation*** (Aislamiento): Los cambios realizados dentro de una transacción son visibles solo para la transacción que realiza los cambios. Una vez que una transacción confirma los cambios, son visibles atómicamente para otras transacciones.

* ***Durability*** (Durabilidad): Los cambios realizados dentro de una transacción perduran más allá de la finalización de la transacción.

Se dice que una transacción que cumple con todos estos requisitos es una transacción ACID (el término familiar ACID se obtiene al combinar la primera letra de cada una de las cuatro propiedades).

No todas las transacciones son transacciones ACID, y aquellas que lo son a menudo ofrecen cierta flexibilidad en el cumplimiento de las propiedades ACID. Por ejemplo, el nivel de aislamiento es una configuración común que se puede configurar para proporcionar grados de aislamiento más flexibles o más estrictos que los descritos anteriormente. Por lo general, se realizan por razones de mayor rendimiento o, en el otro lado del espectro, si una aplicación tiene requisitos de coherencia de datos más estrictos. Las transacciones que discutimos en el contexto de Java EE son normalmente de la variedad ACID.

### TRANSACCIONES EMPRESARIALES EN JAVA

En realidad, las transacciones existen en diferentes niveles dentro del servidor de aplicaciones empresariales. La transacción más baja y básica está al nivel del recurso, que en nuestra discusión se supone que es una base de datos relacional encabezada por una interfaz `DataSource`. Esto se denomina transacción local de recursos y es equivalente a una transacción de base de datos. Estos tipos de transacciones se manipulan interactuando directamente con el JDBC `DataSource` que se obtiene del servidor de aplicaciones. Las transacciones de recursos locales se utilizan con menos frecuencia en servidores que las transacciones de contenedor.

La transacción de contenedor más amplia utiliza la API Java Transaction API (JTA) que está disponible en todos los servidores de aplicaciones Java EE compatibles y en la mayoría de los servidores web basados en Java. Esta es la transacción típica que se utiliza para aplicaciones empresariales y puede involucrar o incorporar una serie de recursos, incluidas las fuentes de datos y otros tipos de recursos transaccionales. Los recursos definidos mediante componentes de Java Connector Architecture (JCA) también se pueden incluir en la transacción del contenedor.

Los contenedores generalmente agregan su propia capa sobre JDBC `DataSource` para realizar funciones como la administración de conexiones y la agrupación que hacen un uso más eficiente de los recursos y proporcionan una integración perfecta con el sistema de administración de transacciones. Esto también es necesario porque es responsabilidad del contenedor realizar la operación de confirmación o reversión en el origen de datos cuando se completa la transacción del contenedor.

Debido a que las transacciones de contenedor usan JTA y debido a que pueden abarcar varios recursos, también se denominan transacciones JTA o transacciones globales. La transacción del contenedor es un aspecto central de la programación dentro de los servidores Java.

#### ***Transaction Demarcation***

Cada transacción tiene un principio y un final. Comenzar una transacción permitirá que las operaciones posteriores se conviertan en parte de la misma transacción hasta que la transacción se haya completado. Las transacciones se pueden completar de dos formas. Se pueden comprometer, provocando todos los cambios a ser persistir en el almacén de datos o revertir, lo que indica que los cambios deben descartarse. El acto de hacer que una transacción comience o se complete se denomina ***demarcación de transacción***. Esta es una parte fundamental de la escritura de aplicaciones empresariales, porque la demarcación de transacciones de forma incorrecta es una de las fuentes más comunes de degradación del rendimiento.

Las transacciones de recursos locales siempre son demarcadas explícitamente por la aplicación, mientras que las transacciones de contenedor pueden ser demarcadas automáticamente por el contenedor o usando una interfaz JTA que admita la demarcación controlada por la aplicación. El primer caso, cuando el contenedor asume la responsabilidad de la demarcación de transacciones, se denomina gestión de transacciones gestionadas por contenedor (CMT), pero cuando la aplicación es responsable de la demarcación, se denomina gestión de transacciones gestionadas por bean (BMT).

Los EJB pueden utilizar transacciones gestionadas por contenedor o transacciones gestionadas por bean. Los servlets se limitan a la transacción gestionada por beans con un nombre deficiente. El estilo de gestión de transacciones predeterminado para un componente EJB está gestionado por contenedor. Para configurar explícitamente un EJB para que sus transacciones estén demarcadas de una forma u otra, la anotación `@TransactionManagement` debe especificarse en la clase EJB. El tipo enumerado `TransactionManagementType` define `BEAN` para transacciones administradas por bean y `CONTAINER` para transacciones administradas por contenedor. El Listado 3-24 demuestra cómo habilitar transacciones administradas por beans usando este enfoque.

***Listado 3-24*** Cambiar la Transaction Management Type de un EJB

```java
@Stateless
@TransactionManagement(TransactionManagementType.BEAN)
public class ProjectService {
   // methods in this class manually control transaction demarcation
}
```

**NOTA** ***Debido a que la administración de transacciones predeterminada para un EJB es administrada por contenedor, la anotación `@TransactionManagement` solo debe especificarse si se desean transacciones administradas por bean.

#### ***Container-Managed Transactions***

La forma más común de demarcar transacciones es usar CMT, que ahorran a la aplicación el esfuerzo y el código para comenzar y confirmar transacciones de manera explícita.

Los requisitos de transacción están determinados por metadatos y se pueden configurar según la granularidad de la clase o incluso de un método. Por ejemplo, un bean de sesión puede declarar que cada vez que se invoca un método específico en ese bean, el contenedor debe asegurarse de que se inicie una transacción antes de que comience el método. El contenedor también es responsable de confirmar la transacción después de completar el método.

Es bastante común que un bean invoque a otro bean desde uno o más de sus métodos. En este caso, una transacción iniciada por el método de llamada no se habrá confirmado porque el método de llamada no se completará hasta que se complete su llamada al segundo bean. Es por eso que necesitamos configuraciones para definir cómo debe comportarse el contenedor cuando se invoca un método dentro de un contexto transaccional específico.

Por ejemplo, si una transacción ya está en curso cuando se llama a un método, se puede esperar que el contenedor solo use esa transacción, mientras que se le puede indicar que inicie una nueva si no hay ninguna transacción activa. Estas configuraciones se denominan atributos de transacción y determinan el comportamiento transaccional administrado por el contenedor.

Las opciones de atributos de transacción definidos son las siguientes:

* `MANDATORY`: Si se especifica este atributo para un método, se espera que una transacción ya se haya iniciado y esté activa cuando se llame al método. Si no hay ninguna transacción activa, se lanza una excepción. Este atributo rara vez se usa, pero puede ser una herramienta de desarrollo para detectar errores de demarcación de transacciones cuando se espera que una transacción ya se haya iniciado.

* `REQUIRED`: Este atributo es el caso más común en el que se espera que un método esté en una transacción. El contenedor proporciona una garantía de que una transacción está activa para el método. Si uno ya está activo, se utiliza; si no existe, se crea una nueva transacción para la ejecución del método.

* `REQUIRES_NEW`: Este atributo se usa cuando el método siempre necesita estar en su propia transacción; es decir, el método debe confirmarse o deshacerse independientemente de los métodos que se encuentran más arriba en la pila de llamadas. Debe utilizarse con precaución porque puede generar una sobrecarga de transacción excesiva.

* `SUPPORTS`: Los métodos marcados con apoyos no dependen de una transacción, pero tolerarán la ejecución dentro de una si existe. Este es un indicador de que no se accede a recursos transaccionales en el método.

* `NOT_SUPPORTED`: Un método marcado para no admitir transacciones hará que el contenedor suspenda la transacción actual si hay una activa cuando se llama al método. Implica que el método no realiza operaciones transaccionales, pero puede fallar de otras formas que podrían afectar indeseablemente el resultado de una transacción. Este no es un atributo de uso común.

* `NEVER`: Un método marcado para no admitir nunca transacciones hará que el contenedor genere una excepción si una transacción está activa cuando se llama al método. Este atributo se usa muy poco, pero puede ser una herramienta de desarrollo para detectar errores de demarcación de transacciones cuando se espera que las transacciones ya se hayan completado.

Cada vez que el contenedor inicia una transacción para un método, se supone que el contenedor también intenta confirmar la transacción al final del método. Cada vez que se debe suspender la transacción actual, el contenedor es responsable de reanudar la transacción suspendida al finalizar el método.

En realidad, hay dos formas diferentes de especificar transacciones administradas por contenedor, una para EJB y otra para beans CDI, servlets, clases de recursos JAX-RS y todos los demás tipos de componentes administrados de Java EE. Los EJB fueron el primer tipo de componente en ofrecer una función de transacción administrada por contenedor y metadatos específicos definidos para este propósito. Los beans CDI y otros tipos de componentes Java EE, como servlets o clases de recursos JAX-RS, utilizan un interceptor transaccional.

#### ***EJB Container-Managed Transactions***

El atributo de transacción para un EJB se puede indicar anotando la clase EJB, o uno de sus métodos que forma parte de la interfaz empresarial opcional, con la anotación `@TransactionAttribute`. Esta anotación requiere un único argumento del tipo enumerado `TransactionAttributeType`, cuyos valores se definen en la lista anterior. Anotar la clase bean hará que el atributo de transacción se aplique a todos los métodos comerciales de la clase, mientras que anotar un método aplica el atributo sólo al método. Si existen anotaciones tanto a nivel de clase como a nivel de método, la anotación a nivel de método tiene prioridad. En ausencia de anotaciones `@TransactionAttribute` de nivel de clase o de método, el atributo predeterminado `REQUIRED` sera aplicado.

El Listado 3-25 muestra cómo el método `addItem()` del bean de carrito de compras en el Listado 3-5 podría usar un atributo de transacción. No se proporcionó ninguna configuración de gestión de transacciones, por lo que se utilizarán transacciones gestionadas por contenedor. No se especificó ningún atributo en la clase, por lo que el comportamiento predeterminado de `REQUIRED` se aplicará a todos los métodos de la clase. La excepción es que el método `addItem()` ha declarado un atributo de transacción de `SUPPORTS`, que anula la configuración `REQUIRED`. Siempre que se haga una llamada para agregar un artículo, ese artículo se agregará al carrito, pero si no hay ninguna transacción activa, no será necesario iniciar ninguna.

***Listado 3-25*** Especificación de un EJB Transaction Attribute

```java
@Stateful
public class ShoppingCart {
   @TransactionAttribute(TransactionAttributeType.SUPPORTS)
   public void addItem(String item, Integer quantity) {
      verifyItem(item, quantity);
      // ...
   }
   // ...
}
```

Además, antes de que el método `addItem()` agregue el artículo al carrito, realiza alguna validación en un método privado llamado `verifyItem()` que no se muestra en el ejemplo. Cuando se invoca este método desde `addItem()`, se ejecutará en cualquier contexto transaccional que se haya invocado `addItem()`.

Cualquier bean que desee provocar la reversión de una transacción gestionada por contenedor puede hacerlo invocando el método `setRollbackOnly()` en el objeto `EJBContext`. Aunque esto no provocará la reversión inmediata de la transacción, es una indicación para el contenedor de que la transacción debe revertirse cuando se complete. Tenga en cuenta que los administradores de entidad también harán que la transacción actual se configure para revertir cuando se lanza una excepción durante la invocación de un administrador de entidad o cuando se completa la transacción.

#### ***Transactional Interceptors***

Cuando se introdujeron los interceptores en la plataforma Java EE 6, abrieron la posibilidad futura de desacoplar la facilidad de transacción administrada por contenedor de EJB y ofrecerla a cualquier componente que admitiera la interceptación. En Java EE 7, se agregó la anotación `@javax.transaction.Transactional` como un medio para especificar que se aplicaría un interceptor transaccional al componente de destino. El mecanismo actúa de manera similar a las transacciones administradas por contenedor de EJB, ya que se aplica de forma declarativa en una clase o método y una semántica de clase puede ser anulada por una basada en un método. La principal diferencia es que se usa la anotación `@Transactional` en lugar de `@TransactionAttribute`, y el tipo de valor enumerado es una enumeración anidada llamada `Transactional.TxType` en lugar del `TransactionAttributeType` que se usa en EJB. Estas constantes de enumeración se denominan exactamente igual en `Transactional.TxType` y tienen la misma semántica.

Quizás la diferencia más relevante entre los componentes basados en interceptores transaccionales y EJB CMT es el hecho de que los componentes que utilizan interceptores transaccionales no obtienen CMT automáticamente, sino que deben optar por utilizar la anotación `@Transactional`. Los EJB obtienen CMT de forma predeterminada y deben optar por no participar si prefieren utilizar BMT.

Ahora podemos reescribir el Listado 3-25 para usar un bean CDI en lugar de un bean de sesión con estado. El bean `ShoppingCart` del Listado 3-26 tiene un ámbito de sesión para que su estado se mantenga durante toda la sesión. La anotación `@Transactional` vacía hace que la transaccionalidad predeterminada se establezca en `REQUIRED` para todos los métodos de la clase, excepto para el método `addItem()` que se anula explícitamente para ser `SUPPORTS`.

***Listado 3-26*** Transactional Interceptor en un CDI Bean

```java
@Transactional
@SessionScoped
public class ShoppingCart {
   @Transactional(TxType.SUPPORTS)
   public void addItem(String item, Integer quantity) {
      verifyItem(item, quantity);
      // ...
   }
   // ...
}
```

**TIP** ***`@Transactional` se puede utilizar en beans gestionados o beans CDI, pero no se puede utilizar en EJB. Por el contrario, `@TransactionAttribute` solo se puede utilizar en EJB.***

#### ***Bean-Managed Transactions***

Otra forma de demarcar transacciones es utilizar BMT. Esto solo significa que la aplicación necesita iniciar y detener transacciones explícitamente haciendo llamadas a la API. Con la excepción de los EJB, todos los componentes administrados utilizan de forma predeterminada transacciones administradas por beans y deben hacer su propia demarcación de transacciones si no especifican el interceptor `@Transactional`. Solo los EJB reciben transacciones gestionadas por contenedor de forma predeterminada.

Declarar que un EJB está utilizando transacciones gestionadas por beans significa que la clase de beans asume la responsabilidad de comenzar y confirmar las transacciones siempre que lo considere necesario. Sin embargo, con esta responsabilidad viene la expectativa de que la clase bean lo haga bien. Los EJB que utilizan BMT deben asegurarse de que cada vez que se inicia una transacción, también se debe completar antes de regresar del método que la inició. De no hacerlo, el contenedor revertirá la transacción automáticamente y se lanzará una excepción.

Una penalización de las transacciones administradas por los EJB en lugar del contenedor es que no se propagan a los métodos llamados en otro BMT EJB. Por ejemplo, si EJB A comienza una transacción y luego llama a EJB B, que utiliza transacciones gestionadas por bean, la transacción no se propagará al método en EJB B. Siempre que una transacción esté activa cuando se invoca un método BMT EJB, la transacción activa se suspenderá hasta que el control vuelva al método de llamada. Esto no es cierto para los componentes que no son de EJB.

Por lo general, no se recomienda el uso de BMT en EJB porque agrega complejidad a la aplicación y requiere que la aplicación realice un trabajo que el servidor ya puede hacer. Si bien otros tipos de componentes pueden usar interceptores transaccionales si así lo desean, no tienen las mismas restricciones de BMT que los EJB, por lo que es más común que adopten un enfoque de BMT controlado por aplicaciones. También han utilizado tradicionalmente BMT porque nunca hubo realmente una opción en el pasado. Solo recientemente, en Java EE 7, se introdujeron los interceptores de transacciones.

#### ***UserTransaction***

Para que un componente pueda comenzar y confirmar transacciones de contenedor manualmente, la aplicación debe tener una interfaz que lo admita. La interfaz `javax.transaction.UserTransaction` es el objeto designado en el JTA al que los componentes de la aplicación pueden aferrarse e invocar para gestionar los límites de las transacciones. Una instancia de `UserTransaction` no es en realidad la instancia de transacción actual; es una especie de proxy que proporciona la API de transacciones y representa la transacción actual. Se puede inyectar una instancia de `UserTransaction` en los componentes utilizando las anotaciones Java EE `@Resource` o CDI `@Inject`. Una `UserTransaction` también está presente en el contexto de nomenclatura del entorno con el nombre reservado `java:comp/UserTransaction`. La interfaz `UserTransaction` se muestra en el Listado 3-27.


***Listado 3-27*** La Interface UserTransaction 

```java
public interface UserTransaction {
   public abstract void begin();
   public abstract void commit();
   public abstract int getStatus();
   public abstract void rollback();
   public abstract void setRollbackOnly();
   public abstract void setTransactionTimeout(int seconds);
}
```

Cada transacción JTA está asociada con un hilo de ejecución, por lo que se deduce que no puede haber más de una transacción activa en un momento dado. Entonces, si una transacción está activa, el usuario no puede iniciar otra en el mismo hilo hasta que la primera se haya confirmado o revertido. Alternativamente, la transacción puede expirar, haciendo que la transacción se revierta.

Discutimos anteriormente que en ciertas condiciones de CMT, el contenedor suspenderá la transacción actual. En la API anterior, puede ver que no existe un método `UserTransaction` para suspender una transacción. Solo el contenedor puede hacer esto mediante una API de gestión de transacciones interna. De esta manera, se pueden asociar múltiples transacciones con un solo hilo, aunque solo una pueda estar activa a la vez.

Las reversiones pueden ocurrir en varios escenarios diferentes. El método `setRollbackOnly()` indica que la transacción actual no se puede confirmar, dejando la reversión como el único resultado posible. La transacción se puede revertir inmediatamente llamando al método `rollback()`. Alternativamente, se puede establecer un límite de tiempo para la transacción con el método `setTransactionTimeout()`, lo que hace que la transacción retroceda cuando se alcanza el límite. El único inconveniente con los tiempos de espera de las transacciones es que el límite de tiempo debe establecerse antes de que comience la transacción y no se puede cambiar una vez que la transacción está en curso.

En JTA, cada hilo tiene un estado transaccional al que se puede acceder a través de la llamada `getStatus()`. El valor de retorno de este método es una de las constantes definidas en la interfaz `javax.transaction.Status`. Si no hay ninguna transacción activa, por ejemplo, el valor devuelto por `getStatus()` será `STATUS_NO_TRANSACTION`. Del mismo modo, si se ha llamado a `setRollbackOnly()` en la transacción actual, el estado será `STATUS_MARKED_ROLLBACK` hasta que la transacción haya comenzado a retroceder.

El Listado 3-28 muestra un fragmento de un servlet que usa el bean `ProjectService` para demostrar el uso de `UserTransaction` para invocar múltiples métodos EJB dentro de una sola transacción. El método `doPost()` usa la instancia `UserTransaction` inyectada con la anotación `@Resource` para iniciar y confirmar una transacción. Tenga en cuenta el bloque `try… catch` requerido alrededor de las operaciones de transacción para garantizar que la transacción se limpie correctamente en caso de falla. Detectamos la excepción y luego la volvemos a lanzar dentro de una excepción de tiempo de ejecución después de realizar el rollback.

***Listado 3-28*** Usando la Interface UserTransaction 

```java
public class ProjectServlet extends HttpServlet {
    @Resource UserTransaction tx;
    @EJB ProjectService bean;
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
        throws ServletException, IOException {
        // ...
        try {
            tx.begin();
            bean.assignEmployeeToProject(projectId, empId);
            bean.updateProjectStatistics();
            tx.commit();
        } catch (Exception e) {
            // Try to roll back (may fail if exception came from begin() or commit(), but that's ok)
            try { tx.rollback(); } catch (Exception e2) {}
            throw new MyRuntimeException(e);
        }
        // ...
    }
}
```

## Poniendolo todo junto

Ahora que hemos analizado el modelo de componentes de la aplicación y los servicios disponibles como parte de un servidor de aplicaciones Java EE, podemos volver a visitar el ejemplo de `EmployeeService` del capítulo anterior y llevarlo al entorno Java EE. A lo largo del camino, proporcionamos código de ejemplo para mostrar cómo encajan los componentes y cómo se relacionan con el ejemplo de Java SE.

### DEFINIENDO EL COMPONENTE

Para comenzar, consideremos la definición de la clase `EmployeeService` del Listado 2-9 en el Capítulo 2. El objetivo de esta clase es proporcionar operaciones comerciales relacionadas con el mantenimiento de los datos de los empleados. Al hacerlo, encapsula todas las operaciones de persistencia. Para introducir esta clase en el entorno Java EE, primero debemos decidir cómo se debe representar. El patrón de servicio exhibido por la clase sugiere un bean de sesión o un componente similar. Debido a que los métodos de negocio del bean no tienen dependencia entre sí, podemos decidir además que cualquier bean sin estado, como un bean de sesión sin estado, es adecuado. De hecho, este bean demuestra un patrón de diseño muy típico llamado *fachada de sesión(session façade)*, <sup>4</sup> en el que se utiliza un bean de sesión sin estado para proteger a los clientes de tratar con una API de persistencia particular. Para convertir la clase `EmployeeService` en un bean de sesión sin estado, solo necesitamos anotarlo con `@Stateless`.

En el ejemplo de Java SE, la clase `EmployeeService` debe crear y mantener su propia instancia de administrador de entidad. Podemos reemplazar esta lógica con inyección de dependencia para adquirir el administrador de la entidad automáticamente. Habiendo decidido un bean de sesión sin estado y una inyección de dependencia, el bean de sesión sin estado convertido se muestra en el Listado 3-29. Con la excepción de cómo se adquiere el administrador de la entidad, el resto de la clase es idéntico. Esta es una característica importante de la API de persistencia de Java porque la misma interfaz `EntityManager` se puede utilizar tanto dentro como fuera del servidor de aplicaciones.

***Listado 3-29*** El EmployeeService Session Bean

```java
@Stateless
public class EmployeeService {

   @PersistenceContext(unitName="EmployeeService")
   protected EntityManager em;
   
   EntityManager getEntityManager() {
        return em;
   }
   
   public Employee createEmployee(int id, String name, long salary) {
      Employee emp = new Employee(id);
      emp.setName(name);
      emp.setSalary(salary);
      getEntityManager().persist(emp);
      return emp;
   }
    
   public void removeEmployee(int id) {
      Employee emp = findEmployee(id);
      if (emp != null) {
         getEntityManager().remove(emp);
      }
   }
    
   public Employee changeEmployeeSalary(int id, long newSalary) {
      Employee emp = findEmployee(id);
      if (emp != null) {
         emp.setSalary(newSalary);
      }
      return emp;
   }
    
   public Employee findEmployee(int id) {
      return getEntityManager().find(Employee.class, id);
   }
    
   public List<Employee> findAllEmployees() {
      TypedQuery query = getEntityManager().createQuery("SELECT e FROM Employee e", Employee.class);
      return query.getResultList();
   }
}
```

### DEFINIENDO LA INTERFACE USER 

La siguiente pregunta a considerar es cómo se accederá al bean. Una interfaz web es un método de presentación común para aplicaciones empresariales. Para demostrar cómo un servlet puede utilizar este bean de sesión sin estado, considere el Listado 3-30. Los parámetros de la solicitud se interpretan para determinar la acción, que luego se lleva a cabo invocando métodos en el bean `EmployeeService` inyectado. Aunque solo se describe la primera acción, puede ver cómo esto podría extenderse fácilmente para manejar cada una de las operaciones definidas en EmployeeService.

***Listado 3-30*** Uso del Bean de Sesión `EmployeeService` desde un Servlet

```java
public class EmployeeServlet extends HttpServlet {
   @EJB 
   EmployeeService bean;
   
   protected void doPost(HttpServletRequest request, HttpServletResponse response) {
      String action = request.getParameter("action");
      if (action.equals("create")) {
         String id = request.getParameter("id");
         String name = request.getParameter("name");
         String salary = request.getParameter("salary");
         bean.createEmployee(Integer.parseInt(id), name, Long.parseLong(salary));
      }
      // ...
    }
}
```

### PACKAGING IT UP

En el entorno Java EE, se pueden omitir muchas propiedades necesarias en el archivo `persistence.xml` para Java SE. En el Listado 3-31, verá el archivo `persistence.xml` del Listado 2-11 convertido para su implementación como parte de una aplicación Java EE. En lugar de las propiedades de JDBC para crear una conexión, ahora declaramos que el administrador de la entidad debe usar el nombre de la fuente de datos `jdbc/EmployeeDS`. Si la fuente de datos se definió para estar disponible en el espacio de nombres de la aplicación en lugar del contexto de nomenclatura del componente local, en su lugar podríamos usar el nombre de la fuente de datos de `java:app/jdbc/EmployeeDS`. El atributo de tipo de transacción también se ha eliminado para permitir que la unidad de persistencia se establezca por defecto en JTA. El servidor de aplicaciones encontrará automáticamente las clases de entidad, por lo que incluso la lista de clases se ha eliminado. Este ejemplo representa la configuración mínima ideal de Java EE.

Debido a que la lógica empresarial que utiliza esta unidad de persistencia se implementa en un bean de sesión sin estado, el archivo `persistence.xml` normalmente se ubicaría en el directorio `META-INF` del correspondiente EJB JAR, o en el directorio `WEB-INF/classes/META-INF` del WAR. Describimos completamente el archivo `persistence.xml` y su ubicación dentro de una aplicación Java EE en el Capítulo 14.

***Listado 3-31*** Definición de una Unidad de Persistencia en Java EE

```html
<persistence>
   <persistence-unit name="EmployeeService">
      <jta-data-source>jdbc/EmployeeDS</jta-data-source>
   </persistence-unit>
</persistence>
```

## Resumen

Sería imposible proporcionar detalles sobre todas las características de la plataforma Java EE en un solo capítulo. Sin embargo, no podemos poner JPA en contexto sin explicar el entorno del servidor de aplicaciones en el que se utilizará. Con este fin, presentamos las tecnologías que son de mayor relevancia para el desarrollador que utiliza la persistencia en aplicaciones empresariales.

Comenzamos con una introducción a los componentes de software empresarial y presentamos el modelo de componentes EJB. Argumentamos que el uso de componentes es más importante que nunca e identificamos algunos de los beneficios que se obtienen al aprovecharlos. Introdujimos los fundamentos de los beans de sesión sin estado, con estado y singleton y mostramos la sintaxis para declararlos, así como la diferencia en el estilo de interacción entre ellos.

A continuación, analizamos la gestión de dependencias en los servidores de aplicaciones Java EE. Discutimos los tipos de anotaciones de referencia y cómo declararlos. También analizamos la diferencia entre la dependency lookup (búsqueda de dependencia ) y la ependency injection (inyección de dependencia). En el caso de la inyección, analizamos la diferencia entre la inyección de campo y la de incubadora. Luego exploramos cada uno de los tipos de recursos, demostrando cómo adquirir e inyectar recursos de servidor y JPA.

Sobre la base de la inyección de recursos Java EE, pasamos a presentar el modelo CDI con su noción generalizada de bean administrado. Enumeramos los ámbitos predefinidos y explicamos los contextos que usa CDI para almacenar en caché las instancias contextuales que inyecta. Mostramos cómo los calificadores pueden contribuir con restricciones adicionales al proceso de resolución de la inyección y cómo se pueden definir los productores para devolver las instancias que el contenedor CDI usa para la inyección. Luego demostramos una forma de utilizar a los productores para inyectar recursos de persistencia calificados.

En la sección sobre gestión de transacciones, analizamos JTA y su función en la creación de aplicaciones centradas en datos. Luego, analizamos la diferencia entre las transacciones administradas por bean y las transacciones administradas por contenedor para EJB y no EJB. Documentamos los diferentes tipos de atributos de transacción para los beans CMT y mostramos cómo controlar manualmente las transacciones administradas por beans.

Concluimos el capítulo explorando cómo usar componentes Java EE con JPA convirtiendo la aplicación de ejemplo presentada en el capítulo anterior de una aplicación Java SE de línea de comandos a una aplicación basada en web que se ejecuta en un servidor de aplicaciones.

Ahora que hemos introducido JPA en los entornos Java SE y Java EE, es hora de profundizar en la especificación en detalle. En el siguiente capítulo, comenzamos este viaje con el enfoque central de JPA: el mapeo relacional de objetos.

#### Notas al pie

**1** Todas las anotaciones utilizadas en este capítulo se definen en los paquetes `javax.ejb`, `javax.inject`, `javax.enterprise.inject` o `javax.annotation`.

**2** Se excluyen las clases internas no estáticas.
 
**3** Kapila Bogahapitiya, Sandeep Nair. *Mastering Java EE 8 Application Development. Paperback: 2018*

**4** Alur et al., *Core J2EE Patterns*.
