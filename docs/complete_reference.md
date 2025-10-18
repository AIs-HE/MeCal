# Complete Reference - Sistema de Memorias de Cálculo Eléctrico

## Información del Proyecto

**Nombre del Proyecto:** Sistema de Gestión de Memorias de Cálculo Eléctrico  
**Fecha de Inicio:** 18 de Octubre, 2025  
**Stack Tecnológico:**
- **Backend:** Node.js con Express
- **Frontend:** React con Next.js
- **Base de Datos:** MySQL (Hostinger)
- **Control de Versiones:** Git

---

## 1. Visión General del Sistema

### 1.1 Propósito
Aplicación web para gestionar memorias de cálculo eléctrico siguiendo normas IEC e IEEE. Permite a diferentes roles de usuarios crear, editar y gestionar estudios eléctricos con persistencia en base de datos y generación de documentos Word.

### 1.2 Usuarios Objetivo
- **Administradores:** Control total del sistema
- **Directores:** Gestión completa de proyectos y usuarios
- **Empleados:** Desarrollo de memorias de cálculo en proyectos asignados

### 1.3 Funcionalidades Core
- Autenticación y autorización por roles
- Gestión de proyectos con ID único
- Desarrollo de memorias de cálculo especializadas
- Almacenamiento incremental en MySQL
- Generación de templates Word con datos del proyecto

---

## 2. Arquitectura del Sistema

### 2.1 Estructura General
```
┌─────────────────┐
│   React/Next.js │  Frontend
│   (Cliente)     │
└────────┬────────┘
         │ HTTP/REST API
         │
┌────────▼────────┐
│   Node.js       │  Backend
│   Express       │
└────────┬────────┘
         │ SQL Queries
         │
┌────────▼────────┐
│   MySQL         │  Base de Datos
│   (Hostinger)   │
└─────────────────┘
```

### 2.2 Módulos Principales

#### Backend Modules
- **Auth Module:** Autenticación JWT, gestión de sesiones
- **Users Module:** CRUD de usuarios y roles
- **Projects Module:** Gestión de proyectos
- **Calculations Module:** Lógica de memorias de cálculo
- **Reports Module:** Generación de templates Word
- **Database Module:** Conexión y queries a MySQL

#### Frontend Modules
- **Auth Pages:** Login, registro, recuperación
- **Dashboard:** Panel principal por rol
- **Project Management:** Listado y gestión de proyectos
- **Calculation Interfaces:** UIs específicas por tipo de memoria
- **Report Viewer:** Visualización y descarga de documentos

---

## 3. Roles y Permisos

### 3.1 Matriz de Permisos

| Funcionalidad | Admin | Director | Empleado |
|--------------|-------|----------|----------|
| Gestionar usuarios | ✅ | ✅ | ❌ |
| Crear proyectos | ✅ | ✅ | ❌ |
| Eliminar proyectos | ✅ | ✅ | ❌ |
| Ver todos los proyectos | ✅ | ✅ | ❌ |
| Ver proyectos asignados | ✅ | ✅ | ✅ |
| Editar datos de proyecto | ✅ | ✅ | ✅* |
| Crear memorias de cálculo | ✅ | ✅ | ✅ |
| Editar memorias de cálculo | ✅ | ✅ | ✅* |
| Eliminar memorias de cálculo | ✅ | ✅ | ❌ |
| Generar reportes | ✅ | ✅ | ✅ |

*Solo en proyectos asignados

### 3.2 Definición de Roles

**Admin:**
- Control total del sistema
- Gestión de usuarios y roles
- Configuración del sistema
- Acceso a todas las funcionalidades

**Director:**
- Creación y gestión completa de proyectos
- Asignación de proyectos a empleados
- Supervisión de todos los proyectos
- Aprobación de memorias de cálculo

**Empleado:**
- Desarrollo de memorias en proyectos asignados
- Edición de datos de cálculo
- Consulta de proyectos propios
- Generación de reportes preliminares

---

## 4. Tipos de Memorias de Cálculo

### 4.1 Memorias Fase 1 (Inicial)

#### 4.1.1 SSAA (Sistemas de Servicios Auxiliares)
**Descripción:** Cálculo de sistemas auxiliares de corriente alterna y continua  
**Normas Aplicables:** IEC 61850, IEEE 1547  
**Inputs Típicos:**
- Cargas AC/DC
- Baterías y rectificadores
- Inversores
- Sistemas de respaldo

#### 4.1.2 Coordinación de Protecciones
**Descripción:** Estudios de selectividad y coordinación de dispositivos de protección  
**Normas Aplicables:** IEEE 242, IEC 60909  
**Inputs Típicos:**
- Niveles de cortocircuito
- Curvas de protecciones
- Tiempos de operación
- Ajustes de relés

#### 4.1.3 Cálculos y Ductos
**Descripción:** Dimensionamiento de conductores y canalizaciones  
**Normas Aplicables:** IEC 60364, IEEE 399  
**Inputs Típicos:**
- Corrientes de carga
- Longitudes de circuitos
- Condiciones de instalación
- Factores de corrección

### 4.2 Memorias Futuras (Fase 2+)
- Estudios de cortocircuito
- Flujo de carga
- Análisis de armónicos
- Estudios de arco eléctrico
- Dimensionamiento de transformadores
- Cálculo de puesta a tierra
- [Total: 10-30 memorias]

---

## 5. Estructura de Base de Datos

