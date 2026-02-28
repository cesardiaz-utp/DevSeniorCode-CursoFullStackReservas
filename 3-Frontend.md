# Curso: Desarrollo de Aplicaciones FullStack con Java y Angular - Frontend

## 1. Introducción al Desarrollo Web

### a. Fundamentos (HTML, CSS, JavaScript)

Antes de Angular, es crucial entender los pilares de la web. Estos tres lenguajes forman la base sobre la cual se construye cualquier sitio o aplicación web, desde el más simple blog hasta el servicio de streaming más complejo.

- **HTML (HyperText Markup Language)**: Es el lenguaje de marcado que define la **estructura** y el contenido de una página web. Piensa en HTML como el esqueleto de tu aplicación. Define las partes lógicas del contenido, como títulos, párrafos, tablas, listas y enlaces. Sin HTML, una página web sería solo texto sin formato.
- **CSS (Cascading Style Sheets)**: Es el lenguaje de estilos que define **la apariencia y el diseño** de los elementos HTML. Con CSS, puedes dar color, formato y organizar el diseño de tu sitio. Controla aspectos como colores, tipografía, espaciado, y la posición de los elementos. Es lo que transforma un esqueleto básico en una interfaz visualmente atractiva.
- **JavaScript**: Es el lenguaje de programación que añade **interactividad y lógica** a una página web. Permite que la página reaccione a las acciones del usuario (como clics o movimientos del mouse), valide formularios, o incluso cargue nuevos datos sin recargar la página completa. JavaScript es el "músculo" que hace que la experiencia de usuario sea dinámica y receptiva.

### b. ¿Por qué Angular?

En las aplicaciones web modernas, una página simple ya no es suficiente. Los usuarios esperan experiencias dinámicas y rápidas, similares a las de las aplicaciones de escritorio. Aquí es donde entran los **Single Page Applications (SPA)** y frameworks como Angular.

Un **SPA** es una aplicación web que se carga en una sola página HTML, y el contenido se reescribe dinámicamente a medida que el usuario interactúa. Esto hace que la navegación sea mucho más fluida y rápida, ya que el navegador no tiene que recargar toda la página. Angular es un framework de Google que nos ayuda a construir SPAs a gran escala. Proporciona una estructura clara, herramientas robustas y un conjunto de principios de diseño que permiten a los equipos de desarrollo colaborar de manera eficiente y mantener el código base a lo largo del tiempo.

## 2. Fundamentos de Angular

### a. TypeScript

Angular está construido sobre **TypeScript**, un superconjunto de JavaScript que añade tipado estático. Esto significa que podemos definir tipos de datos para nuestras variables, lo que ayuda a prevenir errores comunes y hace que el código sea más predecible y fácil de mantener. A diferencia de JavaScript, donde una variable puede cambiar de tipo (`let x = 10; x = "hola";`), TypeScript lo impide, obligando a una mayor consistencia.

- **Ventaja**: Mayor legibilidad y un sistema de validación que te avisa de los errores antes de ejecutar el código. Por ejemplo, si intentas asignar una cadena a una variable que esperas que sea un número, TypeScript te lo señalará de inmediato en tu editor.

### b. Estructura de la Aplicación y Componentes

En las versiones más recientes de Angular, el enfoque principal son los componentes **standalone** (autónomos). Esto simplifica la estructura, eliminando la necesidad de los módulos tradicionales. Ahora, cada componente es independiente y gestiona sus propias dependencias, lo que hace que el código sea más legible y más fácil de organizar.

- **Componentes (`Components`)**: Son el bloque de construcción fundamental de una aplicación Angular. Cada componente controla una parte de la pantalla (ej. un encabezado, una tabla de productos o un formulario). Un componente está compuesto por:
  - **Un archivo de TypeScript (lógica)**: Contiene la clase del componente y define las propiedades y métodos.
  - **Un archivo de HTML (plantilla)**: Contiene la estructura de la interfaz de usuario.
  - **Un archivo de CSS (estilos)**: Contiene las reglas de estilo para ese componente en particular.
