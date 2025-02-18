# 2. Internacionalización y fragmentos 90m

   * 05 Internacionalización basada en ficheros 16:57 
   * 06 Internacionalización basada en bases de datos 16:28 
   * 07 ¿Qué son los fragmentos? 6:46 
   * 08 Definición de fragmentos 16:31 
   * 09 Fragmentos con parámetros 14:57 
   * 10 Integración con AJAX 19:06 
   * Contenido adicional 6
   
# 05 Internacionalización basada en ficheros 16:57 

[PDF Internacionalizacion_basada_en_ficheros.pdf](pdfs/04_Internacionalizacion_basada_en_ficheros.pdf)

## Resumen Profesor

### I18n (internacionalización)

Spring Boot admite mensajes localizados para que nuestra aplicación pueda atender a usuarios de diferentes preferencias de idioma. Por defecto, Spring Boot busca la presencia de un recurso (fichero) llamado `messages` en la raiz del classpath.

Podemos cambiar el nombre base de estos ficheros a través de algunas propiedades:

`spring.messages.basename=messages,config.i18n.messages`

Esta propiedad soporta una lista separada por comas incluyendo una lista de ubicaciones o un paquete.

### Algunos beans necesarios
### `LocaleResolver`

Se trata de una interfaz que nos permite resolver, en un entorno web, cual es el `Locale`, y también modificarlo.

Utilizaremos una de sus implementaciones, `SessionLocalResolver`, que permite establecer el idioma en función de la sesión, estableciendo su valor por defecto en el español de España.

```java
@Bean
public LocaleResolver localeResolver() {
   SessionLocaleResolver sessionLocaleResolver = new SessionLocaleResolver();
   sessionLocaleResolver.setDefaultLocale(new Locale("es", "ES"));
   return sessionLocaleResolver;
}
```

De esta forma, el sistema identifica el idioma por defecto en el que mostrará la aplicación.

### `LocaleChangeInterceptor`

Este bean nos permite establecer la forma en la que se puede cambiar de un idioma a otro. Lo haremos a través del parámetro lang en la URL de la petición.

```java
@Bean
public LocaleChangeInterceptor localeChangeInterceptor() {
   LocaleChangeInterceptor localeChangeInterceptor = new LocaleChangeInterceptor();
   localeChangeInterceptor.setParamName("lang");
   return localeChangeInterceptor;
}
```

De esta forma, si en nuestra url recibimos el parámetro `lang` con otro valor, este interceptor lo capturará y lo cambiará.

`http://localhost:9000/?lang=en`

Para añadir este interceptor (que no deja de ser un filtro) nuestra clase de configuración debe implementar la interfaz `WebMvcConfigurer`, y tenemos que sobrescribir el método `addInterceptors`, para añadir el que acabamos de definir.

```java
@Configuration
public class I18nConfiguration implements WebMvcConfigurer {

    // Resto del código

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(localeChangeInterceptor());
    }
}
```

### Ficheros de internacionalización

El siguente paso es definir qué textos son susceptibles de ser internacionalizados. Mediante esta técnica podremos internacionalizar aquellos que sean estáticos.

*La internacionalización de nuestras entidades (por ejemplo, tener la información de todos nuestros productos en diferentes idiomas) requiere de un modelo de datos mucho más complejo, y escapa del alcance de este curso.*

En nuestro caso, los textos que podemos modificar son:

* Títulos (Openwebinars.net Shop)
* Texto de botones
* Encabezados de tablas
* ...

Para cada uno de ellos, tenemos que pensar en una *etiqueta* que lo identifique. Este proceso es clave, ya que esta etiqueta debe ser común para todos los idiomas.

Por ejemplo:

* `titulo_principal` para *Openwebinars.net Shop*,
* `categoria.nuevo` para el botón de añadir una nueva categoría
* `categoria.imagen` para uno de los títulos de la tabla de categorías
* ...

El fichero con el idioma principal se llamará `messages.properties` (es el nombre por defecto, pero se puede modificar) y lo almacenaremos en `/src/main/resources`. Los ficheros para el resto de idiomas se llamarán `messages_COD.properties`, donde `COD` será el código del idioma.

Si algún texto no se encuentra para un idioma, se utilizará la etiqueta del idioma base.