### 5.1 Principios de Diseño
- Normalización hasta 3FN
- Tablas relacionales con llaves foráneas
- Almacenamiento incremental de datos
- Campos JSON para datos flexibles de cálculo
- Auditoría con timestamps

### 5.2 Tablas Principales (Conceptual)

**users**
- Datos de usuarios, credenciales, roles

**projects**
- Información general de proyectos
- Cliente, normas aplicables, estado

**project_assignments**
- Relación proyectos-empleados

**calculations**
- Memorias de cálculo por proyecto
- Tipo, datos, resultados, estado

**calculation_data**
- Datos específicos de cada tipo de memoria (JSON flexible)

**audit_log**
- Registro de cambios y acciones

---

## 6. Flujo de Trabajo

### 6.1 Flujo de Creación de Proyecto
1. Admin/Director crea proyecto con datos generales
2. Asigna empleados al proyecto
3. Define tipos de memorias requeridas
4. Empleados desarrollan memorias asignadas
5. Revisión y aprobación
6. Generación de documento final

### 6.2 Flujo de Desarrollo de Memoria
1. Empleado accede a proyecto asignado
2. Selecciona tipo de memoria
3. Ingresa datos de forma incremental
4. Sistema guarda automáticamente en BD
5. Empleado puede pausar y retomar
6. Finaliza y marca como completa
7. Genera reporte preliminar

### 6.3 Flujo de Generación de Documento
1. Usuario solicita reporte de proyecto
2. Sistema consulta datos de BD
3. Carga template Word correspondiente
4. Rellena campos con datos del proyecto
5. Incluye cálculos y resultados
6. Genera documento descargable
7. Ingeniero finaliza documento manualmente

---

## 7. Seguridad

### 7.1 Autenticación
- JWT tokens para sesiones
- Refresh tokens para renovación
- Passwords hasheados (bcrypt)
- Políticas de contraseñas fuertes

### 7.2 Autorización
- Middleware de verificación de roles
- Validación de permisos por endpoint
- Control de acceso a recursos propios

### 7.3 Protección de Datos
- Validación de inputs (backend y frontend)
- Sanitización de datos
- Protección contra SQL injection
- CORS configurado adecuadamente

---

## 8. Integración Futura con IA

### 8.1 Funcionalidades Planeadas
- Asistente para ingreso de datos
- Validación automática de cálculos
- Sugerencias basadas en normas
- Detección de inconsistencias
- Generación de conclusiones preliminares

### 8.2 Consideraciones Técnicas
- API de LLM a integrar
- Prompts específicos por tipo de memoria
- Validación de outputs de IA
- Feedback loop para mejora continua

---

## 9. Deployment

### 9.1 Entorno Actual
- **Hosting:** Hostinger (compartido)
- **Base de Datos:** MySQL en Hostinger
- **Dominio:** [Por definir]

### 9.2 Entorno Futuro (Producción)
- **VPS:** Hostinger VPS
- **Node.js:** Runtime en servidor
- **PM2:** Process manager
- **Nginx:** Reverse proxy
- **SSL:** Certificado Let's Encrypt

### 9.3 Variables de Entorno
```
DB_HOST=
DB_USER=
DB_PASSWORD=
DB_NAME=
JWT_SECRET=
JWT_EXPIRES_IN=
NODE_ENV=production
PORT=3000
```

---

## 10. Consideraciones de Desarrollo

### 10.1 Mejores Prácticas
- Código modular y reutilizable
- Comentarios en español para documentación
- Manejo consistente de errores
- Logging de operaciones críticas
- Testing de funcionalidades core

### 10.2 Convenciones
- **Idioma:** Código en inglés, comentarios en español
- **Naming:** camelCase para JS, snake_case para DB
- **Git:** Commits descriptivos en español
- **Branches:** feature/, bugfix/, hotfix/

### 10.3 Performance
- Consultas DB optimizadas con índices
- Paginación en listados grandes
- Carga lazy de componentes React
- Caché de datos estáticos
- Compresión de respuestas

---

## 11. Documentación de Normas

### 11.1 IEC (International Electrotechnical Commission)
- **IEC 60364:** Instalaciones eléctricas de baja tensión
- **IEC 60909:** Cálculo de corrientes de cortocircuito
- **IEC 61850:** Comunicaciones en subestaciones

### 11.2 IEEE (Institute of Electrical and Electronics Engineers)
- **IEEE 242:** Protección de sistemas industriales
- **IEEE 399:** Análisis de sistemas de potencia
- **IEEE 1547:** Interconexión de recursos distribuidos

### 11.3 Recursos
- Documentos normativos en carpeta `/docs/normas/`
- Referencias rápidas por tipo de memoria
- Actualizaciones de normas

---

## 12. Glosario

- **SSAA:** Sistemas de Servicios Auxiliares
- **CRUD:** Create, Read, Update, Delete
- **JWT:** JSON Web Token
- **VPS:** Virtual Private Server
- **IEC:** International Electrotechnical Commission
- **IEEE:** Institute of Electrical and Electronics Engineers
- **BD/DB:** Base de Datos / Database

---

## 13. Contacto y Recursos

**Equipo de Desarrollo:**
- [Por definir]

**Recursos Externos:**
- Hostinger Support: https://www.hostinger.com/support
- Node.js Docs: https://nodejs.org/docs
- React Docs: https://react.dev
- Next.js Docs: https://nextjs.org/docs
- MySQL Docs: https://dev.mysql.com/doc/

---

**Última Actualización:** 18 de Octubre, 2025  
**Versión del Documento:** 1.0