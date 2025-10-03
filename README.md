# Ondroiver 🚗

### Una aplicacion por Carlos Villamizar y Fabian pertuz  👥


---




## ¿Qué entiendes por “endpoint” en el contexto de una API?

Un endpoint, en este contexto, es la direccion url donde se utiliza como punto de entrada para que el cliente pueda acceder a un recurso o servicio dentro de la API.

---

## ¿Cuál es la diferencia entre un endpoint público y uno privado?

Un endpoint publico como su nombre lo indica se puede acceder libremente sin restriccion o permisos por internet, mientras que un endpoint privado requiere una autenticacion o verificacion previa, ya sea una IP privada u otros parametros.


---

## ¿Qué información de un usuario consideras confidencial y no debería exponerse?

Existen diversos datos de los usuarios los cuales son confidenciales y no deberian salir a la luz mas alla del almacenamiento de una aplicacion, tales como datos bancarios, contraseñas, direccion de residencia, y dependiendo de ciertos criterios que solicitan otras aplicaciones que puedan llegar a terceros para ser amenazados o extorsionados.

---

## ¿Por qué es importante definir bien los métodos HTTP (GET, POST, PUT/PATCH, DELETE) en cada endpoint?

Definir correctamente los métodos HTTP es crucial para que las API sean claras, predecibles y sigan los principios de la arquitectura RESTful, facilitando la gestión de recursos (crear, leer, actualizar, eliminar), promoviendo la escalabilidad al no depender del estado del cliente y garantizando una separación de responsabilidades entre cliente y servidor. 

---

## ¿Qué tipo de información requiere autenticación en este sistema?

La autenticacion de usuario es crucial para diferenciar el tipo de cliente al cual ingresa a la plataforma y los distintos metodos que podra ejecutar al ser de cierto tipo o no, tambien influye en la parte de gestion y administrativa para realizar funciones especificas.

---

## ¿Cómo manejarías la seguridad de la ubicación de conductores y pasajeros?

Mostrandole previamente al conductor el lugar de encuetro y lugar de destino para que este tenga constancia en todo momento de a donde y por donde se dirige, a su vez mostrarle el historial de este cliente para saber si lo acepta o no y viceversa, el pasajero no podra cambiar su lugar de destino una vez establecido y tendra que ser muy claro a la hora de comunicarse y elegir el conductor.

---

## ¿Qué pasaría si un viaje es solicitado y no hay conductores disponibles? ¿Cómo debería responder la API?

Se pondria en prioridad dicha solicitud para que el cliente que ya lleva tiempo esperando pueda tener mayor posibilidad de ser aceptado; otra opcion seria dandole un bono al conductor que acepte dicho viaje. Tambien es importante informarle al cliente en todo momento si no hay conductores disponibles o si el precio estipulado no es suficiente para el trayecto solicitado.

---

## ¿Cómo identificarías los recursos principales de esta aplicación?

Los recursos principales y mas importantes de la aplicacion serian: users, rides, payments, ratings.

---

## ¿Qué ventajas tendría versionar la API (por ejemplo, /v1/...) desde el inicio?

Permite mantener compatibilidad con versiones anteriores, facilitar actualizaciones futuras y evitar romper integraciones existentes.

---

## ¿Por qué es importante documentar las respuestas de error y no solo las exitosas?

Para llevar constancia de dichos errores para investigarlos, corregirlos y mejorarlos en un futuro para que la aplicacion pueda salir a flote sin problemas ni fallas recurrentes.


# Roles y permisos

### Pasajero
	Solicitar viaje, ver historial, pagar, calificar, cancelar viajes.
### Conductor
	Aceptar/rechazar viajes, ver historial, actualizar estado, calificar.
### Administrador
	Ver todos los viajes, usuarios, estadísticas, eliminar cuentas, suspender usuarios.

---

# Recursos principales

- users: usuarios registrados (pasajeros, conductores, administradores).

- auth: autenticación y registro.

- rides: viajes solicitados y gestionados.

- payments: información y procesamiento de pagos.

- ratings: calificaciones entre usuarios.

- vehicles: información de vehículos de los conductores.

- locations: actualizaciones de ubicación en tiempo real.

# Tabla de Endpoints

