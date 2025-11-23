# Requisitos de Hardware y Software

## Para el desarrollo de código:
### Hardware
- Procesador: AMD A10-7860K Radeon R7, 12 Compute Cores 4C+8G
- Memoria RAM: 16.0 GB
- Tarjeta gráfica: AMD Radeon(TM) R7 Graphics
- Almacenamiento: 224 GB SSD
- Tipo de sistema: Procesador de 64 bits
- Conectividad: Acceso a Internet para descargas y actualizaciones de Software (5mbs)
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

## Para el cliente (uso final):
- Sistema Operativo: Windows 10, compatible con Java Runtime Environment (JRE) necesario para ejecutar la aplicación.
- Java Runtime Environment: Java SE 17 Runtime, para permitir la ejecución de la aplicación sin la necesidad completa del JDK.
- No se requerirán IDE ni herramientas de desarrollo, solo el entorno mínimo necesario para correr la aplicación.