### Cambios en nuestra plantilla

Thymeleaf nos ofrece un tipo de expresiones para manejar los mensajes, que son `#{...}`. Dentro podemos utilizar el nombre de alguno de los mensajes definidos en los ficheros de *properties*.

Por ejemplo:

```html
<h3 th:text="#{principal.categorias}">Categorías</h3>
```

Para poder cambiar rápidamente de `Locale`, incluimos dos enlaces en la barra superior, para alternar entre español de España (`es_ES`) e Inglés del Reino Unido (`en_GB`).

```html
<li><a th:href="@{''(lang=es_ES)}">Español</a></li>
<li><a th:href="@{''(lang=en_GB)}">Inglés (UK)</a></li>
```

*En una expresión de URL, `@{ }`, el uso de las dobles comillas '' hacen referencia a la URL actual.*

## Transcripción

# 06 Internacionalización basada en bases de datos 16:28 

[PDF Internacionalizacion_basada_en_base_de_datos.pdf](pdfs/05_Internacionalizacion_basada_en_base_de_datos.pdf)

## Resumen Profesor

### I18n basada en base de datos

Los elementos básicos para la internacionalziación con base de datos son los mismos que con ficheros. Necesitamos modificar el modo en que Spring va a rescatar nuestro mensaje en base a su clave, código o identificador.

Por defecto, la implementación que Spring Boot usa para la interfaz `MessageSource` es `ResourceBundleMessageSource` (no es la mejor, pero es la más cercana a la que proporciona directamente Java). **Según esta configuración, Spring Boot espera un bean llamado, exactamente, `messageSource`**. Podemos solapar dicho bean con uno de nuestra cosecha.

Una alternativa a la implementación de la interfaz `MessageSource` es extender la clase `AbstractMessageSource`. Nos evitará tener que implementar todos los métodos, para así centrarnos solo en sobreescribir el método `resolveCode`.

```java
@Component("messageSource")
public class DatabaseDrivenMessageSource extends AbstractMessageSource {        

    @Override
    protected MessageFormat resolveCode(String code, Locale locale) {
        // Ahora completamos este código
    }

}
```

Para manejar los datos de la internacionalización en base de datos, podemos utilizar Spring Data JPA, con sus entidades y repositorios, para hacerlo más fácilmente. Puedes ver el código completo de esta parte en el ejemplo. Debemos seguir los siguiente pasos:

1) Crear una entidad para manejar los datos a internacionalizar.
2) Añadir el esquema en base de datos al script **schema.sql**, y los datos necesarios a **data.sql**.
3) Crear un repositorio para esta entidad, con una consulta que nos devuelva lo que necesitamos:

```java
public interface MensajeI18nRepository extends JpaRepository<MensajeI18n, Long>{
    MensajeI18n findFirstByMessageKeyAndLocale(String messageKey, String Locale);
}
```

La consulta nos buscará, en base a una clave y un locale (esta pareja no debería repetirse) la primera instacia de la entidad. Si no existiera, nos devolverá `null`.

En la implementación del método `resolveCode` de la clase `DatabaseDrivenMessageSource` utilizamos estos elementos para devolver el mensaje en cuestión.

```java
@Component("messageSource")
public class DatabaseDrivenMessageSource extends AbstractMessageSource {

    private static final Logger log = LoggerFactory.getLogger(DatabaseDrivenMessageSource.class);

    @Autowired
    private MensajeI18nRepository repositorio;

    private static final String DEFAULT_LOCALE = "es";

    @Override
    protected MessageFormat resolveCode(String code, Locale locale) {
        MensajeI18n mensaje = repositorio.findFirstByMessageKeyAndLocale(code, locale.getLanguage());
        if (mensaje == null) {
            //Lo buscamos en el idioma por defecto
            mensaje = repositorio.findFirstByMessageKeyAndLocale(code, DEFAULT_LOCALE);
            //Si de nuevo es nulo
            if (mensaje == null) {
                log.debug(String.format("key: %s return null", code));
                return null;
            }
        }

        String valor = mensaje.getMessageValue();

        log.debug(String.format("key: %s -> value: %s", code, valor));

        return new MessageFormat(valor, locale);

    }

}
```

Destacamos algunos elementos del código:

* Añadimos algo de *logging* de forma que si depuramos nuestra aplicacación, podamos ver cómo se está haciendo la búsqueda de claves en base de datos.
* Añadimos un `DEFAULT_LOCALE`. De esta forma, estamos emulando la implementación basada en ficheros, ya que si no localizaba una clave en un fichero de idioma, utilizaba la del fichero del idioma principal.
* Según la especificación del método `resolveCode` este debe devolver una instacia de `MessageFormat` si la clave es localizada, o `null` si no lo es.

Un último apunte: al estereotipar el bean, lo cualificamos con el nombre `messageSource`, para solapar al bean original. Este modo de configurar el bean, que podría chirriar a los más puristas, se puede cambiar, realizando la configuración *manual* de Thymeleaf, de forma que donde se configura el `SpringTemplateEngine` podemos asignarle nuestro `DatabaseDrivenMessageSource`.

```java
@Configuration
public class ThymeleafConfiguration {

    //Resto del código

    @Autowired
    DatabaseDrivenMessageSource messageSource;

    @Bean
    public SpringTemplateEngine templateEngine(){
        SpringTemplateEngine templateEngine = new SpringTemplateEngine();
        templateEngine.setTemplateEngineMessageSource(messageSource);

        //Resto del código

        return templateEngine;
    }

}
```

## Transcripción

# 07 ¿Qué son los fragmentos? 6:46 

[PDF Que_son_los_fragmentos.pdf](pdfs/06_Que_son_los_fragmentos.pdf)

## Resumen Profesor

En nuestras plantillas, hay veces que nos puede interesar incluir partes de otras plantillas, como el encabezado, el pie, el menú... De forma que lo podamos reutilizar.

Para poder realizar esto, necesitamos tener definido algún fragmento mediante el atributo `th:fragment`. Supongamos una plantilla que se llama `pie.html`

```html
<!DOCTYPE html>

<html xmlns:th="http://www.thymeleaf.org">

  <body>

    <div th:fragment="copy">
      &copy; 2011 The Good Thymes Virtual Grocery
    </div>

  </body>

</html>
```

*Ejemplo de la documentación oficial de Thymeleaf*

Si queremos reutilizar este fragmento e incluirlo en otra plantilla, podemos utilizar los atributos:

* `th:insert`: inserta el fragmento en la etiqueta donde se encuentre este atributo.
* `th:replace` reemplaza la etiqueta donde se encuentra este atributo con el fragmento.
* `th:include`: este está deprecado, y se mantiene por compatibilidad con plantillas antiguas. No deberíamos usarlo a partir de Thymeleaf 3.0.

Cualquiera de estos tres atributos esperan una expresión de fragmentos, que tiene la sintaxis `~{...}`.

Si queremos incluir el fragmento anterior en una nueva plantilla, el código sería similar a este:

```html
<body>

  ...

  <div th:insert="~{footer :: copy}"></div>

</body>
```

Podemos comprobar que la expresión de fragmento indica la plantilla en la que se encuentra el fragmento, y tras dos puntos dobles, el nombre del propio fragmento.

*La expresión `~{footer :: copy}` se puede resumir en `footer :: copy`, ya que `~{}` es opcional con `th:insert` y `th:replace`.

Aprenderemos más sobre la sintaxis de fragmentos en las próximas lecciones.

## Transcripción

# 08 Definición de fragmentos 16:31 

[PDF Definicion_y_uso_de_fragmentos.pdf](pdfs/07_Definicion_y_uso_de_fragmentos.pdf)

## Resumen Profesor

### Sintaxis de especificación de fragmentos

La sintaxis para especificar fragmentos es bastante sencilla. Tenemos 3 formatos:

* `~{nombrePlantilla::selector}`: incluye el fragmento que resulta de aplicar el selector `selector` a la plantilla llamada `nombrePlantilla`. Nótese que `selector` puede ser también el nombre de una plantilla.

*La sintaxis de selección de marcado está definida por una librería (que se usa subyacentemente) llamada AttoParser, y es similar a las expresiones XPath o los selectores de CSS.*

* `~{nombrePlantilla}`: incluye de forma completa la plantilla llamada `nombrePlantilla`.

*¡Ojo! El nombre de la plantilla que se utiliza con `th:replace` o `th:insert` debe ser accesible, al igual que lo hacemos con los método en los controladores de Spring.*