| Método | Endpoint                   | Descripción                                | Parámetros | Autenticación |
|--------|----------------------------|--------------------------------------------|------------|----------------|
| POST   | `/v1/auth/register`        | Registro de usuario                        | `body`: name, email, password, role | Público |
| POST   | `/v1/auth/login`           | Inicio de sesión                           | `body`: email, password | Público |
| POST   | `/v1/auth/logout`          | Cierre de sesión                           | - | Token |
| GET    | `/v1/users/me`             | Obtener perfil propio                      | - | Token |
| PUT    | `/v1/users/me`             | Actualizar perfil                          | `body`: name, phone, photo | Token |
| POST   | `/v1/rides`                | Solicitar un viaje                         | `body`: origin, destination | Token (Pasajero) |
| GET    | `/v1/rides/me`             | Ver historial de viajes propios            | `query`: status | Token |
| GET    | `/v1/rides/active`         | Ver viaje en curso                         | - | Token |
| POST   | `/v1/rides/:id/cancel`     | Cancelar viaje                             | `path`: id | Token |
| POST   | `/v1/rides/:id/accept`     | Aceptar viaje                              | `path`: id | Token (Conductor) |
| POST   | `/v1/rides/:id/start`      | Marcar inicio del viaje                    | `path`: id | Token (Conductor) |
| POST   | `/v1/rides/:id/finish`     | Finalizar viaje                            | `path`: id | Token (Conductor) |
| POST   | `/v1/payments/:rideId`     | Procesar pago de viaje                     | `path`: rideId | Token |
| GET    | `/v1/payments/history`     | Historial de pagos                         | - | Token |
| POST   | `/v1/ratings/:rideId`      | Calificar usuario tras un viaje            | `body`: score, comment | Token |
| GET    | `/v1/ratings/me`           | Ver calificaciones propias                 | - | Token |
| GET    | `/v1/admin/users`          | Listar todos los usuarios                  | `query`: role | Token (Admin) |
| DELETE | `/v1/admin/users/:id`      | Eliminar un usuario                        | `path`: id | Token (Admin) |
| GET    | `/v1/vehicles/me`          | Ver datos del vehículo (conductor)         | - | Token (Conductor) |
| PUT    | `/v1/vehicles/me`          | Actualizar vehículo                        | `body`: plate, model, color | Token (Conductor) |

---

# Flujos de uso

## 1. Solicitud, aceptación y finalización de un viaje

1. El pasajero solicita un viaje vía POST /v1/rides.

2. Un conductor disponible acepta vía POST /v1/rides/:id/accept.

3. El conductor inicia viaje con POST /v1/rides/:id/start.

4. Al llegar al destino, finaliza con POST /v1/rides/:id/finish.

5. El sistema procesa el pago automáticamente.

6. Ambos usuarios califican la experiencia.

## 2. Cancelación de viaje y reembolso

1. El pasajero cancela el viaje con POST /v1/rides/:id/cancel.

2. Si el viaje aún no fue aceptado, se cancela sin penalidad.

3. Si fue aceptado, se aplica política de penalización parcial.

4. Se registra el evento y se devuelve parte del monto si aplica.

## 3. Calificación entre usuarios

1. Tras finalizar un viaje, el sistema habilita el endpoint POST /v1/ratings/:rideId.

2. El pasajero califica al conductor, y viceversa.

3. Las calificaciones afectan el score visible en el perfil (GET /v1/ratings/me).

---

# Decisiones de diseño y justificación

- #### Versionado:
 Se usa /v1/ desde el inicio para facilitar futuras mejoras sin romper integraciones.

- #### Seguridad:
 Autenticación vía JWT, endpoints protegidos según el rol. Uso de HTTPS obligatorio.

- #### Privacidad:
 Ubicaciones en tiempo real solo compartidas durante viajes activos.

- #### Errores:
 Todos los endpoints pueden retornar errores estructurados (error_code, message).

- #### Claridad RESTful: 
Uso correcto de verbos HTTP para representar acciones.


# Manejo de errores

**Formato de error general:**

```json
{
  "error_code": "RIDE_NOT_FOUND",
  "message": "El viaje solicitado no existe."
}

```

### Errores comunes

| Código                 | Situación                                | HTTP |
|------------------------|-----------------------------------------|------|
| `AUTH_INVALID_CREDENTIALS` | Login fallido                       | 401  |
| `RIDE_NOT_FOUND`           | Viaje no existe                     | 404  |
| `USER_UNAUTHORIZED`        | Token inválido o expirado           | 401  |
| `NO_DRIVERS_AVAILABLE`     | No hay conductores disponibles      | 200 o 404 |
| `PAYMENT_FAILED`           | Error en procesamiento de pago      | 402  |


# Propuestas de mejora

1. Soporte para pagos en efectivo.

2. Modo “solo mujeres” para pasajeras que lo deseen.

3. Sistema de bonificaciones por fidelidad.

4. Soporte para múltiples paradas en un mismo viaje.

5. Chat en tiempo real entre conductor y pasajero.
