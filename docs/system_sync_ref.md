# System Sync Reference - Integración Frontend-Backend

## Información del Documento
**Propósito:** Documentar la sincronización y comunicación entre el frontend (React/Next.js) y backend (Node.js/Express)  
**Fecha de Creación:** 18 de Octubre, 2025  
**Última Actualización:** 18 de Octubre, 2025

---

## 1. Arquitectura de Comunicación

### 1.1 Protocolo
- **Tipo:** RESTful API
- **Formato:** JSON
- **Autenticación:** JWT (JSON Web Tokens)
- **Puerto Backend:** 3000 (desarrollo), variable en producción

### 1.2 Flujo de Autenticación
```
Cliente (Next.js)          Servidor (Express)
     │                            │
     ├──── POST /api/auth/login ──>│
     │     {email, password}       │
     │                            │
     │<──── 200 OK ───────────────┤
     │     {token, user, role}    │
     │                            │
     ├──── GET /api/projects ────>│
     │     Header: Authorization  │
     │     Bearer <token>          │
     │                            │
     │<──── 200 OK ───────────────┤
     │     {projects: [...]}      │
```

---

## 2. API Endpoints

### 2.1 Autenticación (`/api/auth`)

#### POST `/api/auth/register`
**Descripción:** Registro de nuevo usuario (solo Admin/Director)  
**Permisos:** Admin, Director  
**Request:**
```json
{
  "email": "user@example.com",
  "password": "SecurePass123!",
  "firstName": "Juan",
  "lastName": "Pérez",
  "role": "employee"
}
```
**Response (201):**
```json
{
  "success": true,
  "message": "Usuario creado exitosamente",
  "userId": "uuid-here"
}
```

#### POST `/api/auth/login`
**Descripción:** Inicio de sesión  
**Permisos:** Público (con credenciales)  
**Request:**
```json
{
  "email": "user@example.com",
  "password": "SecurePass123!"
}
```
**Response (200):**
```json
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "refreshToken": "refresh_token_here",
  "user": {
    "id": "uuid",
    "email": "user@example.com",
    "firstName": "Juan",
    "lastName": "Pérez",
    "role": "employee"
  }
}
```

#### POST `/api/auth/refresh`
**Descripción:** Renovar token de acceso  
**Request:**
```json
{
  "refreshToken": "refresh_token_here"
}
```
**Response (200):**
```json
{
  "success": true,
  "token": "new_access_token"
}
```

#### POST `/api/auth/logout`
**Descripción:** Cerrar sesión  
**Headers:** `Authorization: Bearer <token>`  
**Response (200):**
```json
{
  "success": true,
  "message": "Sesión cerrada exitosamente"
}
```

---

### 2.2 Usuarios (`/api/users`)

#### GET `/api/users`
**Descripción:** Listar todos los usuarios  
**Permisos:** Admin, Director  
**Headers:** `Authorization: Bearer <token>`  
**Query Params:**
- `page` (opcional): Número de página
- `limit` (opcional): Resultados por página
- `role` (opcional): Filtrar por rol

**Response (200):**
```json
{
  "success": true,
  "data": {
    "users": [
      {
        "id": "uuid",
        "email": "user@example.com",
        "firstName": "Juan",
        "lastName": "Pérez",
        "role": "employee",
        "createdAt": "2025-10-18T10:00:00Z"
      }
    ],
    "pagination": {
      "total": 25,
      "page": 1,
      "pages": 3
    }
  }
}
```

