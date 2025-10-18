# Quick Start Guide - Próximos Pasos

## Información del Documento
**Propósito:** Guía de pasos inmediatos a seguir después de cada sesión de desarrollo  
**Fecha de Creación:** 18 de Octubre, 2025  
**Última Actualización:** 18 de Octubre, 2025

---

## 🎯 Estado Actual del Proyecto

### ✅ Completado en Esta Sesión (Sesión #1 - 18 Oct 2025)
- [x] Definición de arquitectura del sistema
- [x] Selección de stack tecnológico (Node.js/Express + React/Next.js + MySQL)
- [x] Definición de roles y permisos (Admin, Director, Empleado)
- [x] Identificación de 3 memorias iniciales (SSAA, Coordinación de Protecciones, Cálculos y Ductos)
- [x] Creación de 5 archivos core de documentación
- [x] Definición conceptual de flujos de trabajo

### 🔄 En Progreso
- Ninguno (proyecto iniciando)

### ⏳ Pendiente para Próximas Sesiones
- Diseño detallado de base de datos
- Estructura de carpetas del proyecto
- Configuración inicial del repositorio Git
- Setup del proyecto backend
- Setup del proyecto frontend
- Definición detallada de UIs por tipo de memoria

---

## 📋 Próximos Pasos Inmediatos

### PASO 1: Diseño de Base de Datos (PRIORITARIO)
**Objetivo:** Crear el esquema completo de MySQL antes de comenzar a codificar

**Tareas:**
1. Diseñar tabla `users` con campos:
   - id (UUID/VARCHAR)
   - email (UNIQUE)
   - password_hash
   - first_name, last_name
   - role (ENUM: admin, director, employee)
   - created_at, updated_at
   - is_active

2. Diseñar tabla `projects` con campos:
   - id (UUID/VARCHAR)
   - project_id (VARCHAR UNIQUE) ej: "PRJ-2025-001"
   - name, client, description
   - status (ENUM: active, completed, archived)
   - standards (JSON o tabla relacional)
   - created_by, created_at, updated_at

3. Diseñar tabla `project_assignments`:
   - id
   - project_id (FK)
   - user_id (FK)
   - assigned_at
   - assigned_by

4. Diseñar tabla `calculations`:
   - id
   - project_id (FK)
   - type (ENUM: ssaa, coordinacion, ductos, etc.)
   - status (ENUM: not_started, in_progress, completed)
   - progress (INT 0-100)
   - created_by (FK)
   - created_at, updated_at

5. Diseñar tabla `calculation_data`:
   - id
   - calculation_id (FK)
   - data_json (JSON o LONGTEXT)
   - version (INT)
   - updated_by (FK)
   - updated_at

6. Diseñar tabla `audit_log`:
   - id
   - user_id (FK)
   - action (VARCHAR)
   - resource_type, resource_id
   - changes (JSON)
   - ip_address
   - created_at

**Preguntas a Resolver:**
- ¿Usar UUIDs o INT auto-increment para IDs?
- ¿Estructura exacta del JSON en calculation_data?
- ¿Necesitamos tabla de normas/standards separada?

**Entregable:** Script SQL con CREATE TABLE statements

---

### PASO 2: Estructura de Carpetas del Proyecto
**Objetivo:** Definir organización de archivos antes de iniciar desarrollo

**Estructura Propuesta Backend:**
```
backend/
├── src/
│   ├── config/          # Configuraciones (DB, JWT, etc)
│   ├── controllers/     # Lógica de endpoints
│   ├── middleware/      # Auth, validation, error handling
│   ├── models/          # Modelos de BD (si usamos ORM)
│   ├── routes/          # Definición de rutas
│   ├── services/        # Lógica de negocio
│   ├── utils/           # Funciones auxiliares
│   └── app.js           # Setup de Express
├── tests/               # Tests unitarios e integración
├── .env.example
├── .gitignore
├── package.json
└── README.md
```

**Estructura Propuesta Frontend:**
```
frontend/
├── src/
│   ├── app/             # Next.js app directory
│   │   ├── (auth)/      # Grupo de rutas auth
│   │   ├── (dashboard)/ # Grupo de rutas protegidas
│   │   ├── api/         # API routes (si se usan)
│   │   └── layout.js
│   ├── components/      # Componentes reutilizables
│   │   ├── ui/          # Componentes UI base
│   │   ├── forms/       # Formularios
│   │   └── layout/      # Layout components
│   ├── contexts/        # React contexts
│   ├── hooks/           # Custom hooks
│   ├── lib/             # Utilidades y configs
│   ├── services/        # API calls
│   └── types/           # TypeScript types
├── public/
├── .env.local.example
├── next.config.js
└── package.json
```

**Decisión Pendiente:**
- ¿Usar TypeScript en frontend? (Recomendado: SÍ)
- ¿Usar TypeScript en backend? (Opcional)

---

### PASO 3: Inicializar Repositorio Git
**Objetivo:** Configurar control de versiones desde el inicio

**Comandos:**
```bash
# Crear repo principal
mkdir electrical-calculations-platform
cd electrical-calculations-platform

# Inicializar Git
git init

# Crear estructura base
mkdir backend frontend docs

# Crear .gitignore principal
touch .gitignore

# Primer commit
git add .
git commit -m "Initial commit: Project structure and core documentation"

# Conectar con GitHub/GitLab
git remote add origin [URL_DEL_REPOSITORIO]
git push -u origin main
```

**Contenido de .gitignore raíz:**
```
node_modules/
.env
.env.local
*.log
.DS_Store
dist/
build/
coverage/
```

---

### PASO 4: Definir Schemas de Validación
**Objetivo:** Establecer reglas de validación para cada entidad