* `~{::selector}` o `~{this::selector}`: inserta un fragmento en la misma plantilla.

Tanto el nombre de la plantilla, como el selector, admiten el uso de expresiones y condicionales:

```html
<div th:insert="footer :: (${user.isAdmin}? #{footer.admin} : #{footer.normaluser})"></div>
```

Los fragmentos pueden incluir cualquier etiqueta de thymeleaf. Estas será evaluadas una vez que sean incluidos.

### Estilo inclusivo vs. Estilo jerárquico

El **estilo incluisvo** utiliza el sistema estándar de layout de Thymeleaf (Thymeleaf Standard Layout System); es decir, lo trae por defecto.

El **estilo jerárquico** utiliza el dialecto de layout de Thymeleaf, que no se incluye por defecto. Para poder utilizarlo debemos añadir la siguiente dependencia

```html
<dependency>
    <groupId>nz.net.ultraq.thymeleaf</groupId>
    <artifactId>thymeleaf-layout-dialect</artifactId>
</dependency>
```

*¡OJO! Si no utilizamos Spring Boot, tenemos que indicar la versión de esta dependencia. Además, tenemos que añadir el uso de este dialecto en la configuración de Thymeleaf.*

Además, para poder usarlo, añadimos en namespace correspondiente a nuestras plantillas. Estas deberían quedar de la siguiente forma:

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org"
    xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout">
<head>
...
</head>
<body>
...
</body>
</html>
```

### Algunos elementos del *Thymeleaf Layout Dialect*

Como decíamos antes, el estilo jerárquico utiliza un dialecto específico, el *Thymeleaf Layout Dialect*. Este posee algunos procesadores (atributos) especiales:

* `layout:decorate`: se utiliza en las plantillas de contenido (es decir, en la relación padre-hijo, se usaría en el hijo), y se ubica en la etiqueta `<html>`. Recibe una expresión de fragmento, indicando cual es la plantilla *padre* en la cual nos vamos a basar (en terminología de este dialecto, la plantilla que vamos a decorar).

* `layout:title-patter`: se utiliza en la etiqueta `<title>` y nos permite especificar un patrón para el título de nuestras plantillas de contenido, usando los tokens `$LAYOUT_TITLE` (título del layout padre) y `$CONTENT_TITLE` (título del layout hijo).

* `layout:insert` y `layout:replace`: similar a `th:insert` y `th:replace`, pero permite insertar fragmentos de HTML dentro.

* `layout:fragment`: marca secciones en nuestros layouts o plantillas que pueden reemplazarse por secciones que comparten el mismo nombre.

## Transcripción

# 09 Fragmentos con parámetros 14:57 

[PDF Fragmentos_con_parametros.pdf](pdfs/08_Fragmentos_con_parametros.pdf)

## Resumen Profesor

Thymeleaf permite definir fragmentos como si fueran funciones; es decir, fragmentos definidos con `th:fragment` y que pueden recibir una serie de argumentos.

```html
<div th:fragment="frag(v1, v2)">
    <p th:text="${v1}">...</p>
    <a th:href="@{v2}">...</a>
</div>
```

Esto nos obliga a utilizar alguna de estas dos expresiones a la hora de insertar/reemplazar este fragmento

```html
<div th:replace="::frag(${valor1},${valor2})">...</div>
<div th:replace="::frag(v1=${valor1}, v2=${valor2})">...</div>
```

*La segunda sintaxis, además, nos permite modificar el orden de los argumentos, si es que así lo necesitamos.*

### Validación de argumentos de un fragmento

Thymeleaf nos provee del atributo `th:assert`, muy útil con los fragmentos. Recibe como argumento una lista separada por comas de expresiones booleanas. Todas deben evaluarse y dar como resultado verdadero; en otro caso, se lanza una excepción. Son muy válidas para comprobar los argumentos de un fragmento con parámetros.

```html
<header th:fragment="encabezado(titulo)" th:assert="${!#strings.isEmpty(titulo)}">...</header>
```

### Diseños flexibles

Los fragmentos parametrizables son solo admiten como parámetros valores textuales, números o *beans*. Es muy interesante que también pueden recibir *otros fragmentos*, con lo cual podemos realizar auténticos diseños flexibles. De hecho, podemos crear layouts extensibles sin necesidad de utilizar el *Thymeleaf Layout Dialect*.

Podemos ver este siguiente ejemplo de la documentación oficial de Thymeleaf:

Supongamos un bloque de código en el fichero `base.html`

```html
<head th:fragment="common_header(title,links)">

  <title th:replace="${title}">The awesome application</title>

  <!-- Estilos y scripts comunes -->
  <link rel="stylesheet" type="text/css" media="all" th:href="@{/css/awesomeapp.css}">
  <link rel="shortcut icon" th:href="@{/images/favicon.ico}">
  <script type="text/javascript" th:src="@{/sh/scripts/codebase.js}"></script>

  <!--/* Estilos o elementos adiocionales para una página concreta */-->
  <th:block th:replace="${links}" />

