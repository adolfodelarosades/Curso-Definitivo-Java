# Capítulo 1. Pruebas de web services y soapUI

Los web services son una de las claves de los bloques de construcción para soluciones orientadas a servicios. Debido a su uso e importancia en las aplicaciones empresariales(enterprise applications), se espera que los equipos del proyecto estén bien informados y familiarizados con las tecnologías asociadas con los web services y la **arquitectura orientada a servicios - service-oriented architecture(SOA)**. El aspecto de prueba de los web services en particular es uno de los temas clave que debe discutirse cuando se trabaja con web services.

Las pruebas(testing) de web services se pueden realizar utilizando muchos enfoques. Las API de cliente incluidas en frameworks de web services como **Apache Axis2** se pueden usar para invocar web services mediante programación. Además de eso, hay varias herramientas de propiedad y de código abierto disponibles para probar los web services automáticamente. **soapUI** es una de esas herramientas de prueba gratuitas y de código abierto que admite evaluaciones funcionales y no funcionales de web services.

Discutiremos los siguientes temas en este capítulo que le proporcionarán una introducción a los conceptos básicos de **SOA**, pruebas de web services y **soapUI**:

* Descripción general de algunas de las características clave de los web services
* El papel de los web services en SOA
* Enfoques de testing de web services
* Desafíos de testing de web services
* Introducción a la interfaz de usuario de soapUI
* Instalación de la interfaz de usuario de soapUI

## SOA y Web Services

***SOA es un distintivo enfoque para separar preocupaciones y construir soluciones comerciales utilizando componentes reutilizables y poco acoplados***. SOA ya no es una característica agradable para la mayoría de las empresas y se usa ampliamente en las organizaciones para lograr muchas ventajas estratégicas. Al adoptar SOA, las organizaciones pueden permitir que sus aplicaciones comerciales respondan de manera rápida y eficiente a los cambios comerciales, de procesos y de integración que generalmente ocurren en cualquier entorno empresarial.

### Service-oriented solutions - Soluciones orientadas al servicio

Si un sistema de software se construye siguiendo los principios asociados con SOA, se puede considerar como una solución orientada a servicios. ***Las organizaciones generalmente tienden a crear soluciones orientadas a servicios para aprovechar la flexibilidad en sus negocios, fusionarse o adquirir nuevos negocios y lograr ventajas competitivas***. Para comprender el uso y el propósito de SOA y las soluciones orientadas a servicios, echemos un vistazo a un estudio de caso simplificado.

#### Caso de Estudio

**Smith and Co.** es un gran proveedor de pólizas de seguros de automóviles ubicado en América del Norte. La empresa utiliza un sistema de software para realizar todas sus operaciones asociadas con el procesamiento de reclamos de seguros. El sistema consta de varios módulos, incluidos los siguientes:

* Inscripción y registro de clientes
* Tramitación de pólizas de seguro
* Procesamiento de reclamos de seguros
* Gestión de clientes
* Contabilidad
* Gestión de proveedores de servicios

Con el enorme éxito y la satisfacción del cliente de las reclamaciones de seguros procesadas por la compañía durante el pasado reciente, **Smith and Co.** adquirió **InsurePlus Inc.**, uno de sus proveedores de seguros competidores, hace unos meses.

**InsurePlus** también ha proporcionado algunas de las pólizas de reclamaciones de automóviles de seguros que son similares a las que **Smith and Co.** proporciona a sus clientes. Por lo tanto, la dirección de la empresa ha decidido integrar los sistemas de procesamiento de reclamaciones de seguros utilizados por ambas empresas y ofrecer una solución a sus clientes.

**Smith and Co.** utiliza muchas tecnologías de Microsoft(TM) y todas sus aplicaciones de software, incluido el sistema general de administración de pólizas de seguros, se basan en el framework .NET. Por otro lado, **InsurePlus** usa mucho **J2EE** y sus aplicaciones de procesamiento de seguros están todas basadas en tecnologías Java. Para empeorar el problema de la integración, **InsurePlus** también consta de un componente de aplicación de administración de clientes heredado, que se ejecuta en un sistema AS-400.

Los departamentos de TI de ambas empresas enfrentaron numerosas dificultades cuando intentaron integrar las aplicaciones de software en **Smith and Co.** e **InsurePlus Inc.**. Tuvieron que escribir muchos módulos adaptadores para que ambas aplicaciones se comunicaran entre sí y realizaran las conversiones de protocolo necesarios.

Para superar estos y futuros problemas de integración, la gerencia de TI de **Smith and Co.** decidió adoptar **SOA** en su metodología de desarrollo de aplicaciones comerciales y convertir el sistema de procesamiento de seguros en una solución orientada a servicios.

Como ***primer paso***, se construyeron muchos wrapper services (web services que encapsulan la lógica de diferentes módulos de procesamiento de seguros), exponiéndolos como web services. Por lo tanto, los módulos individuales pudieron comunicarse entre sí con preocupaciones mínimas de integración. ***Al adoptar SOA, sus aplicaciones usaban un lenguaje común, XML, en la transmisión de mensajes*** y, por lo tanto, sistemas heterogéneos como el sistema de manejo de pólizas de seguro basado en .NET en **Smith and Co.** pudo comunicarse con las aplicaciones basadas en Java que se ejecutan en **InsurePlus Inc**.

Al implementar una solución orientada al servicio, el sistema de **Smith and Co.** pudo fusionarse con muchos otros sistemas heredados con una sobrecarga de integración mínima.

#### Bloques de construcción de SOA