- **Modelos (`Models`)**: Son clases o interfaces que definen la estructura de los datos que vamos a usar en la aplicación. Por ejemplo, la clase Producto que definimos en el backend, que nos asegura que los datos que recibimos y enviamos tienen el formato correcto.
- **Servicios (`Services`)**: Contienen lógica que se puede compartir entre diferentes componentes, como la lógica para comunicarse con el backend a través de APIs, realizar cálculos complejos o gestionar el estado de la aplicación. Son una excelente forma de mantener la lógica de negocio separada de la lógica de la interfaz de usuario.

## 3. Integración entre Backend y Frontend

### a. CORS (Cross-Origin Resource Sharing)

Para que nuestra aplicación de Angular, que se ejecuta en un dominio (`localhost:4200`), pueda comunicarse con nuestra API de backend, que se ejecuta en otro dominio (`localhost:8080`), necesitamos habilitar CORS. Sin esta configuración, el navegador bloqueará las peticiones por seguridad.

El backend debe configurar las cabeceras `Access-Control-Allow-Origin` para permitir las peticiones del frontend.

### b. Configuración de CORS en Spring Boot

Para habilitar CORS en nuestro backend de Spring Boot, la práctica recomendada es definir un `CorsFilter` como un bean en nuestra configuración. Esto asegura que la configuración de CORS se aplique a todas las peticiones de manera uniforme.

- **Archivo**: `src/main/java/.../config/WebConfig.java`

  ```java
  package com.miaplicacion.config;

  import org.springframework.context.annotation.Bean;
  import org.springframework.context.annotation.Configuration;
  import org.springframework.web.cors.CorsConfiguration;
  import org.springframework.web.cors.UrlBasedCorsConfigurationSource;
  import org.springframework.web.filter.CorsFilter;

  @Configuration
  public class WebConfig {

      @Bean
      public CorsFilter corsFilter() {
          UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
          CorsConfiguration config = new CorsConfiguration();
          config.setAllowCredentials(true);
          config.addAllowedOrigin("http://localhost:4200"); // Permite peticiones desde el origen de Angular
          config.addAllowedHeader("*");
          config.addAllowedMethod("*"); // Permite todos los métodos HTTP (GET, POST, PUT, DELETE, etc.)
          source.registerCorsConfiguration("/**", config);
          return new CorsFilter(source);
      }
  }
  ```

Este código crea un filtro de CORS que se aplicará a todas las rutas (`/**`). Le indica a Spring Boot que debe aceptar peticiones del origen `http://localhost:4200` y permite todos los métodos y cabeceras necesarios para la comunicación.

### c. Consumo de APIs HTTP en Angular

Utilizaremos el servicio `HttpClient` para manejar todas nuestras peticiones a la API. Este servicio es fundamental para la comunicación con el backend y maneja las peticiones de forma asíncrona usando Observables.

#### Paso 1: Configurar `HttpClientModule` y el Enrutamiento

Antes de poder usar el servicio `HttpClient`, debemos registrarlo como un proveedor en nuestra aplicación. Como estamos usando componentes standalone, el proveedor se agrega directamente en el archivo de configuración principal de la aplicación.

Además, con la incorporación de un componente de login, es esencial configurar el enrutamiento para gestionar la navegación entre las diferentes vistas de la aplicación.

- **Archivo: `src/app/app.config.ts`**

  ```typescript
  import { ApplicationConfig } from '@angular/core';
  import { provideRouter } from '@angular/router';
  import { provideHttpClient } from '@angular/common/http';
  import { routes } from './app.routes';

  export const appConfig: ApplicationConfig = {
    providers: [
      provideRouter(routes), // Registramos las rutas
      provideHttpClient() // Lo agregamos como un proveedor en la configuración
    ]
  };
  ```

- **Archivo: `src/app/app.routes.ts`**

  ```typescript
  import { Routes } from '@angular/router';
  import { LoginComponent } from './login/login.component';
  import { ProductoComponent } from './producto/producto.component';

  export const routes: Routes = [
      { path: '', redirectTo: '/login', pathMatch: 'full' },
      { path: 'login', component: LoginComponent },
      { path: 'productos', component: ProductoComponent }
  ];
  ```