</head>
```

Podemos ver como el fragmento definido, `common_header`, espera dos argumentos, llamados `title` y `links`, de manera que el título será reemplazado con el argumento `title` que proporcionemos, y el bloque final será reemplazado con el argumento `links`.

*Como indicábamos más arriba, los argumentos de un fragmento con parámetros pueden ser a su vez otros fragmentos.*

Este fragmento lo podemos utilizar desde otra plantilla (supongoamos `index.html`) de la siguiente forma:

```html
...
<head th:replace="base :: common_header(~{::title},~{::link})">

  <title>Awesome - Main</title>

  <link rel="stylesheet" th:href="@{/css/bootstrap.min.css}">
  <link rel="stylesheet" th:href="@{/themes/smoothness/jquery-ui.css}">

</head>
...
```

Como podemos apreciar, aquí suceden varias cosas:

* En esta plantilla, el bloque `<head>` se sustituye con el fragmento `common_header` de la plantilla `base.html`.
* Se le proporcionan los dos argumentos esperados.
   * El primer argumento es el fragmento `<title>` de esta misma plantilla.
   * El segundo argumento es el conjunto de todos los `<link>` de esta misma plantilla.
   
*Cabe recordar que a la hora de invocar fragmentos, la sintaxis habitual es `~{plantilla::selector}`, donde si el fragmento está en la misma plantilla, se puede usar `this` u omitir el nombre de la misma; y el selector no tiene por qué ser un fragmento definido con `th:fragment`, sino que pueden ser etiquetas HTML de la plantilla.*

Así, el resultado final que se enviaría al navegador sería:

```html
...
<head>

  <title>Awesome - Main</title>

  <!-- Estilos y scripts comunes -->
  <link rel="stylesheet" type="text/css" media="all" href="/awe/css/awesomeapp.css">
  <link rel="shortcut icon" href="/awe/images/favicon.ico">
  <script type="text/javascript" src="/awe/sh/scripts/codebase.js"></script>

  <link rel="stylesheet" href="/awe/css/bootstrap.min.css">
  <link rel="stylesheet" href="/awe/themes/smoothness/jquery-ui.css">

</head>
...
```

### Fragmento vacío y token de no-operación

En algunos casos, dado el caso de utilizar un fragmento con parámetros, es posible que queramos pasar un **fragmento vacío**, sin contenido de marcado; pensemos por ejemplo, en el caso anterior, que no quisiéramos añadir ningún elemento `<link>` más a los estilos comunes. Podemos utilizar la expresión `~{}`, que denota un fragmento vacío.

```html
...
<head th:replace="base :: common_header(~{::title},~{})">

  <title>Awesome - Main</title>

</head>
...
```

Tendríamos entonces este resultado:

```html
...
<head>

  <title>Awesome - Main</title>

  <!-- Estilos y scripts comunes -->
  <link rel="stylesheet" type="text/css" media="all" href="/awe/css/awesomeapp.css">
  <link rel="shortcut icon" href="/awe/images/favicon.ico">
  <script type="text/javascript" src="/awe/sh/scripts/codebase.js"></script>

</head>
...
```

También podemos utilizar el **token de no-operación** (que también se puede utilizar en otro tipo de expresiones) si queremos dejar que se utilice el valor por defecto de un bloque. Fijémonos en este caso, en el `<title>`:

```html
...
<head th:replace="base :: common_header(_,~{::link})">

  <title>Awesome - Main</title>

  <link rel="stylesheet" th:href="@{/css/bootstrap.min.css}">
  <link rel="stylesheet" th:href="@{/themes/smoothness/jquery-ui.css}">

