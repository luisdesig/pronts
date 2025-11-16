¡Claro! Aquí tienes la redacción final para el correo electrónico, incorporando la ruta específica de la API, los requisitos de eliminación lógica, la implementación de JWT (generación, renovación y revocación) y el uso del patrón Builder para la conversión de entidades.

He mantenido el formato de texto corrido y el tono profesional.

Asunto: Especificación Técnica Final: Microservicio Dinamus Security (v1) con JWT y Eliminación Lógica

Hola [Nombre del Destinatario/Equipo],

Adjunto la especificación técnica final y detallada para el desarrollo del microservicio Security del proyecto Dinamus. La API base será dinamus/security/v1/.

El microservicio se construirá en Spring Boot con Maven y utilizará Spring Data JPA. El package principal es com.dinamus.security. Se debe integrar Lombok y Log4j.

1. Modelado de Datos y Estándares
La capa de persistencia debe incluir las entidades: Person, User, Tenant, Module, Operation, Role (agregar entidades de asociación si son necesarias, como OperationTenant o UserTenantRole).

Estándares de Código: Se debe usar inglés, Camel Case y la correcta aplicación del singular/plural. Todas las clases, interfaces y funciones públicas deben estar documentadas (Javadoc).

Eliminación Lógica (Soft Delete): Los registros de la base de datos no deben eliminarse físicamente. Se debe implementar un campo boolean para manejar la eliminación lógica y un campo timestamp para registrar la fecha de eliminación.

Manejo de IDs:

ID Interno: Campo id (Integer) para JPA y relaciones internas.

ID Público (API): Campo pk (UUID) para exponer la entidad y ser usado en requests y responses.

2. DTOs y Conversión de Modelos
Se requieren DTOs de Request y Response para cada entidad.

Regla de ID en DTOs: El ID público (UUID) de la entidad debe ser mapeado al campo id en los DTOs. El ID interno (Integer) no debe incluirse.

Métodos de Conversión: Las clases Entidad deben incluir dos funciones estáticas:

toResponse(): Convierte la entidad a su DTO de Respuesta.

fromRequest(): Convierte el DTO de Solicitud a la entidad, utilizando el patrón Builder.

Se requiere generar las interfaces y las implementaciones para las capas Service y Repository.

3. Seguridad y Funcionalidad (JWT)
Se debe implementar la funcionalidad de login y control de acceso utilizando JSON Web Tokens (JWT):

Login (Autenticación): Permite el login de un User.

Generación de Tokens: El JWT inicial debe tener una duración de 5 minutos.

Renovación: Implementar una funcionalidad para renovar el token expirado con una duración de 5 minutos adicionales.

Revocación: Implementar la funcionalidad de eliminar o revocar el token para cerrar la sesión de manera segura.

Lógica de Negocio Multitenant: El sistema debe validar los permisos basándose en el rol que un User tiene en un Tenant específico. Las combinaciones de Operation por Role son únicas para cada Tenant.

4. Manejo de Respuestas y Errores
Se implementará un mecanismo de respuesta unificado (ApiResponse) que cubra ambos escenarios:

Éxito: Contiene url, data, timestamp y code (HTTP Status).

Error: Contiene url, error (detalle), timestamp y code (HTTP Status).

Se debe implementar un gestor de excepciones centralizado para capturar errores comunes de runtime (ej., ResourceNotFound, InvalidData, etc.) y mapearlos al formato ApiResponse de Error. Es crucial gestionar las validaciones directamente en los Controllers y, si se usa try-catch, personalizar el mensaje de error para evitar exponer trazas de Java al exterior del sistema.
