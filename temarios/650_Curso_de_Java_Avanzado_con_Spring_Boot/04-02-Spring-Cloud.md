# 02 - Spring Cloud

![04-02-01](images/04-02-01.png)

En esta lección vamos a ver Spring Cloud que es un conjunto de paquetes y dependencias que nos van a permitir resolver todas las problematicas que se nos presentan al realizar una arquitectura de Microservicios.

![04-02-02](images/04-02-02.png)

Spring Cloud consta de distintos proyectos que contienen una serie de características que nos ayudan a resolver un problema concreto de las aplicaciones de MicroServicios.

* **Spring Cloud Config**
   El proyecto **Spring Cloud Config** es el que nos va a permitir crear un servidor de configuración distribuido para el cloud, a este servidor de configuración es donde se van a conectar el resto de MicroServicios para leer las propiedades de configuración especificas y nos va a permitir eso, tener en un mismo servidor de configuración las distintas propiedades de configuración para los distintos MicroServicios y además para cada MicroServicio las distintas propiedades de configuración para los distintos entornos.
   
* **Spring Cloud Netflix**
   El proyecto **Spring Cloud Netflix** que es de Netflix contiene una serie de proyectos que nos van a permitir resolver otra serie de dificultades:
   
   * **Eureka** que es un Servicio de Nombres, un Servicio de Nombres es donde el resto de MicroServicios se van a registrar y a partir de ahí tanto el resto de Microservicios como cuando hagamos peticiones a nuestra aplicación la URL a la que vamos a ir no va a ser directamente la URL del servicio, por ejemplo si el servicio lo levantamos en el http://localhost:8080 no vamos a ir a http://localhost:8080/nombreendpoint que corresponda, sino que si ese servicio tiene un nombre por ejemplo "servicioA" vamos a "servicioA/loquecorresponda" de esta manera lo que hacemos es que ocultamos tras un nombre todas las instancias que haya de ese servicio, por que ese servicio puede tener una instancia en el puerto 8080, otra en el puerto 8081, otra en el 8082 y además esas instancias van a ser dinamicas se van a ir levantando o tirandose automaticamente en función de la carga que tenga la aplicación, por lo tanto nunca vamos a saber realmente que instancias estan realmente arriba, lo podríamos saber pero nos dificultaría las cosas, por eso se utiliza un *Servicio de Nombres* ese *Servicio de Nombres* oculta por así decirlo todas esas instancias tras un nombre y es a través de ese nombre a través del que accedemos a las instancias.
   
   * **Ribbon** que es un balanceador de la carga, es el que se encarga en función de la carga que tengan cada una de las instancias de ese servicio de distribuir esa llamada y decir vale esta llamada viene al "servicioA", el "servicioA" tiene 10 instancias, de estas 10 instancias la que mens carga tiene es "esta", así que llamo la llamada a "esta" instancia del servicio.
   * **Feign** que es un cliente REST, con el cliente REST en lugar de usar el REST Template de toda la vida vamos a utilizar **Feign**, por que **Feign** es un cliente Rest que tiene una serie de integraciones que nos va a facilitar mucho la vida al utilizarlo junto con **Ribbon**, además **Feign** es un cliente REST declarativo, es como cuando veiamos los repositorios de JPA en los que no teniamos que implementar el método, simplemente con definir el método, con declararlo, era suficiente para tenerlo disponible, con **Feign** va a pasar un poco igual, no vamos a tener que hacer la implementación, lo veremos como nos va a facilitar la vida y nos va a ahorrar muchas líneas de código.
   * **Zuul** que es un API Gateway, nos permite tener una especie de Proxy hacia nuestra aplicación, es decir podemos hacer que todas las llamadas que vayan a nuestra aplicación pasen por este API Gateway, como todas las peticiones pasan por este API Gateway, podemos en el implementar la autorización y la autenticación que podemos aplicar a todas las llamadas y las llamadas ya no tendrían ni que llamar a los servicios en el caso de no estar autorizadas o autenticadas.
   
* **Spring Cloud Bus**
   El proyecto **Spring Cloud Bus** nos permite que todos nuestros servicios se suscriban a una cola de mensajería y ser capaces de comunicarnos con todos nuestros Microservicios mediante eventos, por ejemplo si tenemos varios Microservicios y varias instancias de estos Microservicios conectadas al servidor de configuración, si cambiamos la configuración podemos mandar un mensaje con Spring Cloud Bus a todos los Microservicios para decirles que hemos cambiado la configuración y que tiene que actualizarse, en lugar de estar llendo servicio por servicio e informando a cada uno, los servicios simplemente estaran suscritos a esta cola de mensajería y cuando se produzca un evento en la cola de mensajería se mandara un mensaje mediante Spring Cloud Bus para que se actualicen.
   
![04-02-03](images/04-02-03.png)   
  
Veíamos la problematica que teníamos con las aplicaciones con la arquitectura de Microservicios y hemos visto que Spring Cloud viene al rescate para resolver estos problemas, como se muestra en la diapositiva.

Para la Monitorización vamos a usar **Zipkin** y **Zuul** vamos a ver la problematica que tenemos con el trazado distribuido, en aplicaciones distribuidas y como lo podemos solucionar con Zipkin por que tenemos que ser capaces de seguir una petición durante todo el flujo, imaginarse la encadenación de 5 microservicios y falla la petición en el tercer microservicio eso lo vamos a solucionar con **Hystrix** con la tolerancia a fallos, pero si quisieramos seguir todo el flujo de esa petición de un microservicio a otro nos costaria demasiado trabajo, tenríamos que mirar los logs del primer microservicio, averiguar que request es la que se ha hecho, mirar los logs del segundo microservicio y asi con todos, eso con Zipkin vamos a ser capaces de solucionarlo y Zipkin nos va a permitir mirar en un Dashboard ver todo ese flujo de llamadas agrupado como si estuvieramos viendo un mismo flujo, y luego la toleracia a fallos como deciamos si falla el tercer microservicio falla no queremos que toda la aplicación se caiga, lo que queremos es ser capaces de recuperar de ese error y que el fallo de un microservicio no haga que la aplicación entera falle, 

![04-02-04](images/04-02-04.png)  

Este sería un pequeño esquema de algunos de los elementos que vamos a usar, por ejemplo en este caso tendríamos dos microservicios en el cual el primero de ellos SERVICIO A tiene dos instancias y el segundo SERVICIO B tiene una instancia. Nosotros a través de la Interface nos vamos a comunicar a través de RIBBON, vamos a hacer una petición a los servicios y RIBBON con el servidor de nombres va a descubrir a que servicio tiene que redirigir la petición y en función de la carga RIBBON lo va a dirigir.

![04-02-05](images/04-02-05.png) 

¿Cuales son las ventajas de los MicroServicios? 

* Flexibilidad de Tecnologías
   Podemos tener una aplicación con el monton de Microservicios y que un Microservicio este escrito en Java, otro en NodeJS, otro en .NET, por lo que podemos tener un equipo multidisiplinar, podemos tener gente trabajando desde ya, por que no tiene que aprender una tecnología nueva.
   
* Escalado Dinámico 
   Nos puede ayudar a reducir los costes, por si hay una parte de la aplicación poder escalar solo esa parte, o poder desescalar parte de la aplicación cuando tengan muy baja carga es una ventaja muy grande.
   
* Ciclos de Entrega
   Los ciclos de entrega suelen ser más rápidos ya que se puede aislar mucho el trabajo sobre una funcionalidad, sin que tenga dependencias con otros y con eso se pueden hacer pequeñas entregas de valor muy rapidamente por lo que es una arquitectura que es optima para el desarrollo Agile.
