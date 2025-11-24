## Para el desarrollo de código:
### Software
- Sistema Operativo: Windows 10 Pro
- Lenguaje de Programación: Java 17 (configurado mediante la propiedad <release> en Maven)
- Entorno de Desarrollo (IDE): Apache NetBeans IDE 20
- Gestión de proyecto y dependencias: Apache Maven, conforme al archivo pom.xml del proyecto
- Dependencias Incluidas:
  - Jackson Databind (procesamiento de JSON)
  - Apache PDFBox (generación de boletas en PDF)
  - JCalendar (opcional, para manejo de fechas en reportes)
- Empaquetado: Generación de un archivo JAR ejecutable (“fat jar”) mediante maven-assembly-plugin, especificando la clase principal grupo2.mecanica_ed_02.App bajo el nombre final GestionMecanica.jar
- Control de Versiones: GitHub