</head>
...
```

Como en el fragmento `common_header`, teníamos esta etiqueta:

```html
<title th:replace="${title}">The awesome application</title>
```

El resultado sería el siguiente:

```html
...
<head>

  <title>The awesome application</title>

  <!-- Estilos y scripts comunes -->
  <link rel="stylesheet" type="text/css" media="all" href="/awe/css/awesomeapp.css">
  <link rel="shortcut icon" href="/awe/images/favicon.ico">
  <script type="text/javascript" src="/awe/sh/scripts/codebase.js"></script>

  <link rel="stylesheet" href="/awe/css/bootstrap.min.css">
  <link rel="stylesheet" href="/awe/themes/smoothness/jquery-ui.css">

</head>
...
```

### Eliminar fragmentos

Supongamos que queremos maquetar una tabla en la que visualizar los elementos de una lista:

```html
<table>
  <tr>
    <th>Nombre</th>
    <th>PVP</th>
    <th>Valoración</th>
    <th>Disponibilidad de stock</th>
  </tr>
  <tr th:each="prod : ${prods}" th:class="${prodStat.odd}? 'odd'">
    <td th:text="${prod.nombre}">Macbook Pro</td>
    <td th:text="${prod.pvp}">1999</td>
    <td th:text="${prod.valoracion}">4.5/5</td>
    <td th:text="${prod.inStock}? #{si} : #{no}">Sí</td>
  </tr>
</table>
```

Este código es válido como plantilla para procesar una lista de productos pero, ¿qué pasa si queremos maquetar primero sin el procesamiento de Thymeleaf? No sería lo más apropiado, ya que se muestra solamente una fila, y tenemos estilos que se alternan en filas pares o impares. Es decir, necesitamos más filas. Para ello, podríamos hacer lo siguiente:

```html
<table>
  <tr>
    <th>Nombre</th>
    <th>PVP</th>
    <th>Valoración</th>
    <th>Disponibilidad de stock</th>
  </tr>
  <tr th:each="prod : ${prods}" th:class="${prodStat.odd}? 'odd'">
    <td th:text="${prod.nombre}">Macbook Pro</td>
    <td th:text="${prod.pvp}">1999</td>
    <td th:text="${prod.valoracion}">4.5/5</td>
    <td th:text="${prod.inStock}? #{si} : #{no}">Sí</td>
  </tr>
  <tr class="odd">
    <td>Dell XPS 9570</td>
    <td>1500</td>
    <td>4.6/5</td>
    <td>Sí</td>
  </tr>
  <tr>
    <td>Lenovo ThinkPad X1 Carbon Gen 7</td>
    <td>1300</td>
    <td>4.4/5</td>
    <td>No</td>
  </tr>
</table>
```

En este caso ya tendríamos 3 filas, y la tabla parecería más realista como prototipo estático. Pero, ¿y si ahora procesamos con Thymeleaf? Tendríamos tantos productos, como el tamaño de la lista `prods`, además de los dos últimos productos. Ya que las dos últimas filas son parte del prototipo, pero no queremos que aparezcan en el resultado final, *necesitaríamos eliminarlas*. **Esto lo podemos hacer mediante el atributo** `th:remove`.

```html
<table>
  <thead>
    <tr>
      <th>Nombre</th>
      <th>PVP</th>
      <th>Valoración</th>
      <th>Disponibilidad de stock</th>
    </tr>
  </thead>
  <tbody th:remove="all-but-first">
    <tr th:each="prod : ${prods}" th:class="${prodStat.odd}? 'odd'">
      <td th:text="${prod.nombre}">Macbook Pro</td>
      <td th:text="${prod.pvp}">1999</td>
      <td th:text="${prod.valoracion}">4.5/5</td>
      <td th:text="${prod.inStock}? #{si} : #{no}">Sí</td>
    </tr>
    <tr class="odd">
      <td>Dell XPS 9570</td>
      <td>1500</td>
      <td>4.6/5</td>
      <td>Sí</td>
    </tr>
    <tr>
      <td>Lenovo ThinkPad X1 Carbon Gen 7</td>
      <td>1300</td>
      <td>4.4/5</td>
      <td>No</td>
    </tr>
  </tbody>
