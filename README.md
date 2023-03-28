# JSTL - JSP Standard Tag Library

## Instalación de librerías JSTL manualmente

### Con Apache Tomcat 9 o anterior

Deberás incluir la siguiente librería de JSTL en la carpeta `WEB-INF/lib` de tu proyecto.

> [jstl-1.2.jar](lib-tomcat9/jstl-1.2.jar)

### Con Apache Tomcat 10 o superior

Si usas Tomcat 10 o superior, al usar la OpenJDK de Java, las librerías son las de Jakarta (en lugar de las de javax). Por lo tanto, los archivos a copiar en la carpeta `WEB-INF/lib` son las siguientes (los dos archivos):

> [jakarta.servlet.jsp.jstl-2.0.0.jar](lib-tomcat10/jakarta.servlet.jsp.jstl-2.0.0.jar)

> [jakarta.servlet.jsp.jstl-api-2.0.0.jar](lib-tomcat10/jakarta.servlet.jsp.jstl-api-2.0.0.jar)


## Instalación de librerías JSTL usando Maven

Si estás usando Maven como gestor de dependencias, también deberás tener en cuenta si usas Tomcat 9 o anterior, o Tomcat 10 o superior. A continuación te indicamos las dependencias a añadir al `pom.xml` según la versión de Tomcat.

### Con Apache Tomcat 9 o anterior

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
</dependency>
```



### Con Apache Tomcat 10 o superior

```xml
<!-- Esta dependencia añade la librería JSTL -->
<dependency>
    <groupId>jakarta.servlet.jsp.jstl</groupId>
    <artifactId>jakarta.servlet.jsp.jstl-api</artifactId>
    <version>3.0.0</version>
</dependency>

<!-- Añade esta también si da algún problema en el paso siguiente (ej: IntelliJ) -->
<dependency>
    <groupId>org.glassfish.web</groupId>
    <artifactId>jakarta.servlet.jsp.jstl</artifactId>
    <version>3.0.1</version>
</dependency>
```

> 💡  Aquí se detallan las últimas versiones que se han probado y que funcionan en un proyecto usando Jakarta 10 y Tomcat 10. Si no funciona, prueba a usar versiones más antiguas modificando la versión.




## Pasos a seguir para usar JSTL

Para usar las etiquetas de JSTL, debemos hacer los siguientes pasos:

1. Instalar las librerías tal y como se ha descrito anteriormente.

2. Escribir al principio del todo, en la página JSP donde queramos usar las etiquetas JSTL, la siguiente línea:

   

   ```jsp
   <%@taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
   ```

3. Usar las etiquetas de JSTL tal y como se describen en la documentación siguiente.

## Documentación de JSTL

En esta página, encontrarás toda la información sobre como usar las etiquetas de JSTL.

> http://www.tutorialspoint.com/jsp/jsp_standard_tag_library.htm

A continuación un resumen con ejemplos de las etiquetas más comunes.



## Ejemplos de etiquetas comunes de JSTL

Para sustituir por completo los scriptlets de java en una página JSP, usaremos las etiquetas JSTL, que ofrecen la misma funcionalidad, pero en forma de etiqueta HTML, la cual encaja mucho mejor en una página HTML que los scriptlets.

### Condicionales simples

Para usar un `if`, usaremos la etiqueta `<c:if>`

Ejemplo:

```jsp
<c:if test="${usuario.edad > 18}">
    <div>Eres mayor de edad</div>
</c:if>
```

La etiqueta `<c:if>` tiene un atributo `test`, que es la expresión que se evaluará, en este caso será `${usuario.edad > 18}` y si es verdadera, incluirá el contenido `<div>Eres mayor de edad</div>` en el cuerpo de la página resultante, o no se incluirá nada en caso contrario. 

No hay un `else` en las etiquetas `<c:if>`. Si queremos hacer algo en caso contrario deberemos usar otra etiqueta `<c:if>` con la condición contraria, o bien usar una etiqueta `<c:choose>` con una condición `<c:otherwise>`.

### Condicionales compuestas

Para usar una estructura tipo `switch`, usaremos la etiqueta `<c:choose>`

```jsp
<c:choose>
    <c:when test="${usuarios.size() > 1}">
        usuarios 
    </c:when>    
    <c:otherwise>
        usuario 
    </c:otherwise>
