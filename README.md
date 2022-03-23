# JSTL - JSP Standard Tag Library

## Instalación de librerías JSTL

### Con Apache Tomcat 9 o anterior

Deberás incluir la siguiente librería de JSTL en la carpeta `WEB-INF/lib` de tu proyecto.

> [jstl-1.2.jar](lib-tomcat9/jstl-1.2.jar)

### Con Apache Tomcat 10 o superior

Si usas Tomcat 10 o superior, al usar la OpenJDK de Java, las librerías son las de Jakarta (en lugar de las de javax). Por lo tanto, los archivos a copiar en la carpeta `WEB-INF/lib` son las siguientes (los dos archivos):

> [jakarta.servlet.jsp.jstl-2.0.0.jar](lib-tomcat10/jakarta.servlet.jsp.jstl-2.0.0.jar)

> [jakarta.servlet.jsp.jstl-api-2.0.0.jar](lib-tomcat10/jakarta.servlet.jsp.jstl-api-2.0.0.jar)