</table>
```

Esto permitiría que Thymeleaf respete solo la primera línea (que precisamente es la que es capaz de procesar la lista de productos) y elimine las demás. Las opciones de `th:remove` son:

* `all`: Elimina el contenido de la etiqueta y sus etiquetas hija.
* `body`: No elimina el contenido de la etiqueta, pero si elimina toda las etiquetas hija.
* `tag`: Elimina la etiqueta contenedora, pero no elimina las hijas.
* `all-but-first`: Elimina todos los hijos menos el primero.
* `none`: No elimina ninguno. Es útil en caso de una evaluación dinámica a través de una expresión.

### Herencia de layout

Aunque en la lección anterior aprendíamos como usando el *Thymeleaf Layout Dialect* podíamos crear una jerarquía de layouts que eran decorados por plantillas concretas, también es cierto que podemos utilizar los **fragmentos con parámetros para tener una plantilla base, y jugar con los argumentos que reciba para construir nuestras plantillas**.

Supongamos nuestro layout base, definido en `base.html`:

```html
<!DOCTYPE html>
<html th:fragment="layout(titulo, contenido)" xmlns:th="http://www.thymeleaf.org">
<head>
    <title th:replace="${titulo}">Título</title>
</head>
<body>
    <h1>Layout H1</h1>
    <div th:replace="${contenido}">
        <p>Contenido del layout</p>
    </div>
    <footer>
        Pie del layout
    </footer>
</body>
</html>
```

Hemos definido un fragmento que ocupa toda la plantilla (por estar definido en la etiqueta `html`) y que espera dos argumentos, que usará para reemplazar el título y el contenido.

Este puede ser usado en otro documento, por ejemplo, `index.html`, de la siguiente forma:

```html
<!DOCTYPE html>
<html th:replace="~{base :: layout(~{::title}, ~{::section})}">
<head>
    <title>Título de la pagina</title>
</head>
<body>
<section>
    <p>Contenido de la página</p>
    <div>...</div>
    <img src="..." />
</section>
</body>
</html>
```

Aquí suceden varias cosas:

* La etiqueta `<html>` de `index.html` y todo su contenido quedan sustituidas por la etiqueta correspondiente del fichero `base.html`.
* El título de `base.html` se reemplaza con el contenido de `<title>` de `index.html`.
* El contenido (etiqueta `<div>`) de `base.html` queda sustituido por la etiqueta `<section>` (y todo su contenido) de `index.html`.

En definitiva, el resultado procesado sería:

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
     <title>Título de la pagina</title>
</head>
<body>
    <h1>Layout H1</h1>
    <section>
      <p>Contenido de la página</p>
      <div>...</div>
      <img src="..." />
    </section>
    <footer>
        Layout footer
    </footer>
</body>
</html>
```

## Transcripción

# 10 Integración con AJAX 19:06 

[PDF Fragmentos_con_AJAX.pdf](pdfs/09_Fragmentos_con_AJAX.pdf)

## Resumen Profesor

Thymeleaf ofrece la posibilidad de renderizar, en lugar de una plantilla completa, una parte de ella, es decir, un fragmento.

Si se utiliza con Spring, podemos hacer uso del valor de retorno de un controlador:

```html
@RequestMapping("/showContentPart")
public String showContentPart() {
    ...
    return "index :: content";
}
```

## Transcripción

# Contenido adicional 6   

* [PDF Internacionalizacion_basada_en_ficheros.pdf](pdfs/04_Internacionalizacion_basada_en_ficheros.pdf)
* [PDF Internacionalizacion_basada_en_base_de_datos.pdf](pdfs/05_Internacionalizacion_basada_en_base_de_datos.pdf)
* [PDF Que_son_los_fragmentos.pdf](pdfs/06_Que_son_los_fragmentos.pdf)
* [PDF Definicion_y_uso_de_fragmentos.pdf](pdfs/07_Definicion_y_uso_de_fragmentos.pdf)
* [PDF Fragmentos_con_parametros.pdf](pdfs/08_Fragmentos_con_parametros.pdf)
* [PDF Fragmentos_con_AJAX.pdf](pdfs/09_Fragmentos_con_AJAX.pdf)
