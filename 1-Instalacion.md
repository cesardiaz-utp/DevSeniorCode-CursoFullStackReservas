# Guía de Instalación de Herramientas

Esta guía detalla los pasos necesarios para configurar tu entorno de desarrollo para el curso de Aplicaciones FullStack.

## 1. Java Development Kit (JDK) 25

El JDK proporciona las herramientas necesarias para compilar y ejecutar aplicaciones Java. En este curso lo usamos para desarrollar y poner en marcha el backend basado en Spring Boot; sin él no se podría construir ni lanzar la API.

- **Descarga**: Accede a la web oficial de Oracle o utiliza [Adoptium (Temurin)](https://adoptium.net/). Esta es la distribución del kit que incluye el compilador `javac`, la máquina virtual JVM y otras utilidades.
- **Instalación**: Ejecuta el instalador y marca la opción "Add to PATH" para que los comandos Java sean accesibles desde cualquier terminal. Esto añade las rutas necesarias a las variables de entorno.
- **Verificación**: Abre una terminal y ejecuta:

  ```bash
  java -version
  ```

  Con este comando confirmas que el ejecutable se encuentra en el PATH y ves la versión instalada, lo cual es útil para asegurarte de que tienes la versión requerida.

## 2. Node.js 24

Node.js es un entorno de ejecución de JavaScript en el servidor que incluye `npm`, el gestor de paquetes. Es fundamental para el frontend porque nos permite instalar librerías de Angular, compilar el proyecto y lanzar el servidor de desarrollo con `ng serve`.

- **Descarga**: Ve a [nodejs.org](https://nodejs.org/) y selecciona la versión 24 (elige la LTS para máxima estabilidad). Esta instalación incluye el runtime de Node y el administrador de paquetes `npm`.
- **Verificación**:

  ```bash
  node -v
  npm -v
  ```

  Estos comandos te muestran las versiones instaladas; confirma que ambas herramientas están disponibles antes de avanzar.

## 3. Git y GitHub

Git es el sistema de control de versiones que usamos para llevar un historial de los cambios en el código. GitHub es una plataforma remota que nos permite alojar repositorios, colaborar con otros y hacer copias de seguridad de nuestro trabajo.

- **Git**: Descárgalo desde [git-scm.com](https://git-scm.com/). La instalación te dará acceso a comandos como `git commit`, `git push` y `git pull`.
- **GitHub**: Crea una cuenta en [github.com](https://github.com/) si aún no la tienes; allí alojarás tus repositorios y podrás compartir proyectos y colaborar.
- **Configuración inicial**: Una vez instalado Git, define tu identidad global con estos comandos para que tus commits estén correctamente atribuidos:

  ```bash
  git config --global user.name "Tu Nombre"
  git config --global user.email "tu@email.com"
  ```

  También puedes configurar otros ajustes como el editor predeterminado o el formato de línea de finalización dependiendo de tu entorno.

## 4. Postgresql

PostgreSQL es un sistema de gestión de bases de datos relacionales de código abierto y potente. En este curso, lo utilizaremos para almacenar de forma persistente toda la información de nuestra aplicación (reservas, usuarios, productos, etc.), aprovechando su robustez y compatibilidad con Spring Data JPA.

- **Descarga**: Ve a la página oficial de [PostgreSQL](https://www.postgresql.org/download/) y selecciona el instalador para tu sistema operativo (Windows, macOS o Linux).
- **Instalación**:
  - Ejecuta el instalador y sigue los pasos del asistente.
  - **Contraseña del superusuario**: Durante la instalación, se te pedirá una contraseña para el usuario `postgres`. Asegúrate de recordarla, ya que la necesitarás para configurar la conexión en Spring Boot.
  - **Puerto**: Por defecto utiliza el `5432`. Déjalo así a menos que ya esté ocupado.
  - **Stack Builder**: Al finalizar, no es necesario instalar complementos adicionales a través de Stack Builder para este curso.
- **Herramienta de Administración (pgAdmin)**: El instalador suele incluir **pgAdmin**, una interfaz gráfica que te permitirá crear la base de datos `mi_basededatos` de forma visual antes de conectar la aplicación.
- **Verificación**: Abre pgAdmin o utiliza la terminal (`psql -U postgres`) para confirmar que puedes acceder al servidor de base de datos.

## 5. Cursor

Cursor es un editor de código basado en VS Code que integra inteligencia artificial de forma nativa. Lo utilizaremos como nuestra herramienta principal de desarrollo para acelerar la escritura de código, realizar refactorizaciones inteligentes y resolver errores de manera eficiente tanto en el Backend como en el Frontend.

- **Descarga**: Ve a [cursor.com](https://cursor.com/) y descarga la versión correspondiente a tu sistema operativo.
- **Instalación**: Ejecuta el instalador. Al ser un fork de VS Code, puedes importar todas tus extensiones, configuraciones y atajos de teclado existentes durante el primer inicio.
- **Configuración de IA**: Al iniciar, Cursor te permitirá configurar el modelo de lenguaje (como Claude 3.5 Sonnet o GPT-4o). Asegúrate de loguearte para aprovechar las funciones de autocompletado predictivo (Tab) y el chat contextual (`Ctrl+L` o `Cmd+L`).
- **Uso de Composer**: Familiarízate con la función Composer (`Ctrl+I` o `Cmd+I`), que permite generar cambios en múltiples archivos simultáneamente siguiendo instrucciones en lenguaje natural.

## 6. Extensiones necesarias

Para sacar el máximo partido a este curso, instala las siguientes extensiones en VS Code:

1. **Java Extension Pack** (incluye herramientas de lenguaje Java, Maven y Spring Boot). Permite autocompletar, depurar y ejecutar aplicaciones Spring.
2. **Spring Boot Extension Pack** o al menos **Spring Boot Tools**: facilita la creación y ejecución de proyectos Spring Boot.
3. **Angular Language Service**: proporciona autocompletado, navegación y comprobaciones en tus archivos `.ts`, `.html` y `.css` de Angular.

> 💡 Puedes instalar todas estas extensiones a la vez desde el marketplace de VS Code o mediante la paleta de comandos (`Ctrl+Shift+P`) con `Extensiones: Instalar extensiones` y buscando por nombre.
