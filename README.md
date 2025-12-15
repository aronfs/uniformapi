# UniformBackend - Colección Bruno

Esta colección contiene todos los endpoints de la API del sistema UniformBackend.

## 📋 Configuración

### Variables de Entorno

El archivo `environments/Desarrollo.bru` contiene las variables de entorno:

- `base_url`: URL base del servidor (por defecto: `http://localhost:4000`)
- `token`: Token JWT obtenido después de iniciar sesión

### Autenticación

La mayoría de los endpoints requieren autenticación mediante JWT. Para usar los endpoints protegidos:

1. Primero ejecuta **"Iniciar Sesión"** para obtener un token
2. El token se guarda automáticamente en la variable `{{token}}`
3. Todos los endpoints protegidos incluyen automáticamente el header `Authorization: Bearer {{token}}`

## 📁 Estructura de Carpetas

```
UniformBackend/
├── Account/              # Autenticación y cuentas
├── Tipos Usuario/        # Gestión de tipos de usuario
├── Usuarios/             # CRUD de usuarios
├── Producotos/           # CRUD de productos
├── Categoria Productos/  # Gestión de categorías
├── Ordenes/              # Gestión de órdenes
├── Facturas/             # Gestión de facturas
└── environments/         # Variables de entorno
```

## 🔐 Endpoints de Autenticación

### 1. Crear Cuenta
- **POST** `/api/v1/accounts/register`
- **Descripción**: Crea la cuenta de administrador (solo puede ejecutarse una vez)
- **Autenticación**: No requerida
- **Body**: `email`, `password`

### 2. Iniciar Sesión
- **POST** `/api/v1/accounts/login`
- **Descripción**: Autentica un usuario y retorna un token JWT
- **Autenticación**: No requerida
- **Body**: `email`, `password`
- **Respuesta**: Token JWT en `data.token`

## 👥 Endpoints de Usuarios

### 1. Crear Usuario
- **POST** `/api/v1/user/createUser`
- **Autenticación**: Requerida
- **Body**: `name`, `last_name`, `telf`, `address`, `foto`, `email`, `type_user`

### 2. Obtener Todos los Usuarios
- **GET** `/api/v1/user/getUsers`
- **Autenticación**: Requerida

### 3. Obtener Usuario por Email
- **GET** `/api/v1/user/getUser/:email`
- **Autenticación**: Requerida
- **Parámetros**: `email` en la URL

### 4. Actualizar Usuario
- **PUT** `/api/v1/user/updateUser/:email`
- **Autenticación**: Requerida
- **Parámetros**: `email` en la URL
- **Body**: Campos opcionales a actualizar

### 5. Eliminar Usuario
- **DELETE** `/api/v1/user/deleteUser/:email`
- **Autenticación**: Requerida
- **Parámetros**: `email` en la URL

## 📦 Endpoints de Productos

### 1. Crear Producto
- **POST** `/api/v1/products/create`
- **Autenticación**: Requerida
- **Body**: `name` (requerido), `code`, `description`, `talla`, `color`, `foto`, `category_id`

### 2. Obtener Todos los Productos
- **GET** `/api/v1/products/getAll`
- **Autenticación**: Requerida

### 3. Obtener Producto por ID
- **GET** `/api/v1/products/get/:id`
- **Autenticación**: Requerida
- **Parámetros**: `id` en la URL

### 4. Actualizar Producto
- **PUT** `/api/v1/products/update/:id`
- **Autenticación**: Requerida
- **Parámetros**: `id` en la URL
- **Body**: Campos opcionales a actualizar

### 5. Eliminar Producto
- **DELETE** `/api/v1/products/delete/:id`
- **Autenticación**: Requerida
- **Parámetros**: `id` en la URL
- **Nota**: Desactiva el producto (soft delete)

## 🏷️ Endpoints de Categorías

### 1. Crear Categoría
- **POST** `/api/v1/categories/create`
- **Autenticación**: Requerida
- **Body**: `name`

### 2. Obtener Todas las Categorías
- **GET** `/api/v1/categories/getAll`
- **Autenticación**: Requerida

## 📋 Endpoints de Órdenes

### 1. Crear Orden
- **POST** `/api/v1/orders/create`
- **Autenticación**: Requerida
- **Body**: 
  - `name` (requerido)
  - `tipo_pago` (opcional)
  - `details` (array requerido)
    - `product_id`
    - `user_id`
    - `cantidad`
    - `precio`
- **Nota**: El total se calcula automáticamente como `cantidad * precio`

### 2. Obtener Todas las Órdenes
- **GET** `/api/v1/orders/getAll`
- **Autenticación**: Requerida

## 🧾 Endpoints de Facturas

### 1. Crear Factura
- **POST** `/api/v1/invoices/create`
- **Autenticación**: Requerida
- **Body**: `order_id`
- **Nota**: Calcula automáticamente subtotal, IVA (12%) y total

### 2. Obtener Todas las Facturas
- **GET** `/api/v1/invoices/getAll`
- **Autenticación**: Requerida

### 3. Obtener Factura por ID
- **GET** `/api/v1/invoices/get/:id`
- **Autenticación**: Requerida
- **Parámetros**: `id` en la URL

## 🔧 Tipos Usuario

### 1. Obtener Tipos de Usuario
- **GET** `/api/v1/typeUser/getTypeUser`
- **Autenticación**: Requerida

## 📝 Notas Importantes

1. **Primer Uso**: Debes crear la cuenta de administrador primero usando "Crear Cuenta" (solo una vez)

2. **Token JWT**: Después de iniciar sesión, el token se guarda automáticamente y se usa en todas las peticiones protegidas

3. **Códigos de Estado**:
   - `200`: Operación exitosa
   - `201`: Recurso creado exitosamente
   - `400`: Error en la petición
   - `404`: Recurso no encontrado

4. **Formato de Respuesta**:
   ```json
   {
     "status": 200,
     "message": "Mensaje descriptivo",
     "data": { ... }
   }
   ```

5. **Errores**:
   ```json
   {
     "code": 400,
     "message": "Descripción del error",
     "data": null
   }
   ```

## 🚀 Flujo de Trabajo Recomendado

1. **Configuración Inicial**:
   - Crear cuenta de administrador
   - Iniciar sesión para obtener token

2. **Gestión de Catálogo**:
   - Crear categorías
   - Crear productos asociados a categorías

3. **Gestión de Usuarios**:
   - Obtener tipos de usuario
   - Crear usuarios

4. **Proceso de Venta**:
   - Crear orden con productos
   - Generar factura de la orden

## 📞 Soporte

Para más información sobre la API, consulta la documentación de cada endpoint en Bruno o revisa el código fuente del proyecto.