**Ejemplos a Definir:**
- Schema de registro/login
- Schema de creación de proyecto
- Schema de datos por cada tipo de memoria (SSAA, Coordinación, Ductos)

**Herramientas:**
- Backend: Joi o express-validator
- Frontend: Yup o Zod con React Hook Form

---

### PASO 5: Diseño de UIs (Wireframes)
**Objetivo:** Definir estructura visual de cada pantalla

**Pantallas Prioritarias:**
1. Login
2. Dashboard por rol (3 versiones)
3. Listado de proyectos
4. Detalle de proyecto
5. Interfaz SSAA (más compleja)
6. Interfaz Coordinación de Protecciones
7. Interfaz Cálculos y Ductos

**Decisión Pendiente:**
- ¿Hacer mockups en Figma/Sketch o directamente en código?
- ¿Qué biblioteca de componentes UI usar? (Opciones: Material-UI, Chakra, Tailwind + shadcn/ui)

---

## 🚀 Inicio Rápido para Desarrollador Nuevo

### Si este es tu primer día en el proyecto:

1. **Lee los documentos en este orden:**
   - `README.md` (cuando esté creado)
   - `Complete_reference.md`
   - `System_sync_ref.md`
   - Este documento

2. **Configura tu entorno:**
   - Instala Node.js (v18+)
   - Instala MySQL localmente o usa Docker
   - Clona el repositorio
   - Crea archivo `.env` basado en `.env.example`

3. **Instala dependencias:**
   ```bash
   cd backend && npm install
   cd ../frontend && npm install
   ```

4. **Ejecuta migraciones de BD:**
   ```bash
   cd backend
   npm run migrate
   ```

5. **Inicia servidores de desarrollo:**
   ```bash
   # Terminal 1 - Backend
   cd backend && npm run dev

   # Terminal 2 - Frontend
   cd frontend && npm run dev
   ```

---

## 📊 Métricas de Progreso

### Fase 1: Fundamentos (0-25%)
- [x] Documentación inicial
- [ ] Diseño de BD
- [ ] Setup de proyectos
- [ ] Autenticación básica

### Fase 2: Core Features (25-50%)
- [ ] CRUD de usuarios
- [ ] CRUD de proyectos
- [ ] Sistema de permisos
- [ ] Primera memoria (SSAA)

### Fase 3: Expansión (50-75%)
- [ ] Segunda y tercera memoria
- [ ] Generación de reportes Word
- [ ] Dashboard completo
- [ ] Guardado automático

### Fase 4: Pulido y Deploy (75-100%)
- [ ] Testing completo
- [ ] Optimizaciones
- [ ] Deploy a Hostinger
- [ ] Documentación de usuario

**Progreso Actual: 5%** (Documentación base completada)

---

## ❓ Preguntas Frecuentes

### ¿Cómo agrego una nueva memoria de cálculo?
1. Agregar tipo a ENUM en tabla `calculations`
2. Crear componente UI específico
3. Definir schema de validación
4. Implementar endpoint de guardado
5. Crear template Word
6. Actualizar documentación

### ¿Cómo funciona el guardado automático?
- Frontend usa `useAutoSave` hook
- Cada 30 segundos detecta cambios
- Envía PUT a `/api/calculations/:id`
- Backend actualiza `calculation_data`
- Se muestra indicador visual del estado

### ¿Dónde se almacenan los templates Word?
- Carpeta `/backend/templates/` en servidor
- Nombrados por tipo: `ssaa_template.docx`
- Se llenan con librería `docxtemplater`

---

## 🔧 Comandos Útiles (cuando estén configurados)

```bash
# Backend
npm run dev          # Modo desarrollo
npm run test         # Ejecutar tests
npm run migrate      # Aplicar migraciones
npm run seed         # Poblar BD con datos de prueba

# Frontend
npm run dev          # Modo desarrollo
npm run build        # Build de producción
npm run lint         # Linter
npm run test         # Tests de componentes

# Base de datos
npm run db:reset     # Resetear BD (¡cuidado en prod!)
npm run db:backup    # Backup de BD
```

---

## 📞 Contactos y Recursos

**Documentación Técnica:**
- Complete_reference.md - Conocimiento general
- System_sync_ref.md - API y sincronización
- Change_log.md - Historial de cambios
- Roadmap.md - Planificación de fases

**Para Iniciar Nueva Sesión:**
1. Leer este documento
2. Revisar Change_log.md para ver últimos cambios
3. Consultar Roadmap.md para objetivos de la sesión
4. ¡Empezar a desarrollar!

---

## ✅ Checklist de Sesión

**Al Finalizar Cada Sesión de Desarrollo:**
- [ ] Actualizar este documento con nuevos pasos
- [ ] Documentar cambios en Change_log.md
- [ ] Actualizar progreso en Roadmap.md
- [ ] Hacer commit de cambios con mensaje descriptivo
- [ ] Actualizar Complete_reference.md si hay cambios arquitectónicos
- [ ] Actualizar System_sync_ref.md si hay nuevos endpoints

---

## 🎯 Objetivo de Próxima Sesión

**Sesión #2 - Diseño de Base de Datos**

**Entregables Esperados:**
1. Script SQL completo con todas las tablas
2. Diagrama ER de base de datos
3. Decisión sobre estructura de datos JSON para memorias
4. Índices y constraints definidos
5. Scripts de datos semilla (seed data)

**Duración Estimada:** 1-2 horas

**Preparación Requerida:**
- Tener MySQL instalado o acceso a Hostinger DB
- Herramienta de modelado DB (MySQL Workbench, dbdiagram.io, etc.)

---

**Última Actualización:** 18 de Octubre, 2025  
**Sesión Actual:** #1  
**Próxima Sesión:** Por programar