# Inventory-API

API REST para gestión de productos e inventario, con autenticación JWT y autorización basada en roles. Deployada en Render con PostgreSQL.

**Demo:** [API](https://product-api-p3ji.onrender.com) | [Swagger UI](https://product-api-p3ji.onrender.com/swagger-ui.html)

## Stack
 - Java 21 · Spring Boot 4.1.0
 - Spring Security + JWT
 - Spring Data JPA / Hibernate · Bean Validation
 - PostgreSQL (producción) · H2 (desarrollo)
 - Maven · Docker
 - JUnit 5 + Mockito
 - Swagger / OpenAPI

## Features
 - CRUD de productos y categorías
 - Autenticación con JWT y roles ADMIN/USER
 - Validaciones de datos y manejo global de errores
 - Documentación interactiva con Swagger
 - Tests unitarios de la capa de servicios
 - Containerizado con Docker y deployado en Render con PostgreSQL

## Decisiones técnicas destacadas
 - DTOs para no exponer entidades: la API nunca devuelve entidades JPA directamente.
 - Perfiles de configuración (dev/prod): H2 en memoria para desarrollo local y PostgreSQL en producción.
 - Seeder idempotente: en Render, cada reinicio del servicio volvía a ejecutar la carga inicial y duplicaba registros. Lo resolví haciendo la carga idempotente: antes de insertar, verifica si los datos ya existen.

## Cómo correrlo localmente
```bash
git clone https://github.com/TomiiMN/Inventory-API.git
cd Inventory-API
cp .env.example .env
```

Variables de entorno necesarias:
```bash
JWT_SECRET=<secreto-generado>
JWT_EXPIRATION=3600000
```
Comandos para levantar la app con:
  * Docker:
```bash
docker build -t inventory-api .
docker run -p 8080:8080 --env-file .env inventory-api
```
  * Maven:
```bash
./mvnw spring-boot:run
```
La API queda disponible en http://localhost:8080 y Swagger en http://localhost:8080/swagger-ui.html.

## Tests
```bash
./mvnw test
```
13 tests unitarios con JUnit 5 y Mockito, con 72% de cobertura en la capa de servicios.

## Deploy

Credenciales de prueba:
- Admin: admin@demo.com / admin1234
- User: user@demo.com / user1234

Nota: el servicio está en el free tier de Render, puede tardar ~1 min en responder tras un período de inactividad.

## Endpoints principales

| Método | Endpoint                                     | Descripción                                    | Auth requerida |
|--------|----------------------------------------------|------------------------------------------------|----------------|
| POST   | /auth/register                               | Registro de usuario                            | No             |
| POST   | /auth/login                                  | Login, devuelve JWT                            | No             |
| GET    | /products                                    | Listar productos                               | Sí             |
| GET    | /products/{id}                               | Obtener un producto                            | Sí             |
| GET    | /products/price/{price}                      | Listar productos menores al precio indicado    | Sí             |
| GET    | /products/category/{categoryName}            | Listar productos por nombre de categoría       | Sí             |
| GET    | /categories                                  | Listar categorías                              | Sí             |
| GET    | /categories/{id}                             | Obtener una categoría                          | Sí             |
| GET    | /categories/{id}/products                    | Listar los productos de una categoría          | Sí             |
| POST   | /products                                    | Crear producto                                 | Sí (ADMIN)     |
| POST   | /categories                                  | Crear una categoría                            | Sí (ADMIN)     |
| PUT    | /products/{id}                               | Actualizar producto                            | Sí (ADMIN)     |
| PUT    | /products/{productId}/category/{categoryId}  | Asignar o cambiar la categoría de un producto  | Sí (ADMIN)     |
| PUT    | /categories/{id}                             | Actualizar una categoría existente             | Sí (ADMIN)     |
| DELETE | /products/{id}                               | Eliminar producto                              | Sí (ADMIN)     |
| DELETE | /categories/{id}                             | Eliminar categoría                             | Sí (ADMIN)     |
