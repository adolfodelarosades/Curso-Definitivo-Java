# Building and Running a Java SE Application by Using Maven

https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/Maven_SE/Maven.html

## Antes de que empieces

### Objetivo

En este tutorial, creará y compilará una aplicación **Java Platform, Standard Edition (Java SE)** utilizando **Maven**. Usted instala, configura, construye y crea un archivo **Java Archive (JAR)** ejecutable con Maven.

### Tiempo para completar

Aproximadamente 90 minutos

### Fondo

Maven es una herramienta de gestión de compilación fundamental para tareas de compilación de proyectos, como compilación, empaquetado y gestión de artefactos. Maven utiliza un estricto conjunto de reglas basadas en XML para promover la coherencia y al mismo tiempo mantener la flexibilidad. Debido a que la mayoría de los sistemas de integración continua centrados en Java se integran bien con Maven, es una buena opción para un sistema de compilación subyacente.

El objetivo principal de Maven es proporcionar:

* Un modelo de proyecto que es reutilizable, mantenible y más fácil de comprender.

* Complementos o herramientas que interactúan con el modelo declarativo.

La estructura y el contenido del proyecto Maven se declaran en el archivo **`pom.xml`**. El **Project Object Model (POM)** 
 - Modelo de Objetos del Proyecto es la unidad fundamental de todo el sistema Maven.

Maven contiene tres tipos de repositorios para almacenar JARs, complementos y otros artefactos relacionados con el proyecto:

* **Local Repository**: la ubicación creada en su máquina cuando ejecuta su primera instancia de un comando Maven en su máquina.

* **Central Repository**: el repositorio propiedad de la comunidad Maven que contiene bibliotecas de uso común.

* **Remote Repository**: un repositorio personalizado propiedad del desarrollador que contiene bibliotecas y archivos JAR relacionados con el proyecto.

### ¿Qué necesitas?