Al estudiar soluciones típicas orientadas a servicios, podemos identificar tres bloques de construcción principales de la siguiente manera:

* Web services
* Mediation(Mediación)
* Composition(Composición)

##### Web Services

***Los web services son las unidades individuales de lógica de negocio en SOA***. Los web services se comunican entre sí y con otros programas o aplicaciones mediante el envío de mensajes. Los web services consisten en una interfaz pública definición que es una pieza central de información que asigna una identidad al servicio y permite su invocación.

El **service container** es el componente SOA middleware donde se aloja el web service para que las aplicaciones que lo consumen interactúen con él. Permite a los desarrolladores crear, implementar y administrar web services y también representa la función del procesador del lado del servidor en los frameworks de web services. Una lista de usos comunes de los frameworks de web services se pueden encontrar en http://en.wikipedia.org/wiki/List_of_web_service_frameworks; aquí puede encontrar algunos web service middleware populares como **Windows Communication Foundation ( WCF )**, **Apache CXF**, **Apache Axis2**, etc. Usaremos **Apache Axis2** como contenedor de servicios para proyectos de muestra dentro del contexto de este libro. **Apache Axis2** se puede encontrar en http://axis.apache.org/ .

El **service container** contiene la **business logic**, que interactúa con el **service consumer** a través de una **service interface**.Esto se muestra en el siguiente diagrama:

![image](https://github.com/adolfodelarosades/Java/assets/23094588/e816edc2-e3ae-43fa-b8cd-df8743cb6eb5)


##### Mediation(Mediación)

Por lo general, la transmisión de mensajes entre nodos en una solución orientada a servicios no solo ocurre a través de los típicos canales punto a punto(point-to-point channels). En cambio, una vez que se recibe un mensaje, puede fluir a través de múltiples intermediarios y someterse a diversas transformaciones y conversiones según sea necesario. Este comportamiento se conoce comúnmente como mediación de mensajes y es otro bloque de construcción importante en soluciones orientadas a servicios. Similar a cómo se utiliza el contenedor de servicios como plataforma de alojamiento para web services, un intermediario es el SOA middleware component correspondiente para la mediación de mensajes. Por lo general, **enterprise service bus (ESB)** actúa como intermediario en soluciones orientadas a servicios.

##### Composition(Composición)

En las soluciones orientadas a servicios, no podemos esperar que los web services individuales se ejecuten solos para proporcionar la funcionalidad empresarial deseada. En cambio, múltiples web services trabajan juntos y participan en varias composiciones de servicios. Por lo general, los web services se reúnen dinámicamente en el tiempo de ejecución según las reglas especificadas en las definiciones de procesos comerciales. La gestión o coordinación de estos procesos de negocio se rigen por el **process coordinator(coordinador de procesos)**, que es el SOA middleware component asociado a las composiciones de web services.

Examinamos los componentes básicos principales de las soluciones orientadas a servicios y los SOA middleware component correspondientes. A continuación, vamos a discutir algunos de los elementos distinguidos asociados específicamente con los web services. Estos son mensajería SOAP messaging, **Web Services Description Language (WSDL)**, message exchanging patterns, y RESTful services.

## Simple Object Access Protocol

**Simple Object Access Protocol ( SOAP )** puede considerarse como el principal estándar de mensajería para su uso con web services. Está definido por el **World Wide Web Consortium (W3C)** en http://www.w3.org/TR/2000/NOTE-SOAP-20000508/ de la siguiente manera:

   ***SOAP es un protocolo ligero para el intercambio de información en un entorno distribuido y descentralizado. Es un protocolo basado en XML que consta de tres partes: un envelope que define un framework para describir el contenido de un message y cómo procesarlo, un conjunto de reglas de codificación para expresar instancias de tipos de datos definidos por la aplicación y una convención para representar llamadas y respuestas a procedimientos remotos.***

La especificación SOAP ha sido universalmente aceptada como el protocolo de transporte estándar para mensajes procesados por web services. Hay dos versiones diferentes de la especificación SOAP y ambas se utilizan ampliamente en soluciones orientadas a servicios. Estas dos versiones son **SOAP v1.1** y **SOAP v1.2.**

Independientemente de la versión de la especificación SOAP, el formato de mensaje de un SOAP message permanece intacto. Un SOAP message es un documento XML que consta de un SOAP envelope obligatorio, un SOAP header opcional y un SOAP body obligatorio.

La estructura de un SOAP message se muestra en el siguiente diagrama:

![image](https://github.com/adolfodelarosades/Java/assets/23094588/421c339d-cdfa-4b1f-9f06-308cf0e6e258)

***El SOAP Envelope es el elemento contenedor que contiene todos los child nodes dentro de un SOAP message.***

***El elemento SOAP Header es un bloque opcional donde se almacena la metainformación.*** Usando los headers, los SOAP messages pueden contener diferentes tipos de información complementaria relacionada con la entrega y el procesamiento de mensajes. Esto proporciona indirectamente la ausencia de estado para los web services, ya que al mantener los SOAP headers, los servicios no necesariamente necesitan almacenar la lógica específica del mensaje. Por lo general, los SOAP headers pueden incluir lo siguiente:

* Instrucciones de procesamiento de mensajes
* Metadatos de la política de seguridad
* Información de direccionamiento
* Datos de correlación de mensajes
* Metadatos de mensajería fiables

***El SOAP body es el elemento donde se alojan los contenidos reales del mensaje. Estos contenidos del body se conocen generalmente como el message payload.***

Echemos un vistazo a un SOAP message de muestra y relacionemos los conceptos anteriores a través del siguiente diagrama:

![image](https://github.com/adolfodelarosades/Java/assets/23094588/65106acd-10da-4db1-9bee-bd9c4800ebc2)

En este ejemplo de SOAP message, podemos identificar claramente los tres elementos; envelope, body y header. El elemento header incluye un conjunto de child elements como **`<wsa:To>`**, **`<wsa:ReplyTo>`**, **`<wsa:Address>`**, **`<wsa:MessageID>`** y  **`<wsa:Action>`**. Estos bloques header forman parte de la especificación **WS-Addressing**. De manera similar, cualquier elemento header asociado con las especificaciones **`WS-*`** se puede incluir dentro del elemento SOAP header.

El elemento **`<s:Body>`** lleva la carga útil del message payload. En este ejemplo, es el elemento **`<p:echoString>`** con un child element.

<hr>

**TIP**

Cuando se trabaja con SOAP messages, la identificación de la versión del SOAP message es uno de los requisitos importantes. A primera vista, puede determinar la versión de la especificación utilizada en el SOAP message a través del identificador de espacio de nombres del elemento **`<Envelope>`**. Si el mensaje cumple con la especificación SOAP 1.1, sería http://schemas.xmlsoap.org/soap/envelope/ ; de lo contrario, http://www.w3.org/2003/05/soap-envelope es el identificador del espacio de nombres de SOAP 1.2 messages.
  
<hr>
  
## Alternativas a SOAP

Aunque **SOAP** se considera el protocolo estándar para la comunicación de servicios web, no es el único protocolo de transporte posible que se utiliza. SOAP fue diseñado para ser extensible para que los otros estándares pudieran integrarse en él. Las extensiones **`WS-*`** como **WS-Security**, **WS-Addressing** y **WS-ReliableMessaging** está asociado con la SOAP messaging debido a esta naturaleza extensible. Además del lenguaje agnosticismo de la plataforma, los SOAP messages se pueden transmitir a través de varios transportes, como **HTTP**, **HTTPS**, **JMS** y **SMTP**, entre otros. Sin embargo, existen algunos inconvenientes asociados con la SOAP messaging. Las degradaciones de rendimiento debidas al procesamiento pesado de XML y las complejidades asociadas con el uso de varias especificaciones **`WS-*`** son dos de las desventajas más comunes del modelo de SOAP messaging. Debido a estas preocupaciones, podemos identificar algunos enfoques alternativos a SOAP.

### REST

Debido a las complejidades que acompañan al modelo **SOAP**, la arquitectura **Representational State Transfer (REST)** ha surgido como resultado. Los **RESTful web services** se pueden considerar como una alternativa ligera a los voluminosos y complejos estándares de servicios web basados en **SOAP**. En los **RESTful web services**, el énfasis está en la comunicación point-to-point a través de **HTTP**, principalmente utilizando **plain old XML (POX)** messages. Analizaremos los **RESTful web services** en detalle en el Capítulo 8, ***Getting started with REST Testing***.

### Java Script Object Notation

La **Java Script Object Notation ( JSON )** es un formato ligero de intercambio de datos similar a XML. Se basa en un subconjunto del lenguaje **JavaScript**. **JSON** utiliza pares de  key-value para representar los datos que se transportan dentro del mensaje. El siguiente ejemplo muestra cómo se puede representar la  XML payload de un SOAP message en JSON:

![image](https://github.com/adolfodelarosades/Java/assets/23094588/f77abe4d-5a60-4162-a034-70196e42df00)

El formato JSON correspondiente del XML payload está representada por:

![image](https://github.com/adolfodelarosades/Java/assets/23094588/9e2782fa-8f9f-4b7b-868a-fd71603cc1f7)

Puede consultar http://www.json.org para obtener más detalles sobre JSON.
  
## Web Services Description Language WSDL - Lenguaje de Descripción de Web Services
  
Según la especificación WSDL 1.1, WSDL esdefinido como:

   ***WSDL es un formato XML para describir los network services como un conjunto de endpoints que operan en mensajes que contienen información orientada a documentos o a procedimientos. Las operaciones y los mensajes se describen de forma abstracta y luego se vinculan a un protocolo de red y un formato de mensaje concretos para definir un endpoint. Los endpoints concretos relacionados se combinan en endpoints abstractos (servicios).***

En términos simples, WSDL proporciona una definición formal del web service a través de definiciones abstractas y concretas de la interfaz. El siguiente diagrama muestra la estructura principal de un documento WSDL:

![image](https://github.com/adolfodelarosades/Java/assets/23094588/82d31b91-9511-4273-9b18-cd209b1e3048)

WSDL es un documento XML con un elemento **`<definitions>`** en la root y los child elements, **`<types>`**, **`<message>`**, **`<portType>`** y **`<binding>`**. Estos se pueden explicar de la siguiente manera:

* El elemento **`<types>`** se utiliza para definir los tipos de datos utilizados por el web service, generalmente a través de un XML schema. El schema se puede definir en línea como un child element **`<types>`** o se puede importar desde una URL externa.

* El elemento **`<message>`** define una representación abstracta de todos los mensajes utilizados por el web service. Un mensaje consta de partes lógicas, cada una de las cuales está asociada con una definición dentro de algún tipo en el XML schema del web service. La siguiente imagen es un ejemplo de un message:

![image](https://github.com/adolfodelarosades/Java/assets/23094588/ce7255a5-458d-47ed-86ef-46b607b2fd80)


* El elemento **`<portType>`** es una representación abstracta de las operaciones y los patrones de intercambio de mensajes utilizados en el web service. Las operaciones representan una acción específica realizada por un web service y que puede estar relacionado con los métodos públicos utilizados por un programa. Las operaciones tienen parámetros de entrada y salida y estos se representan como messages. Por lo tanto, una operación consta de conjuntos de mensajes de entrada y salida. Esto es evidente en la siguiente imagen:

![image](https://github.com/adolfodelarosades/Java/assets/23094588/e65955ab-78d0-4ba7-82dd-ee6c513bf315)

En el ejemplo anterior, el elemento **`SampleServicePortType`** incluye un single child element, **`<wsdl:operation name="echoString">`**, que a su vez incluye dos child elements para definir los mensajes de entrada y salida procesados por la operación **`echoString`**.

* El elemento **`<binding>`** conecta la web service interface abstracta definida por los elementos **`<portType>`** y **`<message>`** en un protocolo de transporte físico. Un binding (enlace) representa una tecnología de transporte particular que el servicio utiliza para comunicarse. Por ejemplo, **SOAP v1.1** es uno de esos enlaces de uso común.

Hablaremos sobre el WSDL en detalle en el Capítulo 2, *El Sample Project*, usando el que se usa en el proyecto de muestra.
  
## Message exchanging patterns - Patrones de Intercambio de Mensajes

Como ya hemos comentado, los web services se comunican entre sí y con los demás programas mediante el envío de mensajes. Si consideramos dos SOAP processing nodes, el patrón de comunicación entre las dos entidades se puede definir como un **message exchanging pattern (MEP)**. Los principales patrones de intercambio de mensajes son:

* Request-response
* Fire and forget

En un patrón Request-response, cuando una entidad de origen (service requester - solicitante de servicio) transmite un mensaje a un destino (service provider - proveedor de servicio), el proveedor debe responder al solicitante. Este es el patrón de intercambio de mensajes más utilizado y lo usaremos en la mayoría de los ejemplos de este libro.

En el siguiente diagrama, un service requester envía un SOAP request message a un service provider:

![image](https://github.com/adolfodelarosades/Java/assets/23094588/7bd13a59-001c-4110-84ea-959a98b2d4d4)


Al recibir el SOAP request message, el service provider responds con una SOAP response como se muestra en el siguiente diagrama:

![image](https://github.com/adolfodelarosades/Java/assets/23094588/ecf01816-cfcf-44cf-875f-3426bc89bfa9)

Cuando no se espera una response a un request message de un web service (o service provider - proveedor de servicios), se conoce como un patrón de intercambio fire and forget message. Por ejemplo, si enviamos una request de ping a un web service, no esperamos un response message.
  

## SOAP Faults - Errores de SOAP

Antes de concluir nuestra discusión sobre los web services y los conceptos asociados, debemos analizar el mecanismo de manejo de fallas de los web services. Los web services pueden devolver fallas debido a varias razones. Por ejemplo, si el request message no se ajusta al XML schema del web service, el servicio responde con un error de SOAP. El elemento de error de SOAP se utiliza para transportar dichas fallas ocurrieron durante la comunicación del web service. Este elemento debe incluirse dentro del cuerpo de un mensaje SOAP. Un mensaje de error típico de SOAP 1.1 consta de los siguientes elementos secundarios:

* **`faultcode`**: El elemento **`faultcode`** se utiliza para definir el tipo de fallo. Por ejemplo, si el problema de la transmisión del mensaje se debe al servidor, el código de falla asociado es **`Server`**. Del mismo modo, podemos usar códigos de error **`VersionMismatch`**, **`MustUnderstand`** y **`Client`** según corresponda.
* **`faultstring`**: El elemento **`faultstring`** tiene la intención de proporcionar una explicación legible por humanos sobre la falla.
* **`faultactor`**: El elemento **`faultactor`** proporciona una indicación sobre la parte responsable que provocó la falla en la  message path.
* **`detail`**: El elemento **`detail`** se utiliza para transportar información de error específica de la aplicación relacionada con el body element. Por ejemplo, si el web service no puede procesar la carga útil de la SOAP request, la respuesta asociada debe incluir el detail element dentro de SOAP Fault.

En el caso de la mensajería SOAP v1.2, **`faultcode`** cambia el nombre a **`Code`** y **`faultstring`** se cambia el nombre a **`Reason`**. Además de eso, un mensaje de error de SOAP v1.2 puede incluir los optional child elements, **`Node`**, **`Role`** y **`Detail`**. Puede encontrar una explicación detallada de las fallas de SOAP 1.1 en http://www.w3.org/TR/2000/NOTE-SOAP-20000508/#_Toc478383507 . Las fallas de SOAP 1.2 se explican en detalle en http://www.w3.org/TR/soap12-part1/#soapfault .
  
## Enfoques de prueba de web services
  
Discutimos un conjunto de conceptos más asociados con los web services. Ahora es el momento de analizar los aspectos de prueba de los web services. Como notamos, los web services son componentes autónomos y poco acoplados que son unidades individuales de lógica de negocio en SOA. Esto facilita un enfoque distinguido para probar web services. Debido a la naturaleza débilmente acoplada, los servicios no mantienen estrechas dependencias entre servicios entre sí. Por lo tanto, una vez que se implementa un web service en particular, se puede probar independientemente de los demás.

Esto brinda a los probadores la capacidad de seguir una metodología de prueba a nivel de componente. Antes de pasar a varias integraciones, se puede probar un web service para verificar los requisitos funcionales y no funcionales. Una vez que el servicio se mejora con diferentes atributos, como políticas de seguridad, dicho servicio también se puede probar individualmente para garantizar que funcione correctamente antes de tener en cuenta los escenarios de integración. Esto brinda una gran flexibilidad para los evaluadores y brinda agilidad a los procesos de prueba.

Podemos identificar un conjunto de enfoques comunes para probando los web services de la siguiente manera:

Examen de la unidad
Pruebas funcionales de web services.
Pruebas de integración de web services
Pruebas de rendimiento

* Unit testing(Pruebas unitarias) de web services
* Functional testing(Pruebas funcionales) de web services
* Integration testing(Pruebas de integración) de web services
* Performance testing(Pruebas de rendimiento) de web services

Analicemos cada uno de estos enfoques en detalle.

### Unit testing(Pruebas unitarias) de web services

Un web service es una unidad de lógica de negocio y consta de una o más operaciones. Estas operaciones deben probarse individualmente para asegurarse de que el web service aborde los problemas comerciales previstos. Por lo tanto, al igual que los métodos individuales en un programa de computadora se prueban como unidades, las operaciones del web service también deben probarse como unidades. Las pruebas unitarias se pueden desarrollar utilizando el marco de pruebas unitarias asociado con el lenguaje de programación que se utiliza para implementar los web services. Por ejemplo, si los web services están escritos en Java, el **framework JUnit** se puede usar como framework de pruebas unitarias. Generalmente, es responsabilidad del autor del web service escribir una cantidad suficiente de pruebas unitarias para cubrir la lógica de las operaciones del web service.

### Pruebas funcionales de web services

Una vez que un web service se implementa en un contenedor de servicios, se somete a una verificación funcional integral. El propósito de la prueba funcional de un web service es para garantizar que el web service proporcione la funcionalidad comercial esperada. Hay muchos enfoques para realizar pruebas funcionales como se explica a continuación.

#### Pruebas asistidas por herramientas

El objetivo principal de usar herramientas para la prueba de web services es admitir la generación y el envío automático de solicitudes de web services. Como la interfaz del web service es un documento XML legible por máquina, no es una tarea fácil leer el WSDL y derivar pruebas manualmente. Por lo tanto, se pueden utilizar herramientas para apuntar al WSDL y generar automáticamente las requests correspondientes, para que los probadores puedan enviarlas al servicio con o sin alteraciones. soapUI es un buen ejemplo de una herramienta de prueba de este tipo, que se puede utilizar en las pruebas funcionales de los web services.

#### Uso de las API de cliente proporcionadas por el middleware del contenedor de servicios

La vida de un web service viene dada por el middleware del contenedor de servicios donde se aloja el servicio. Por lo general, los proveedores de middleware envían las bibliotecas de API de cliente asociadas que se pueden usar para invocar web services mediante programación sin usar ninguna herramienta de terceros.

### Integration testing(Pruebas de integración) de web services
  
Los web services esencialmente no se ejecutan solos. En cambio, están integrados con múltiples componentes, como brokers o service coordinators. Una vez que un servicio se integra o se une a otro componente, debemos realizar pruebas para asegurarnos de que dichas integraciones no rompan el sistema. Por ejemplo, en una solución orientada a servicios, si una aplicación de consumidor de servicios envía un mensaje a un web service pero el mensaje no se ajusta al esquema anunciado del web service. En este caso, el web service suele responder con un fallo de SOAP. Sin embargo, si queremos tomar dicha request y transformar el request SOAP message para que sea válido de acuerdo con el esquema, entonces no queremos pedir a los consumidores de nuestro web service que cambien las aplicaciones cliente a medida que se modifica el esquema del servicio. Este tipo de transformación de mensajes se logra mediante el uso de un broker component(componente intermediario), en otras palabras, enterprise service bus (ESB) middleware. Según las reglas de transformación definidas en el  enterprise service bus, la request se convierte al formato correcto y se reenvía al web service. Este es un tipico ejemplo de integración de web services. Para probar este tipo de integración, el request message debe reenviarse al componente ESB en lugar de enviarlo directamente al web service. Las herramientas como soapUI se pueden usar fácilmente para enviar los mensajes a las ubicaciones de destino deseadas de manera adecuada.

### Performance testing(Pruebas de rendimiento) de web services.
  
Una vez que estamos satisfechos con los aspectos funcionales del web service, debe probarse exhaustivamente el rendimiento. Esto incluye pruebas de carga y estrés del web service, así como la medición del rendimiento bajo varias condiciones Podemos utilizar varias herramientas comerciales o de código abierto en las pruebas de rendimiento de los web services. **Apache JMeter**(que se encuentra en http://jmeter.apache.org/ ) es un buen ejemplo de una herramienta de prueba de código abierto que se puede usar para probar web services. Las pruebas funcionales que creamos en soapUI se pueden ampliar fácilmente para probar el rendimiento de los web services. Discutiremos las capacidades de prueba de rendimiento de soapUI en detalle en el Capítulo 5, ***Load and Performance Testing with soapUI.***

### Los desafíos comunes de los Web services testing
  
En comparación con los enfoques de prueba tradicionales, existen algunos desafíos únicos asociados con las web services testing.

#### Uso de web services externos
  
La naturaleza autónoma y débilmente acoplada de los web services introduce un mayor nivel de escalabilidad y extensibilidad al sistema. Todos los servicios incluidos en un sistema no necesariamente se construyen internamente. Algunos web services pueden ser desarrollados y alojados por terceros. Estos servicios pueden ser descubierto y utilizado dinámicamente de acuerdo con los requisitos del negocio. Si bien esto acelera la entrega de soluciones, probar un sistema de este tipo se vuelve complejo porque la garantía de calidad y la disponibilidad de los servicios de terceros están fuera de su control.

#### Implicaciones del uso de estándares y protocolos complejos
  
Los web services, especialmente los servicios basados ​​en SOAP, pueden usar varias especificaciones **`WS-*`**. Cuando se prueban web services que se adhieren a especificaciones como **WS-Security**, el evaluador debe poseer una buena cantidad de conocimiento sobre los estándares y conceptos para llevar a cabo las pruebas de manera efectiva. Esto introduce una curva de aprendizaje más alta para que los evaluadores comiencen a probar los web services.

Los web services también se pueden exponer a través de múltiples protocolos de transporte. Por lo tanto, las pruebas no se limitan a un transporte en particular, como **HTTP**. Se puede acceder al mismo web service a través de transportes como **JMS** o **VFS**, lo que requiere cambios en la configuración de la prueba, así como un conjunto diferente de escenarios de prueba.

#### Naturaleza Headless de los web services
  
En las pruebas de aplicaciones web tradicionales, los escenarios de prueba se pueden identificar con bastante facilidad estudiando la GUI de los componentes. Como discutimos anteriormente, las operaciones de los web services están expuestas al mundo exterior a través de contratos de servicio legibles por máquina (como WSDL). Por lo tanto, durante las primeras etapas del desarrollo de web services, los evaluadores deben usar WSDL como referencia para la derivación de escenarios de prueba, lo que puede ser difícil en comparación con la exploración de una GUI.

A medida que avancemos con los capítulos de este libro, aprenderemos cómo soapUI aborda algunos de los desafíos antes mencionados y facilita la vida de un probador de web services.

Hemos discutido los fundamentos de SOA y las pruebas de web services. Ahora, estamos listos para explorar el mundo de las pruebas de web services con soapUI.
  
## ¿Qué es SoapUI?
  
El objetivo principal de diseñar herramientas de prueba es ayudar a las personas a probar el software reduciendo el tiempo que lleva la ejecución de la prueba. Hay diferentes tipos de herramientas que se pueden utilizar para pruebas funcionales y no funcionales. Algunas de las herramientas están diseñadas para automatizar las interacciones basadas en la interfaz de usuario y otras se utilizan para derivar automáticamente varios tipos de mensajes de solicitud y transmitirlos a las aplicaciones con o sin modificaciones. Algunas herramientas admiten ambos aspectos.

**SoapUI** es una herramienta que se puede utilizar tanto para pruebas funcionales como no funcionales. No se limita a los web services, aunque es la herramienta de facto utilizada en las pruebas de web services. En las pruebas de web services, soapUI es capaz de desempeñar el papel de cliente y servicio. Permite a los usuarios crear pruebas funcionales y no funcionales de forma rápida y eficiente mediante el uso de un único entorno.

***El primer lanzamiento de soapUI (v1.0) fue en octubre de 2005***. *Mientras trabajaba en un proyecto relacionado con SOA, **Ole Lensmer** sintió la necesidad de una herramienta de prueba para respaldar el desarrollo ágil*. Por lo tanto, comenzó a desarrollar soapUI en su tiempo libre. Eventualmente, el proyecto fue de código abierto y la comunidad creció. Desde entonces, se han lanzado varias versiones con varias funciones y mejoras nuevas, y ***la versión más reciente de soapUI es la 4.0.1 al momento de escribir este libro***. (***A fecha del 04/07/2023 la versión es Version 5.7.0***).

El creador de **soapUI**, **Ole Lensmer**, estuvo administrando los lanzamientos del proyecto a través de una empresa llamada **Eviware** durante los últimos años. En julio de 2011, SmartBear Software adquirió Eviware ( http://smartbear.com/ ) y ***ahora soapUI forma parte de SmartBear Software***.

**soapUI** es una utilidad gratuita y de código abierto, lo que significa que puede utilizar las diversas funciones proporcionadas por la herramienta libremente tan bien como usted permite realizar modificaciones en el código fuente de soapUI y adaptarlo de acuerdo con sus requisitos. soapUI tiene licencia bajo los términos de GNU **Lesser General Public License (LGPL)**. Se ha implementado únicamente utilizando la plataforma Java, por lo que se ejecuta en la mayoría de los sistemas operativos listos para usar.

Cabe señalar que soapUI también se distribuye como una versión comercial no gratuita conocida como **soapUI Pro**, que básicamente brinda a los usuarios utilidades personalizadas y capacidades de prueba de nivel de producción mejoradas. Todas nuestras discusiones y ejemplos se basan en la versión gratuita de soapUI para su conveniencia.

**NOTA**

soapUI v4.0.1 era la versión más nueva en el momento de escribir el libro. Por lo tanto, se utiliza en todo el contexto de este libro. Sin embargo, no discutiremos ningún tema específico de la versión, por lo que las versiones anteriores 3.x de soapUI también se pueden usar para probar los proyectos de muestra y las demostraciones.
  
## Capacidades de soapUI
  
El objetivo principal de los autores de soapUI es brindar a los usuarios una utilidad simple y fácil de usar que se pueda usar para crear y ejecutar pruebas funcionales y no funcionales a través de un solo entorno de prueba. Sobre la base de ese objetivo, soapUI se ha convertido en la herramienta de prueba de web services y SOA líder en el mundo. soapUI se puede instalar sin sobrecarga de configuración en la mayoría de los sistemas operativos que permiten a los usuarios comenzar a utilizar la herramienta sin perder tiempo configurando varios requisitos previos de instalación.

Al usar la GUI basada en Java Swing fácil de usar, puede comenzar a crear pruebas funcionales sin codificación. Eventualmente, las mismas pruebas funcionales se pueden usar para las pruebas de carga y rendimiento a través del mismo entorno de prueba. Esto brinda a los usuarios una gran flexibilidad ya que todas las pruebas funcionales y no funcionales se pueden administrar a través de un único punto de acceso.

Veamos algunas de las características importantes de soapUI que estamos planeando discutir en los siguientes capítulos.

* **Cobertura completa de los aspectos funcionales de los web services y las aplicaciones web**: soapUI admite la mayoría de los estándares utilizados en las aplicaciones web, como la transmisión de mensajes a través de **HTTP**, el **transporte HTTPS** y **JMS**. También admite pruebas de web services **SOAP** y **RESTful**. Específicamente, soapUI admite la mayoría de las especificaciones de web services, como **WS-Security**, **WS-Addressing**, entre otros.

* **Service mocking**: Con los servicios simulados de soapUI, puede simular los web services antes de que se implementen realmente. Esto le brinda la posibilidad de probar las aplicaciones de consumo de web services sin esperar hasta que se implementen los proveedores de web services.

* **Scripting**: Ya sea con **Groovy** o **JavaScript**, soapUI le permite realizar varias configuraciones de prueba previas o posteriores al procesamiento, como respuestas simuladas dinámicas, pruebas de inicialización o limpieza, envío de operaciones simuladas dinámicas, etc.

* **Functional testing(Pruebas funcionales)**: soapUI le permite realizar verificaciones funcionales contra web services, aplicaciones web y fuentes de datos JDBC. Puedes validar las respuestas de tus pruebas utilizando varias aserciones integradas y personalizadas. También le permite agregar pasos de prueba condicionales para controlar el flujo de ejecución de la prueba.
  
* **Performance testing(Pruebas de rendimiento)**: Con solo con unos pocos clics, puede generar pruebas de rendimiento y carga rápidamente usando soapUI.

* **Test automation(Automatización de pruebas)**: soapUI se puede integrar en frameworks de prueba automatizados como **JUnit**, y las pruebas también se pueden iniciar a través de los frameworks de compilación **Apache Maven** y **Apache Ant**. También se puede integrar en *herramientas de integración continua como **Hudson** o **Bamboo***.

Además de las funciones anteriores, la versión propietaria de **soapUI**, **soapUI Pro**, brinda a los usuarios capacidades de prueba basadas en datos, grabación HTTP e informes de prueba que no están dentro del alcance de este libro.
  
## Instalación de la interfaz de usuario de SOAP
  
Examinamos las funciones principales proporcionadas por soapUI y discutimos la herramienta en general. Es hora de explorar la instalación fácil y sencilla de soapUI en algunos de los sistemas operativos populares.

### Requisitos del sistema
  
Para poder ejecutar soapUI, debe tener Java Development Kit ( JDK ) v1.6 ejecutándose en su sistema. Como soapUI se implementa en Java, se ejecuta en muchos sistemas operativos, incluidos Windows XP, Windows Vista, Windows 7, Windows Server 2003, Windows Server 2008, varias versiones de Linux como Ubuntu, Red Hat, Fedora, SuSE y CentOS, y Mac OS X v10.4 o superior.

Podemos resumir los requisitos del sistema para instalar y ejecutar soapUI de la siguiente manera:

![image](https://github.com/adolfodelarosades/Java/assets/23094588/92f77786-bb21-426e-afdd-345b439cf86a)

Analicemos en detalle el procedimiento de instalación de soapUI en cada uno de los sistemas operativos anteriores.

### Instalación de soapUI en Linux

soapUI se distribuye como dos instaladores diferentes para su conveniencia. Puede descargar el archivo binario (ZIP) del instalador o el script del instalador.

Primero, veremos el procedimiento de instalación del archivo binario. Realice los siguientes pasos:

1. Descargue la versión zip binaria de Linux (por ejemplo **`soapui-4.0.1-linux-bin.zip`**) de la última versión de soapUI de http://www.soapui.org.
2. Extraiga la distribución binaria descargada en un directorio de su sistema de archivos local, por ejemplo **`/home/user/soapui`**.

   <hr>
   
   **NOTA**

   Nos referiremos al directorio extraído como **`SOAPUI_HOME`**.

   <hr>

3. Vaya a **`SOAPUI_HOME/bin`** y ejecute el script **`soapui.sh`** de inicio de la siguiente manera: **`./soapui.sh`**. Esto iniciará la interfaz gráfica de usuario de soapUI.

<hr>
   
**TIP**

Si encuentra un error **Permission denied - Permiso denegado** al ejecutar el script **`soapui.sh`**, asegúrese de cambiar el modo de permiso del archivo otorgando privilegios ejecutables al usuario ejecutando el comando **`chmod`** como **`chmod 755 soapui.sh`**.

<hr>

También puede instalar soapUI utilizando el instalador de Linux realizando los siguientes pasos:

1. Descargue un instalador Linux de soapUI (por ejemplo **`soapUI-x32-4_0_1.sh`**) desde http://www.soapui.org.
2. Después de descargar el archivo, otorgue permisos ejecutables ejecutando el comando, **`chmod 755 soapUI-x32-4_0_1.sh`**.
3. Ejecute el instalador de la siguiente manera: **`./soapUI-x32-4_0_1.sh`**.
4. Esto iniciará la interfaz de usuario del instalador como se muestra en la siguiente captura de pantalla:

   ![image](https://github.com/adolfodelarosades/Java/assets/23094588/30029cce-2cad-46d2-a236-02112b18a1f8)


Ahora, puede continuar con el asistente de instalación. Se le pedirá que acepte el contrato de licencia en el siguiente paso del asistente. Simplemente haga clic en **I accept the agreement** y haga clic en **Next**. Se le pedirá que especifique un directorio de destino para la instalación de soapUI.

En el siguiente paso del asistente de instalación, puede seleccionar qué componentes necesita incluir en soapUI, como; **Hermes JMS**, archivos fuente de soapUI y tutoriales. Simplemente acepte todas las opciones y haga clic en Siguiente . Se le solicitará el acuerdo de licencia para los componentes de Hermes en el siguiente paso. Acepte el acuerdo de licencia y haga clic en **Next** para continuar con el asistente. Luego, se le pedirá que especifique un directorio para los tutoriales de soapUI. Ingrese una ubicación que esté en su sistema de archivos y haga clic en **Next**. Se le pedirá un directorio donde se crean enlaces simbólicos de soapUI para ejecutables como el archivo **`soapui.sh`**. Introduzca un directorio y haga clic en **Next**. Puede marcar la casilla de verificación **Create a desktop icon** para crear un icono en el escritorio para que pueda iniciar fácilmente soapUI. Finalmente, haga clic en **Next** botón para iniciar la instalación.

La pantalla de instalación de soapUI se verá como la siguiente captura de pantalla:

![image](https://github.com/adolfodelarosades/Java/assets/23094588/46187bb6-f4a7-4234-95df-9bea2870fad9)

### Instalación de soapUI en Windows

Similar al procedimiento de instalación anterior en Linux, soapUI se puede instalar en un sistema operativo Windows usando Instalador de Windows o archivo binario de Windows.

Veamos los pasos de instalación del archivo binario de Windows. Son los siguientes:

1. Descargue la versión zip binaria de Windows (por ejemplo **`soapui-4.0.1-windows-bin.zip`**) de la última versión de soapUI de http://www.soapui.org.
2. Extraiga la distribución binaria descargada en un directorio de su sistema de archivos local, por ejemplo **`C:/soapui`**.

   <hr>
   
   **NOTA**

   Nos referiremos al directorio extraído como **`SOAPUI_HOME`**. Esto iniciará la interfaz gráfica de usuario de soapUI.

  <hr>

3. Vaya a **`SOAPUI_HOME/bin`** y ejecute el script de inicio **`soapui.bat`** ejecutando el comando: **`soapui.bat`**.

Los pasos para la instalación de soapUI usando el instalador de Windows son casi los mismos que los pasos dados en el instalador de Linux. Solo necesita hacer doble clic en el instalador ( **`soapUI-x32-4_0_1.exe`** ) y se iniciará el asistente de instalación de soapUI.

### Instalación de soapUI en MacOS

La instalación de soapUI en Mac OS es sencillo y similar a los pasos anteriores que describimos para los instaladores de Linux y Windows.
  
## Un vistazo a la interfaz de usuario de soapUI

soapUI es una herramienta de prueba que se explica por sí misma. La interfaz de usuario fácil de usar simplifica el trabajo con soapUI para cualquier tipo de usuario. Con unos pocos clics, puede comenzar a probar un web service o una aplicación web con el mínimo esfuerzo. Esta interfaz de usuario altamente usable y flexible ayudó a soapUI a convertirse en la herramienta de prueba de SOA y web services más fácil de usar y más fácil entre la comunidad de prueba.

Una vez que se inicie soapUI, se le mostrará la interfaz de usuario de inicio como se muestra en la siguiente captura de pantalla:

![image](https://github.com/adolfodelarosades/Java/assets/23094588/55accd9b-12f7-4800-bc49-d7691f9d81d8)


En soapUI, todas las pruebas están organizadas bajo un elemento central, conocido como **Projects**. Simplemente haciendo clic derecho en el nodo **Projects** en el panel lateral izquierdo en la GUI de soapUI, se puede crear un nuevo proyecto de soapUI como se muestra en la siguiente captura de pantalla:

![image](https://github.com/adolfodelarosades/Java/assets/23094588/f46208a8-229e-43c8-b63b-f81d6afeacad)


Dejaré que usted navegue por el resto de los elementos de la interfaz de usuario por su cuenta antes de comenzar con los proyectos de muestra. Encontrarás un montón de materiales en el sitio web oficial de soapUI relacionados con estas características. Exploraremos a través de la interfaz de usuario de soapUI a medida que avancemos a través de las demostraciones y ejemplos en el resto de los capítulos.
  
  
## Resumen
  
Los web services son las unidades individuales de lógica de negocio en SOA. Para probar los web services, debemos poseer un buen conocimiento sobre SOA y los web services, así como los componentes tecnológicos asociados. Este capítulo se ha dedicado a construir esa base.

Comenzamos a buscar en soapUI, la herramienta de prueba de web services y SOA líder y más completa del mundo. Discutimos las metas y objetivos principales del uso de soapUI en las pruebas de web services. Analizamos un poco la historia de soapUI y sus modelos de distribución. Finalmente, se explicaron los pasos para instalar soapUI en Linux, Windows y Mac OS.

Ahora, tenemos SOAPUI ejecutándose en nuestros sistemas. Ensuciémonos las manos con un proyecto de muestra en el próximo capítulo.
  
  