#### Paso 2: Actualizar el `ProductoService` con todos los métodos HTTP

Ahora, modificaremos nuestro servicio para incluir los cuatro tipos de peticiones HTTP principales: `GET` para obtener datos, `POST` para crear, `PUT` para actualizar y `DELETE` para eliminar.

```typescript
// src/app/producto.service.ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';
import { Producto } from './producto';

@Injectable({
  providedIn: 'root'
})
export class ProductoService {
  private apiUrl = 'http://127.0.0.1:8080/api/productos';

  constructor(private http: HttpClient) { }

  // Obtener todos los productos
  getProductos(): Observable<Producto[]> {
    return this.http.get<Producto[]>(this.apiUrl);
  }

  // Crear un nuevo producto
  addProducto(producto: Producto): Observable<Producto> {
    return this.http.post<Producto>(this.apiUrl, producto);
  }

  // Actualizar un producto existente
  updateProducto(producto: Producto): Observable<any> {
    return this.http.put(`${this.apiUrl}/${producto.id}`, producto);
  }

  // Eliminar un producto por su ID
  deleteProducto(id: number): Observable<any> {
    return this.http.delete(`${this.apiUrl}/${producto.id}`);
  }
}
```

## 4. Prompts para la Generación del Servicio Angular

### Paso 0: Ajusta las reglas de Angular

```plain
Agrega dentro de las reglas la estructura de paquetes propuesta para distribuir los elementos a las carpetas de cada tipo.
Recuerda escribirlas en ingles.
```

### Paso 1: Creación del servicio

#### 🧠 Prompt para la creación del servicio Angular

```plain
Usando @cursor.mdc como regla, crea un servicio en Angular llamado ReservaService.

Debe:
- Usar HttpClient
- Tener métodos para:
  - Obtener todas las reservas
  - Crear una reserva
  - Cancelar una reserva
- Usar una variable de entorno para la URL del backend

Realiza las configuraciones adecuadas para que todo quede en funcionamiento.
```

#### 📘 Explicación del Servicio Angular

Conceptos clave:

- `HttpClient` para consumir APIs
- Observables (`Observable<T>`)
- Separación de lógica y UI
- Variables de entorno

👉 El servicio es el puente con el backend.

### Paso 2: 9️⃣ Componente de listado de reservas

#### 🧠 Prompt para la creación del componente de listado

```plain
Crea una página en Angular que muestre una tabla con las reservas.

El componente debe:
- Obtener las reservas desde el ReservaService
- Mostrar los datos en una tabla
- Permitir cancelar una reserva mediante un botón
- Usar el servicio ReservaService

Recuerda que debemos tener separados los 3 archivos del componente (`.ts`, `.html` y `.css`)
```

#### 📘 Explicación del componente de listado

Conceptos clave:

- Ciclo de vida (`ngOnInit`)
- Data binding
- Eventos `(click)`
- Renderizado dinámico con `@for`

👉 Aquí ya vemos el sistema funcionando visualmente.

### Formulario Reactivo

#### 🧠 Prompt para la creación del componente de formulario

```plain
Crea un formulario reactivo en Angular para crear una reserva.

Debe incluir los campos:
- nombreCliente
- fecha
- hora
- servicio

Todos los campos son obligatorios.
Al enviar el formulario, debe llamarse al método correspondiente del servicio.
```

### 📘 Explicación del formulario

Conceptos clave:

- `FormGroup` y `FormControl`
- Validaciones
- Manejo de eventos
- Experiencia de usuario

👉 Formularios reactivos = control total.

### 🎉 Resultado Final

Al final del taller tienes:

- Backend Spring Boot funcional
- Frontend Angular integrado
- Reglas de negocio reales
- Uso profesional de IA

---
_Esto lo hiciste en 5 horas… imagina lo que lograrás en el bootcamp._
