# Protocolo HTTP y Asincronía en JavaScript
## Protocolo HTTP
**HTTP (Hypertext Transfer Protocol)** es un protocolo de comunicación utilizado para la transferencia de datos entre un cliente (como un navegador web) y un servidor web. HTTP define cómo se envían y reciben los mensajes entre estos dos extremos y es la base de la comunicación en la web.
### Verbos HTTP

Los verbos HTTP (también conocidos como métodos) indican la acción que se desea realizar sobre un recurso. Algunos de los verbos más comunes son:

- **GET**: Solicita un recurso del servidor. No debería modificar el estado del recurso.
 ```get
 http://localhost:8080/api/v1/libro
 ```

- **POST**: Envía datos al servidor para crear o actualizar un recurso.
 ```post
 http://localhost:8080/api/v1/libro
 {
    "autor" : "{{$randomFirstName}}",
    "titulo": "{{$randomFileName}}",
    "isbn":"{{$randomBankAccount}}",
    "disponible" : true
}
 ```

- **PUT**: Actualiza un recurso existente en el servidor.
```put
http://localhost:8080/futbolista/1
{   "id" : 1,
    "nombres" : "{{$randomFirstName}}",
    "apellidos": "{{$randomLastName}}",
    "diaNacimiento": 4,
    "mesNacimiento": 10,
    "anioNacimiento": 2024,
    "caracteristicas":"Es zurdo tiene buena pegada",
    "posicion" : 2
}

```

- **DELETE**: Elimina un recurso del servidor.
```delete
http://localhost:8080/futbolista/2
```
#### Body en Protocolo HTTP
- El body (cuerpo) de una solicitud o respuesta HTTP contiene los datos enviados al servidor o recibidos del servidor. No todas las solicitudes o respuestas tienen un cuerpo. El cuerpo se usa típicamente en solicitudes POST, PUT o PATCH.
```json
{
    "nombres" : "{{$randomFirstName}}",
    "apellidos": "{{$randomLastName}}",
    "diaNacimiento": 4,
    "mesNacimiento": 6,
    "anioNacimiento": 2024,
    "caracteristicas":"Es zurdo tiene buena pegada",
    "posicion" : 1
}
```
##### Headers en Protocolo HTTP
- Los headers (encabezados) son campos de metadatos en una solicitud o respuesta HTTP. Proporcionan información adicional sobre la solicitud o la respuesta, como el tipo de contenido, el idioma, la autenticación, etc.
- Headers en solicitud

```http
GET /resource
Host: example.com
Accept: application/json
Authorization: Bearer token741
```
- Headers en Respuesta
```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 741

```
###### Códigos de Respuesta HTTP
- Los códigos de respuesta indican el resultado de una solicitud HTTP. Están divididos en varias categorías:

- 1xx: Información (e.g., 100 Continue)
- 2xx: Éxito (e.g., 200 OK, 201 Created)
- 3xx: Redirección (e.g., 301 Moved Permanently, 302 Found)
- 4xx: Error del Cliente (e.g., 400 Bad Request, 404 Not Found)
- 5xx: Error del Servidor (e.g., 500 Internal Server Error, 502 Bad Gateway)
**Ejemplo 404**
```html
<html>
  <body>
    <h1>Page Not Found</h1>
  </body>
</html>
```
###### Request y Response en Protocolo HTTP
- Request (Solicitud): Es el mensaje enviado por el cliente al servidor, que incluye el método HTTP, la URL, los headers y, en algunos casos, un body.

- Response (Respuesta): Es el mensaje enviado por el servidor al cliente, que incluye un código de estado, headers y, en algunos casos, un body.
###### Asincronía en JavaScript
- La asincronía en JavaScript permite ejecutar operaciones de manera no bloqueante. Esto significa que una tarea puede iniciarse y continuar ejecutándose en segundo plano, mientras que el resto del código sigue ejecutándose. Esto es útil para operaciones que pueden tardar tiempo, como solicitudes HTTP o leer archivos.
###### Como realizar peticiones asincronas
- Las peticiones asíncronas se pueden realizar usando la API fetch, XMLHttpRequest o bibliotecas como axios.
**Ejemplo usando XMLHttpRequest para una Solicitud GET:**
```js
const xhr = new XMLHttpRequest();
xhr.open('GET', 'http://localhost:5001/api/v1/clientes?nombre=Ronny', true);

xhr.onload = function() {
  if (xhr.status >= 200 && xhr.status < 300) {
    console.log('Datos recibidos:', xhr.responseText);
  } else {
    console.error('Error:', xhr.statusText);
  }
};

xhr.onerror = function() {
  console.error('Error de red');
};

xhr.send();
```
###### Como manejar la data luego de la peticion asincrona
- Después de realizar una petición asincrónica con XMLHttpRequest, puedes manejar la data obtenida en el evento onload de la solicitud.
```js
const xhr = new XMLHttpRequest();
xhr.open('GET', 'http://localhost:5001/api/v1/clientes?nombre=Ronny', true);

xhr.onload = function() {
  if (xhr.status >= 200 && xhr.status < 300) {
    const data = JSON.parse(xhr.responseText);
    const list = document.getElementById('data-list');
    data.forEach(item => {
      const li = document.createElement('li');
      li.textContent = item.name;
      list.appendChild(li);
    });
  } else {
    console.error('Error:', xhr.statusText);
  }
};

xhr.onerror = function() {
  console.error('Error de red');
};

xhr.send();

```