#### GET `/api/users/:id`
**Descripción:** Obtener usuario específico  
**Permisos:** Admin, Director, o el propio usuario  
**Response (200):**
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "email": "user@example.com",
    "firstName": "Juan",
    "lastName": "Pérez",
    "role": "employee",
    "assignedProjects": ["proj-1", "proj-2"],
    "createdAt": "2025-10-18T10:00:00Z"
  }
}
```

#### PUT `/api/users/:id`
**Descripción:** Actualizar usuario  
**Permisos:** Admin, Director  
**Request:**
```json
{
  "firstName": "Juan Carlos",
  "lastName": "Pérez García",
  "role": "director"
}
```

#### DELETE `/api/users/:id`
**Descripción:** Eliminar usuario  
**Permisos:** Admin  
**Response (200):**
```json
{
  "success": true,
  "message": "Usuario eliminado exitosamente"
}
```

---

### 2.3 Proyectos (`/api/projects`)

#### GET `/api/projects`
**Descripción:** Listar proyectos (todos o asignados según rol)  
**Permisos:** Todos los roles autenticados  
**Headers:** `Authorization: Bearer <token>`  
**Query Params:**
- `status`: Filtrar por estado (active, completed, archived)
- `assignedTo`: Filtrar por empleado asignado (solo Admin/Director)

**Response (200):**
```json
{
  "success": true,
  "data": [
    {
      "id": "proj-uuid",
      "projectId": "PRJ-2025-001",
      "name": "Subestación Industrial XYZ",
      "client": "Empresa ABC S.A.",
      "status": "active",
      "standards": ["IEC 60364", "IEEE 242"],
      "assignedEmployees": [
        {
          "id": "user-uuid",
          "name": "Juan Pérez"
        }
      ],
      "createdAt": "2025-10-01T10:00:00Z",
      "updatedAt": "2025-10-18T14:30:00Z"
    }
  ]
}
```

#### POST `/api/projects`
**Descripción:** Crear nuevo proyecto  
**Permisos:** Admin, Director  
**Request:**
```json
{
  "projectId": "PRJ-2025-001",
  "name": "Subestación Industrial XYZ",
  "client": "Empresa ABC S.A.",
  "description": "Diseño eléctrico de subestación 34.5kV",
  "standards": ["IEC 60364", "IEEE 242"],
  "assignedEmployees": ["employee-uuid-1", "employee-uuid-2"]
}
```
**Response (201):**
```json
{
  "success": true,
  "message": "Proyecto creado exitosamente",
  "data": {
    "id": "proj-uuid",
    "projectId": "PRJ-2025-001"
  }
}
```

#### GET `/api/projects/:id`
**Descripción:** Obtener detalles de proyecto específico  
**Permisos:** Admin, Director, o empleado asignado  
**Response (200):**
```json
{
  "success": true,
  "data": {
    "id": "proj-uuid",
    "projectId": "PRJ-2025-001",
    "name": "Subestación Industrial XYZ",
    "client": "Empresa ABC S.A.",
    "description": "Diseño eléctrico de subestación 34.5kV",
    "status": "active",
    "standards": ["IEC 60364", "IEEE 242"],
    "assignedEmployees": [...],
    "calculations": [
      {
        "id": "calc-uuid",
        "type": "ssaa",
        "status": "in_progress",
        "completedBy": "Juan Pérez",
        "lastUpdate": "2025-10-18T14:30:00Z"
      }
    ],
    "createdAt": "2025-10-01T10:00:00Z"
  }
}
```

#### PUT `/api/projects/:id`
**Descripción:** Actualizar proyecto  
**Permisos:** Admin, Director  
**Request:** (campos a actualizar)

#### DELETE `/api/projects/:id`
**Descripción:** Eliminar proyecto  
**Permisos:** Admin, Director

---

### 2.4 Memorias de Cálculo (`/api/calculations`)

#### GET `/api/projects/:projectId/calculations`
**Descripción:** Listar memorias de un proyecto  
**Permisos:** Admin, Director, o empleado asignado  
**Response (200):**
```json
{
  "success": true,
  "data": [
    {
      "id": "calc-uuid",
      "projectId": "proj-uuid",
      "type": "ssaa",
      "typeName": "Sistemas de Servicios Auxiliares",
      "status": "in_progress",
      "progress": 65,
      "createdBy": {
        "id": "user-uuid",
        "name": "Juan Pérez"
      },
      "createdAt": "2025-10-15T10:00:00Z",
      "updatedAt": "2025-10-18T14:30:00Z"
    }
  ]
}
```

#### POST `/api/projects/:projectId/calculations`
**Descripción:** Crear nueva memoria de cálculo  
**Permisos:** Admin, Director, empleado asignado  
**Request:**
```json
{
  "type": "ssaa",
  "initialData": {
    "systemVoltage": "125VDC",
    "batteryType": "VRLA"
  }
}
```

#### GET `/api/calculations/:id`
**Descripción:** Obtener memoria específica con todos sus datos  
**Permisos:** Admin, Director, empleado asignado  
**Response (200):**
```json
{
  "success": true,
  "data": {
    "id": "calc-uuid",
    "projectId": "proj-uuid",
    "type": "ssaa",
    "status": "in_progress",
    "calculationData": {
      "systemVoltage": "125VDC",
      "batteryType": "VRLA",
      "loads": [...],
      "batteries": {...},
      "results": {...}
    },
    "createdBy": {...},
    "updatedAt": "2025-10-18T14:30:00Z"
  }
}
```

#### PUT `/api/calculations/:id`
**Descripción:** Actualizar datos de memoria (guardado incremental)  
**Permisos:** Admin, Director, empleado asignado  
**Request:**
```json
{
  "calculationData": {
    "loads": [
      {
        "name": "Carga 1",
        "power": 100,
        "voltage": 125
      }
    ]
  },
  "status": "in_progress"
}
```

#### DELETE `/api/calculations/:id`
**Descripción:** Eliminar memoria de cálculo  
**Permisos:** Admin, Director

---

### 2.5 Reportes (`/api/reports`)

#### POST `/api/reports/generate/:calculationId`
**Descripción:** Generar documento Word con datos del proyecto y memoria  
**Permisos:** Todos los roles autenticados (con acceso al proyecto)  
**Request (opcional):**
```json
{
  "templateType": "standard",
  "includeGraphs": true,
  "language": "es"
}
```
**Response (200):**
```json
{
  "success": true,
  "data": {
    "fileUrl": "/downloads/report-uuid.docx",
    "fileName": "PRJ-2025-001_SSAA_Report.docx",
    "generatedAt": "2025-10-18T15:00:00Z"
  }
}
```

#### GET `/api/reports/download/:fileId`
**Descripción:** Descargar archivo generado  
**Response:** Binary stream (application/vnd.openxmlformats-officedocument.wordprocessingml.document)

---

## 3. Manejo de Errores

### 3.1 Códigos de Estado HTTP
- **200:** Operación exitosa
- **201:** Recurso creado exitosamente
- **400:** Bad Request (datos inválidos)
- **401:** No autenticado (token inválido/ausente)
- **403:** No autorizado (permisos insuficientes)
- **404:** Recurso no encontrado
- **409:** Conflicto (ej: email ya existe)
- **500:** Error interno del servidor

### 3.2 Formato de Respuesta de Error
```json
{
  "success": false,
  "error": {
    "code": "INVALID_CREDENTIALS",
    "message": "Email o contraseña incorrectos",
    "details": null
  }
}
```

### 3.3 Códigos de Error Comunes
- `INVALID_CREDENTIALS`: Credenciales incorrectas
- `TOKEN_EXPIRED`: Token expirado
- `INSUFFICIENT_PERMISSIONS`: Sin permisos
- `RESOURCE_NOT_FOUND`: Recurso no existe
- `VALIDATION_ERROR`: Datos inválidos
- `DATABASE_ERROR`: Error en BD
- `DUPLICATE_ENTRY`: Registro duplicado

---

## 4. Frontend State Management

### 4.1 Contextos de React
```javascript
// AuthContext: Manejo de autenticación
const AuthContext = {
  user: Object | null,
  token: String | null,
  login: Function,
  logout: Function,
  isAuthenticated: Boolean
}