* Descargue e instale el último [Java SE Development Kit](https://www.oracle.com/java/technologies/downloads/#java8). Para este tutorial, la versión disponible es **Java SE 8**.
* Descargue [Apache Maven](https://maven.apache.org/download.cgi). Para este tutorial, la versión disponible es **3.2.2**.
 
## Configurando el entorno Maven

En esta sección, extrae el archivo descargado e instala la última versión de Maven en el directorio que elija. Usted verifica la instalación de Java, configura el entorno de Maven y verifica la instalación de Maven.

1. Verifique la instalación de Java:

   ```sh
   java -version
   ```

   <img width="830" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/f4fca1f2-b9ee-462c-bf2a-19cd389d8b53">

   El resultado muestra la versión de Java que instaló.

2. Extraiga el archivo Maven x.x.x descargado a un directorio local.

   Los nombres de los archivos son:

   * **Windows OS**: apache-maven-3.2.2-bin.zip

   * **Linux OS**: apache-maven-3.2.2-bin.tar.gz

   * **Mac OS**: apache-maven-3.2.2-bin.tar.gz

   **Nota**: Este OBE le muestra cómo instalar y crear una aplicación Java SE usando Maven en un sistema operativo Windows.

3. Haga clic en **Start**, haga clic con el botón derecho en **Computer** y seleccione **Properties**.

   <img width="869" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/ce3293f2-4c91-484a-ad04-b5e7cf4cd255">

4. En la página de inicio del **Control Panel**, haga clic en **Advanced system settings**.

   <img width="624" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/de96e593-8b91-4dc7-bda1-a862ee3d08bc">

5. En el cuadro de diálogo **System Properties**, haga clic en la pestaña **Advanced** y luego haga clic en **Environment Variables**.

   <img width="669" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/36e5dd62-03ab-4ee3-8470-99fd5febedff">

   Se muestra el cuadro de diálogo **Environment Variables**.

6. Haga clic en **New**, agregue M2, M2_HOME,y MAVEN_OPTS a las variables de entorno y haga clic en **OK**.

   <img width="623" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/26d16b8a-c676-4858-84d6-dfe6901b0922">

7. En **System variables**, haga clic en **New**, ingrese los siguientes valores en el cuadro de diálogo **Edit System variable** y haga clic en **OK**:

   * Variable name: **`Path`**

   * Variable value: **`%M2%`** (Ingrese el valor después de **`bin`** en la ruta del sistema)

   <img width="616" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/fe96868d-2dca-410d-80e1-3e14b34fa0de">

8. Verifique la instalación de Maven:

   ```sh
   mvn -version
   ```

   <img width="843" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/7c32741e-2ddd-48fa-9313-fec80f5020a7">

   El resultado muestra la versión de Maven instalada, la versión de Java, el inicio de Java, la configuración regional predeterminada y el nombre del sistema operativo.

 
## Crear un Java SE Project desde un Maven Template

1. Abra el command prompt, navegue hasta el directorio donde instaló Maven y cree **`Maven_app`**, una carpeta de aplicación Java basada en Maven:

   ```sh
   mvn archetype:generate -DgroupId=com.example.bank
   -DartifactId=OracleBanking
   -DarchetypeArtifactId=maven-archetype-quickstart
   -DinteractiveMode=false
   ```
   
   Un arquetipo es un pattern/model original para crear proyectos similares. En Maven, un arquetipo es una plantilla de un proyecto que se combina con la entrada del usuario para producir un proyecto Maven funcional. La siguiente tabla describe lo que hace la plantilla:

   <img width="980" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/0c465e4d-d254-41ed-8e6a-1d2440d8df56">

   Se crea el Java project llamado **`OracleBanking`**. La siguiente tabla presenta los detalles del proyecto:

   <img width="1264" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/cf51c03b-1d52-41ac-b491-709245e9c6d3">

2. Abra el proyecto **`OracleBanking`** y verifique el archivo fuente de Java:

   **`App.java`**

   <img width="725" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/d3fce2d8-4e68-4ef8-863e-ebae5490097d">

3. Verifique el Java test file:

   **`AppTest.java`**

   <img width="604" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/614c5722-819a-41a7-9525-538ea33bacf3">

   De forma predeterminada, Maven agrega el archivo fuente **`App.java`** y el archivo de prueba **`AppTest.java`** a la estructura de directorio predeterminada.

4. Abra el archivo **`pom.xml`** y revise el código.

   <img width="673" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/2c5af054-cb39-4428-a6c0-a9837911a448">

   Cada proyecto tiene un único archivo **`pom.xml`**, y cada archivo **`pom.xml`** tiene un project element y tres mandatory fields(campos obligatorios): **`groupId`**, **`artifactId`** y **`version`**. Observe que Maven ya ha agregado **JUnit** como test framework. La siguiente tabla describe lo que hace cada nodo:

   <img width="972" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/a517e554-38fd-4d26-90d9-1ac53e15021f">

 
## Crear y Modificar Java Source Files

En esta sección, calculará el interés simple creando el archivo fuente **`SimpleInterest.java`** y modificándolo **`App.java`**.

1. Navegue hasta el directorio donde creó su proyecto Maven y luego abra la ubicación especificada:

   ```sh
   \**\Maven_app\OracleBanking\src\main\java\com\example\bank
   ```

2. Cree un archivo fuente Java llamado **`SimpleInterest.java`**.

   <img width="626" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/07c2af2a-e150-497a-bb70-a750b0608720">

3. Edite el archivo **`SimpleInterest.java`** con el siguiente código:

   ```java
   package com.example.bank;

   public class SimpleInterest{
      public static double calculateSimpleInterest(double amount,
                                                   double years,
                                                   double rate_of_interest) {
         double simple_interest;
         simple_interest = amount * years * rate_of_interest;
         return simple_interest;
      }
   }                                                         
   ```                                                      

   El método **`calculateSimpleInterest`** calcula la tasa de interés sobre el monto del préstamo, la duración del préstamo y la tasa de interés anual.

4. Presione **Ctrl+S** y cierre el archivo.

5. Modificar **`App.java`** con el siguiente código:

   ```java
   double result = SimpleInterest.calculateSimpleInterest(10000, 5, 7);
   System.out.println("The simple interest is:" + result);

   public static double calculateSimpleInterest(double amount,
                                                double years,
                                                double rate_of_interest) {
      double simple_interest;
      simple_interest = amount * years * rate_of_interest;
      return simple_interest;
   } 
   ```

6. Revisa el código. Debería verse como el siguiente:

   <img width="773" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/57a753d0-72ee-4c97-967c-72c8f133872f">


7. Presione **Ctrl+S** y cierre el archivo.

 
## Creando un Manifest con Maven 

En esta sección, aprenderá a utilizar **`maven-jar-plugin`** para crear un archivo de manifiesto y empaquetarlo agregándolo al archivo JAR.

El manifest file realiza las siguientes tareas:

* Define el punto de entrada del proyecto y crea un archivo JAR ejecutable.

* Agrega la ruta de clase de las dependencias del proyecto.

1. Edite el archivo **`pom.xml`**:

   ```xml
   <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <version>2.4</version>
                <configuration>
                    <archive>
                        <manifest>
                            <mainClass>com.example.bank.App</mainClass>
                        </manifest>
                    </archive>
                </configuration>
            </plugin>
        </plugins>
   </build>  
   ```

   Definiste **`maven-jar-plugin`** en **`pom.xml`**, configuraste dentro de la tag **`configuration`**.

2. Revisa el código. Debería verse como el siguiente:

   <img width="678" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/55a89d46-96cb-46d4-ac6f-96d9d7e057aa">

   En el proyecto Maven, usted especifica los detalles de la clase principal actualizando el archivo **`pom.xml`**. La clase **`com.example.bank.App`** en el proyecto es la clase principal que se ejecutará cuando ejecute el archivo JAR.

3. Presione **Ctrl+S** y cierre el archivo.

   Actualizaste exitosamente tu archivo **`pom.xml`**.

 
## Testing, Crear y Ejecutar la aplicación con Maven
 
### Testing la Application

En esta sección, aprenderá cómo probar su aplicación **`AppTest.java`** utilizando la interfaz de línea de comandos (CLI) de Maven.

1. Importar el package en **`AppTest.java`**:

   ```java
   import junit.framework.Assert;
   ```

2. Edite el método **`AppTest`**:

   ```java
   double result=App.calculateSimpleInterest(10000,5,7);
   Assert.assertEquals("Test failed. ",35000.0,result);   
   ```  

3. Revisa el código. Debería verse como el siguiente:

   <img width="606" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/19b606c3-c49d-458a-8e0f-f5ee8f9e3a7a">

   Modificó el valor de interés simple y luego verificó el valor mediante declaraciones de afirmación en el caso de prueba JUnit.

4. Presione **Ctrl+S** y luego ejecute los casos de prueba dentro de **`AppTest.java`** del proyecto **OracleBanking**.

   ```sh
   mvn test
   ```
   
5. Revise el resultado.

   <img width="517" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/9a893491-1d98-4da6-8da9-947954ebbf27">

   El caso de prueba falló y se muestra el mensaje de error de assert.

6. Modificar **`AppTest.java`**:

   ```java
   Assert.assertEquals("Test failed. ", 350000.0, result);
   ```

   <img width="613" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/5d07334f-3a4b-4351-8c14-41a1786361da">

   Modificó el valor de interés simple y luego verificó el valor mediante declaraciones assert en el caso de prueba JUnit.

7. Presione **Ctrl+S** y luego ejecute los casos de prueba en **`AppTest.java`**:

   ```java
   mvn test
   ```

   <img width="654" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/2301df4b-4272-40bd-a7ae-04fef5288794">

   El caso de prueba se ejecutó correctamente y se muestra un mensaje de compilación exitosa.

 
## Construyendo la aplicación

En esta sección, aprenderá cómo limpiar y crear su aplicación utilizando la CLI de Maven.

1. Clean y build su proyecto Maven y revise el resultado:

   ```sh
   mvn clean package
   ```

   <img width="697" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/66d02219-6a30-4af4-8348-880c8805f7c6">

   Construyó exitosamente la aplicación **OracleBanking** Java SE usando Maven.

2. Navegue hasta el directorio donde creó **OracleBanking** y observe que se creó una carpeta **`target`**.

   <img width="463" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/5c6a35c1-4dc4-4d8c-88c1-ea5c97573fa7">

3. Abra la carpeta **`target`** y revise la estructura de la carpeta.

   <img width="1088" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/599d602a-8f8e-4cfa-a829-a34fbcd0a7f0">

 
## Empaquetar y ejecutar la aplicación

En esta sección, aprenderá cómo empaquetar y ejecutar el proyecto Java SE utilizando la CLI de Maven.

1. Navegue hasta el directorio donde instaló Maven y abra el archivo **`settings.xml`**.

   <img width="683" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/28754c1c-5889-4e90-ac23-c5967c0112aa">

   El tag **`<localRepository>`** especifica la ubicación del repositorio local en su máquina. De forma predeterminada, el repositorio local se crea en el directorio **`%USER_HOME%`**. Puede especificar otra ubicación actualizándola en el archivo **`settings.xml`**. Si necesita configurar detalles de proxy para la aplicación, actualícelos en el archivo **`settings.xml`**.

2. Clean y package los archivos, plug-ins y libraries antes de ejecutar la aplicación:

   ```sh
   mvn clean package
   ```

3. Utilice el repositorio local de Maven para ejecutar su aplicación Java SE Maven:

   ```sh
   mvn exec:java -Dexec.mainClass="com.example.bank.App" -s "*****location of settings.xml file.********"
   ```

4. Revise el resultado.

   <img width="684" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/f548b8c8-869a-4f06-a2e6-6257afbedf23">

   Ejecutó con éxito la aplicación Java SE **OracleBanking** usando Maven. El interés simple se calcula y se muestra en Maven CLI.

5. Ejecute el archivo JAR con los siguientes comandos:

   ```sh
   cd target
   java -jar OracleBanking-1.0-SNAPSHOT.jar
   ```

6. Revise el resultado.

   <img width="690" alt="image" src="https://github.com/adolfodelarosades/Java/assets/23094588/d088d684-223c-4d50-9b9f-fa791f15ee7b">

   Ejecutó con éxito la aplicación Java SE **OracleBanking** utilizando Maven. El interés simple se calcula y se muestra en Maven CLI.
 
## ¿Querer aprender más?

* [Maven Quick Guide](https://www.tutorialspoint.com/maven/maven_quick_guide.htm)
* [Maven Getting Started Guide](https://maven.apache.org/guides/getting-started/)
* Installing and Configuring [Maven for Build Automation and Dependency Management](https://docs.oracle.com/middleware/1212/core/MAVEN/config_maven.htm#MAVEN8853) in Oracle Fusion Middleware Developing Applications Using Continuous Integration
 
## Créditos

Curriculum Developer: Shilpa Chetan
