# Sistema de Gestión de Recursos Humanos

> Una aplicación web full-stack profesional para la gestión integral de empleados, nómina y administración de recursos humanos. Desarrollada con **Spring Boot** en el backend y **React** en el frontend.

![Java](https://img.shields.io/badge/Java-17-orange?style=flat-square&logo=java)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-brightgreen?style=flat-square&logo=spring-boot)
![React](https://img.shields.io/badge/React-18.x-61dafb?style=flat-square&logo=react)
![MySQL](https://img.shields.io/badge/MySQL-8.0-blue?style=flat-square&logo=mysql)
![Axios](https://img.shields.io/badge/Axios-HTTP%20Client-purple?style=flat-square)

## 📋 Descripción

Sistema integral de gestión de recursos humanos que permite:

- ✅ Administración completa de empleados (CRUD)
- ✅ Gestión de nóminas y salarios
- ✅ Control de asistencia y permisos
- ✅ Departamentos y roles
- ✅ Reportes y análisis de RRHH
- ✅ Interfaz web responsiva y moderna
- ✅ API RESTful robusta

## 🏗️ Arquitectura

```
┌─────────────────────────────────────────────────────────────┐
│                   Frontend (React + Axios)                   │
│                   Interfaz Responsiva - SPA                   │
└──────────────────────┬──────────────────────────────────────┘
                       │
          ┌────────────┴────────────┐
          │                         │
        HTTP                      REST
       (Port 3000)              (Port 8080)
          │                         │
┌─────────▼──────────────────────────┘
│  Backend (Spring Boot 3.x + REST)
│  - REST Controllers
│  - Service Layer
│  - Repository Pattern (JPA)
└──────────────┬──────────────────────
               │
┌──────────────▼──────────────────────┐
│      Data Layer (MySQL DB)           │
│      - Empleados                      │
│      - Nóminas                        │
│      - Departamentos                  │
│      - Usuarios y Permisos            │
└────────────────────────────────────┘
```

## 🛠️ Tecnologías

### Backend
- **Spring Boot 3.x** - Framework web
- **Spring Data JPA** - Persistencia de datos
- **MySQL 8.0** - Base de datos relacional
- **Lombok** - Reducción de boilerplate
- **Maven** - Gestión de dependencias

### Frontend
- **React 18.x** - Biblioteca de UI
- **Axios** - Cliente HTTP
- **React Router** - Navegación SPA
- **CSS/Bootstrap** - Estilos responsivos

## 📦 Requisitos

- Java 17 o superior
- Node.js 16.x o superior
- npm 8.x o superior
- MySQL 8.0 o superior
- Git

## 🚀 Cómo Ejecutar

### Backend (Spring Boot)

1. Navega al directorio del backend:
```bash
cd rh_system_spring
```

2. Configura las variables de entorno (crear `application.properties`):
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/rh_sistema
spring.datasource.username=root
spring.datasource.password=tu_contraseña
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=false
```

3. Compila y ejecuta:
```bash
mvn clean install
mvn spring-boot:run
```

El backend estará disponible en: `http://localhost:8080`

### Frontend (React)

1. Navega al directorio del frontend:
```bash
cd rh_system_react
```

2. Instala las dependencias:
```bash
npm install
```

3. Configura la URL base de la API (en `.env` o `config.js`):
```
REACT_APP_API_URL=http://localhost:8080/api
```

4. Inicia el servidor de desarrollo:
```bash
npm start
```

El frontend estará disponible en: `http://localhost:3000`

## 📚 Estructura del Proyecto

```
Sistema-de-Recursos-Humanos-Spring-React/
├── rh_system_spring/              # Backend Spring Boot
│   ├── src/
│   │   ├── main/java/
│   │   │   ├── controllers/       # REST Controllers
│   │   │   ├── services/          # Business Logic
│   │   │   ├── repositories/      # Data Access
│   │   │   ├── entities/          # JPA Entities
│   │   │   └── RhSystemApplication.java
│   │   └── resources/
│   │       └── application.properties
│   └── pom.xml
│
├── rh_system_react/               # Frontend React
│   ├── public/
│   ├── src/
│   │   ├── components/            # React Components
│   │   ├── pages/                 # Page Components
│   │   ├── services/              # API Client (Axios)
│   │   ├── App.js
│   │   └── index.js
│   ├── package.json
│   └── .env
│
├── Front.jpg                      # Screenshot de la interfaz
├── README.md                      # Este archivo
└── .gitignore
```

## 💻 API Endpoints Principales

### Empleados
```http
GET    /api/empleados              # Listar todos
GET    /api/empleados/{id}         # Obtener por ID
POST   /api/empleados              # Crear nuevo
PUT    /api/empleados/{id}         # Actualizar
DELETE /api/empleados/{id}         # Eliminar
```

### Nóminas
```http
GET    /api/nominas                # Listar nóminas
POST   /api/nominas                # Generar nómina
GET    /api/nominas/{empleadoId}   # Nóminas por empleado
```

### Departamentos
```http
GET    /api/departamentos           # Listar departamentos
POST   /api/departamentos           # Crear departamento
```

## 📝 Ejemplos de Código

### Backend - Controlador de Empleados
```java
@RestController
@RequestMapping("/api/empleados")
public class EmpleadoController {
    
    @Autowired
    private EmpleadoService empleadoService;
    
    @GetMapping
    public ResponseEntity<List<Empleado>> obtenerTodos() {
        return ResponseEntity.ok(empleadoService.obtenerTodos());
    }
    
    @PostMapping
    public ResponseEntity<Empleado> crearEmpleado(@RequestBody Empleado empleado) {
        return ResponseEntity.ok(empleadoService.guardar(empleado));
    }
}
```

### Frontend - Hook para obtener empleados
```javascript
import axios from 'axios';
import { useState, useEffect } from 'react';

const useEmpleados = () => {
  const [empleados, setEmpleados] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchEmpleados = async () => {
      try {
        const response = await axios.get(
          `${process.env.REACT_APP_API_URL}/empleados`
        );
        setEmpleados(response.data);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };
    
    fetchEmpleados();
  }, []);

  return { empleados, loading, error };
};
```

## 🎯 Funcionalidades Principales

### Sistema de Empleados
- Registro de empleados con información completa
- Asignación de departamentos y puestos
- Historial de cambios
- Estados: Activo, Permiso, Inactivo

### Gestión de Nóminas
- Cálculo automático de salarios
- Descuentos y bonificaciones
- Generación de reportes de nómina
- Historial de pagos

### Control de Acceso
- Sistema de roles (Admin, Manager, Empleado)
- Permisos granulares
- Auditoría de acciones

## 🔒 Seguridad

- Validación de entrada en frontend y backend
- CORS configurado para comunicación segura
- Preparación para autenticación JWT (extensible)
- Principio de mínimos privilegios

## 📊 Base de Datos

Entidades principales:
- **Empleados**: DNI, nombre, email, teléfono, departamento, salario
- **Departamentos**: Código, nombre, descripción
- **Nóminas**: Empleado, mes, año, salario bruto, descuentos, neto
- **Usuarios**: Credenciales, rol, permisos

## 🧪 Testing

Para ejecutar pruebas en el backend:
```bash
cd rh_system_spring
mvn test
```

## 📈 Mejoras Futuras

- [ ] Autenticación JWT completa
- [ ] Two-Factor Authentication (2FA)
- [ ] Reportes PDF
- [ ] Dashboard con gráficos
- [ ] Exportación de datos (Excel, CSV)
- [ ] Sistema de vacaciones y permisos
- [ ] Integración con servicios de email
- [ ] Versionado de API
- [ ] Tests E2E con Selenium
- [ ] Deployment en Docker

## 👨‍💼 Autor

**Carlos Zeta** (CharlyZeta)
- GitHub: [github.com/CharlyZeta](https://github.com/CharlyZeta)
- LinkedIn: [linkedin.com/in/carlos-zeta](https://linkedin.com/in/carlos-zeta)

## 📄 Licencia

Este proyecto está bajo la licencia MIT. Ver el archivo `LICENSE` para más detalles.

## 🤝 Contribuciones

Las contribuciones son bienvenidas. Para cambios importantes:

1. Fork el proyecto
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## 💡 Consejos de Desarrollo

### Backend
```bash
# Compilar sin ejecutar
mvn clean compile

# Ejecutar tests
mvn test

# Generar JAR
mvn clean package

# Ver dependencias
mvn dependency:tree
```

### Frontend
```bash
# Ejecutar linter
npm run lint

# Compilar para producción
npm run build

# Ejecutar tests
npm test
```

## ❓ FAQ

**P: ¿Cómo cambio la contraseña de la base de datos?**
R: Actualiza el archivo `application.properties` en el backend con tus credenciales de MySQL.

**P: ¿El frontend y backend deben ejecutarse en máquinas diferentes?**
R: No, pueden estar en la misma máquina en puertos diferentes (8080 para backend, 3000 para frontend).

**P: ¿Cómo agrego un nuevo módulo?**
R: Sigue el patrón MVC existente: crea un Entity, Repository, Service y Controller en el backend, y los componentes correspondientes en el frontend.

---

⭐ Si te ha sido útil, considera dar una estrella al repositorio. ¡Gracias!