// ProjectContext: Proyecto actual
const ProjectContext = {
  currentProject: Object | null,
  setCurrentProject: Function,
  refreshProject: Function
}

// CalculationContext: Memoria actual
const CalculationContext = {
  currentCalculation: Object | null,
  calculationData: Object,
  updateData: Function,
  saveProgress: Function
}
```

### 4.2 Hooks Personalizados
```javascript
// useAuth(): Manejo de autenticación
// useProject(projectId): Obtener y gestionar proyecto
// useCalculation(calcId): Obtener y actualizar memoria
// useAutoSave(data, interval): Guardado automático
```

---

## 5. Sincronización de Datos

### 5.1 Estrategia de Guardado
- **Guardado Automático:** Cada 30 segundos si hay cambios
- **Guardado Manual:** Botón "Guardar" disponible
- **Indicador Visual:** Estado de guardado (Guardado, Guardando..., Error)

### 5.2 Manejo de Conflictos
```javascript
// Si hay conflicto de versiones:
{
  "success": false,
  "error": {
    "code": "VERSION_CONFLICT",
    "message": "Los datos fueron actualizados por otro usuario",
    "serverData": {...},
    "clientData": {...}
  }
}
// Frontend debe resolver: mantener local, usar servidor, o merge
```

### 5.3 Optimistic Updates
- Cambios se reflejan inmediatamente en UI
- Si falla el guardado, se revierte y se notifica
- Se mantiene cola de operaciones pendientes

---

## 6. Validaciones

### 6.1 Frontend (React)
- Validación en tiempo real en formularios
- Librerías: React Hook Form + Yup/Zod
- Mensajes de error en español
- Prevención de envío con errores

### 6.2 Backend (Express)
- Validación de esquemas con Joi o express-validator
- Sanitización de inputs
- Validación de tipos de datos
- Validación de permisos

### 6.3 Ejemplos de Reglas
```javascript
// Email
email: required, valid email format