</c:choose>
```

Se pueden añadir tantas etiquetas `<c:when>` como opciones quieras evaluar. Cada una tendrá su propio `test`.

### Bucles 

Para hacer un bucle, tan sólo existe una única etiqueta, que es `<c:forEach>`. Se usará tanto para bucles desde un número hasta otro número, como para recorrer colecciones. 

**Desde un número hasta otro número:**

```jsp
<c:forEach var="i" begin="1" end="5" step="1">
    <p>Elemento nº ${i}</p>
</c:forEach>
```

```
Elemento nº 1
Elemento nº 2
Elemento nº 3
Elemento nº 4
Elemento nº 5
```

Tiene tres atributos, `var` es donde declaramos la variable que se usará como índice, `begin` es su valor inicial (0 es su valor por defecto) y `end` su valor final. Repetirá el contenido que haya dentro de las etiquetas `<c:forEach> </c:forEach>` las veces que sea necesario para que llegue desde el valor inicial  hasta el final. 

El atributo `step` que indica el incremento que se hace en cada vuelta del bucle. Si no se indica el atributo, el valor por defecto es 1. En nuestro ejemplo, podríamos haberlo omitido.

**Colecciones:**

Para recorrer colecciones, también se usa la etiqueta `<c:forEach>` indicando otros atributos, que es `items` en lugar de `begin` y `end`.

Tenemos una colección llamada `usuarios`, y queramos mostrar el nombre de cada uno de ellos en una lista HTML.
```jsp
<ul>
    <c:forEach var="usuario" items="${usuarios}">
        <li>${usuario.nombre}</li>
    </c:forEach>
</ul>
```

```html
<!-- Este sería el resultado de compilar la JSP -->
<ul>
    <li>Annibal</li>
    <li>Phoenix</li>
    <li>M.A. Barrakus</li>
    <li>Murdock</li>
</ul>
```

Repetirá la línea que hay dentro del `<c:forEach>` tantas veces como elementos haya en la colección. 

En el atributo `items`, indicamos la colección que queremos recorrer. En el atributo `var`, indicamos el nombre de la variable que usaremos para acceder a cada elemento de la colección. JSTL se encargará de acceder al request, extraer el atributo de la petición, y hacer los casting necesarios.

> 👀 En el for de JSTL, el atributo `items` usaremos la sintaxis de EL `${nombreColeccion}` pero en el atributo `var` se pone el nombre de la variable directamente. Mira el ejemplo. **Es algo que se suele confundir mucho** .

### Usar Beans

La etiqueta `jsp:useBean` se utiliza en las páginas JSP para crear una instancia de una clase Java y almacenarla en un objeto de ámbito (scope) específico, como por ejemplo la solicitud (request), la sesión (session) o la aplicación (application). Los distintos ámbitos lo veremos más adelante, por ahora usaremos el `request`, que es el que hemos visto.

No es algo obligatorio para que funcione, pero posiblemente el IDE que uses te podrá ayudar con sugerencias cuando uses *Expression Language*, ya que ahora SI que sabrá la clase del objeto que está tratando.

<img src="img/README/image-20230328194257826.png" alt="Autocomplet" style="zoom:80%;" />

El uso de `jsp:useBean` se puede dividir en dos partes: **declaración** y **uso**.

En la **declaración**, especificas el nombre del objeto que se creará y la clase de Java que se utilizará para crearlo, así como el alcance de ámbito para almacenarlo. Por ejemplo:

```jsp
<jsp:useBean id="usuario" class="com.example.Usuario" scope="request" />
```

En este caso, la etiqueta `jsp:useBean` creará una instancia de la clase `com.example.Usuario` y la almacenará en el ámbito de request con el nombre `usuario`.

Luego, puedes utilizar el objeto `usuario` en cualquier parte de la página JSP. Por ejemplo:

```jsp
<!-- Usando scriptlets... 👎 -->
<p>Bienvenido, <%= usuario.getNombre() %> !</p>

<!-- Usando Expression Language... 👍 -->
<p>Bienvenido, ${usuario.nombre} ! </p>
```

Es importante tener en cuenta que, si el objeto ya existe en el ámbito especificado, `jsp:useBean` simplemente lo recuperará y no creará una nueva instancia. Si el objeto no existe, `jsp:useBean` creará una nueva instancia y la almacenará en el ámbito especificado.

En resumen, `jsp:useBean` es una etiqueta útil para crear y almacenar objetos Java en diferentes ámbitos de una página JSP, sin tener que usar scriptlets.