// Password
password: min 8 chars, 1 uppercase, 1 number, 1 special char

// Project ID
projectId: required, unique, formato "PRJ-YYYY-XXX"

// Calculation data
numbers: valid numeric, ranges específicos por campo
```

---

## 7. Websockets (Futuro)

### 7.1 Casos de Uso Planeados
- Notificaciones en tiempo real
- Colaboración simultánea
- Actualizaciones de estado de proyecto

### 7.2 Eventos
```javascript
// Socket.io events (cuando se implemente)
socket.on('project:updated', handleProjectUpdate)
socket.on('calculation:saved', handleCalculationSaved)
socket.on('user:assigned', handleUserAssigned)
```

---

## 8. Testing de Integración

### 8.1 Backend Tests
- Tests de endpoints con Supertest
- Mocking de base de datos
- Tests de autenticación y autorización

### 8.2 Frontend Tests
- Tests de componentes con React Testing Library
- Tests de integración de API calls
- Mocking de respuestas con MSW

---

## 9. Performance

### 9.1 Frontend
- Lazy loading de rutas
- Memoización de componentes pesados
- Debouncing de búsquedas
- Virtualización de listas largas

### 9.2 Backend
- Índices en BD para queries frecuentes
- Paginación de resultados grandes
- Caché de datos estáticos
- Compresión de respuestas (gzip)

---

## 10. Logs y Monitoreo

### 10.1 Logging Backend
```javascript
// Winston logger
logger.info('User login', { userId, timestamp })
logger.error('Database error', { error, query })
logger.warn('Unauthorized access attempt', { userId, resource })
```

### 10.2 Métricas Frontend
- Tiempo de carga de páginas
- Errores de API capturados
- Acciones de usuario (analytics)
- Performance de renders

---

## 11. Configuración de CORS

### 11.1 Desarrollo
```javascript
// Backend Express
const corsOptions = {
  origin: 'http://localhost:3001', // Next.js dev server
  credentials: true,
  optionsSuccessStatus: 200
}
```

### 11.2 Producción
```javascript
const corsOptions = {
  origin: process.env.FRONTEND_URL,
  credentials: true,
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization']
}
```

---

## 12. Checklist de Integración

### 12.1 Antes de Implementar Nuevo Endpoint
- [ ] Documentar en este archivo
- [ ] Definir estructura request/response
- [ ] Implementar validaciones backend
- [ ] Crear tests de endpoint
- [ ] Implementar en frontend
- [ ] Manejar estados de carga y error
- [ ] Actualizar types/interfaces TypeScript

### 12.2 Antes de Deploy
- [ ] Verificar todas las env variables
- [ ] Probar flujos completos end-to-end
- [ ] Revisar logs de errores
- [ ] Validar permisos por rol
- [ ] Verificar CORS en producción
- [ ] Backup de base de datos

---

**Última Actualización:** 18 de Octubre, 2025  
**Versión:** 1.0