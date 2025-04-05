# 🚀 Tu primera API con Node.js y npm - Guía práctica

Esta guía te enseñará a crear una API de comidas peruanas utilizando Node.js y npm, siguiendo las mejores prácticas de desarrollo.

## 📋 Lo que aprenderás

1. [Conceptos básicos](#1-conceptos-básicos)
2. [Preparando tu entorno](#2-preparando-tu-entorno)
3. [Inicializando el proyecto](#3-inicializando-el-proyecto)
4. [Creando la estructura básica](#4-creando-la-estructura-básica)
5. [Desarrollando tu API](#5-desarrollando-tu-api)
6. [Probando tu API](#6-probando-tu-api)
7. [Siguientes pasos](#7-siguientes-pasos)

## 1. Conceptos básicos

### ¿Qué es Node.js?

Node.js es un entorno que permite ejecutar JavaScript en el servidor (fuera del navegador). Es ideal para crear APIs y aplicaciones web por su velocidad y eficiencia.

### ¿Qué es npm?

npm (Node Package Manager) es el gestor de paquetes de Node.js. Te permite:
- Instalar librerías (paquetes) que otros desarrolladores han creado
- Gestionar las dependencias de tu proyecto
- Ejecutar scripts y tareas automatizadas

### ¿Qué es una API?

Una API (Application Programming Interface) es como un "mesero" que:
1. Recibe peticiones del cliente
2. Busca/procesa la información solicitada
3. Devuelve una respuesta con los datos

En una API REST, usamos:
- URLs (endpoints) para identificar recursos
- Métodos HTTP (GET, POST, PUT, DELETE) para indicar la acción
- Datos en formato JSON para el intercambio de información

## 2. Preparando tu entorno

### Paso 1: Instala Node.js y npm

Descarga e instala desde [nodejs.org](https://nodejs.org/) (npm viene incluido). Recomendamos usar Node.js 18.x o superior.

Verifica la instalación:
```bash
node --version
npm --version
```

Si ves los números de versión, estás listo para comenzar.

## 3. Inicializando el proyecto

### Paso 1: Crea la carpeta del proyecto

```bash
mkdir peruvian-food-api
cd peruvian-food-api
```

### Paso 2: Inicializa npm (crea el package.json)

```bash
npm init -y
```

> **Nota**: El flag `-y` acepta todos los valores predeterminados. Si prefieres personalizar la información, usa simplemente `npm init`.

### Paso 3: Configura el proyecto para usar módulos ES

Abre `package.json` y añade la propiedad `"type": "module"`:

```json
{
  "name": "peruvian-food-api",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "type": "module",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

> **Importante**: Añadir `"type": "module"` permite usar la sintaxis de importación/exportación de ES6 (`import`/`export`) en lugar de la sintaxis CommonJS (`require`/`module.exports`).

### Paso 4: Instala las dependencias básicas

```bash
# Dependencias para producción
npm install express cors

# Dependencias solo para desarrollo
npm install --save-dev nodemon
```

**¿Qué acabamos de instalar?**
- **express**: Framework para crear servidores web y APIs
- **cors**: Permite que otros sitios accedan a nuestra API
- **nodemon**: Reinicia automáticamente el servidor cuando modificas código (solo en desarrollo)

> **Nota para usuarios de Git**: Si usas control de versiones, crea un archivo `.gitignore` con el siguiente contenido:
> ```
> node_modules/
> .env
> .DS_Store
> ```

## 4. Creando la estructura básica

### Paso 1: Crea la estructura de carpetas

```bash
mkdir -p src/data src/routes
```

Esto creará la siguiente estructura:
```
peruvian-food-api/
├── node_modules/     (creada automáticamente por npm)
├── src/
│   ├── data/         (para almacenar datos de ejemplo)
│   ├── routes/       (para definir las rutas de la API)
│   └── index.js      (lo crearemos a continuación)
└── package.json      (creado por npm init)
```

### Paso 2: Crea un archivo index.js básico

Crea el archivo `src/index.js` con este contenido básico:

```javascript
// Import express
import express from 'express';

// Create Express application
const app = express();
const PORT = 3000;

// Basic test route
app.get('/', (req, res) => {
  res.json({ message: 'The server is running!' });
});

// Start the server
app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});
```

### Paso 3: Configura los scripts en package.json

Ahora que ya tienes un archivo `index.js` básico, actualiza el `package.json` para añadir los scripts que te permitirán ejecutar la aplicación:

```json
"scripts": {
  "start": "node src/index.js",
  "dev": "nodemon src/index.js"
}
```

### Paso 4: Prueba que todo funcione

Ejecuta el servidor:
```bash
npm run dev
```

Verás el mensaje: `Server is running on http://localhost:3000`

Abre tu navegador y visita `http://localhost:3000`. Deberías ver:
```json
{
  "message": "The server is running!"
}
```

> **Solución de problemas**: Si aparece algún error relacionado con la sintaxis de importación, verifica que hayas añadido `"type": "module"` en tu package.json.

## 5. Desarrollando tu API

### Paso 1: Crea los datos de ejemplo (src/data/foods.js)

Crea el archivo `src/data/foods.js`:

```javascript
// Sample data of Peruvian foods
const foods = [
  {
    id: 1,
    name: "Ceviche",
    category: "main",
    description: "Fresh raw fish cured in citrus juices",
    ingredients: ["fish", "lime juice", "onions", "chili peppers", "sweet potato"],
    isSpicy: true,
    imageUrl: "https://upload.wikimedia.org/wikipedia/commons/7/78/Cebiche_de_corvina.JPG"
  },
  {
    id: 2,
    name: "Lomo Saltado",
    category: "main",
    description: "Stir-fried beef with vegetables and french fries",
    ingredients: ["beef", "tomatoes", "onions", "soy sauce", "potatoes"],
    isSpicy: false,
    imageUrl: "https://upload.wikimedia.org/wikipedia/commons/c/ca/Lomo-saltado-perudelights.jpg"
  },
  {
    id: 3,
    name: "Ají de Gallina",
    category: "main",
    description: "Creamy chicken stew with yellow chili pepper sauce",
    ingredients: ["chicken", "yellow chili pepper", "bread", "milk", "cheese"],
    isSpicy: true,
    imageUrl: "https://upload.wikimedia.org/wikipedia/commons/thumb/e/e1/Aj%C3%AD_de_gallina.jpg/800px-Aj%C3%AD_de_gallina.jpg"
  },
  {
    id: 4,
    name: "Mazamorra Morada",
    category: "dessert",
    description: "Purple corn pudding with fruit and spices",
    ingredients: ["purple corn", "fruits", "cinnamon", "cloves", "sugar"],
    isSpicy: false,
    imageUrl: "https://upload.wikimedia.org/wikipedia/commons/9/91/Lima_%28Per%C3%BA%29_-_Restaurante_El_Embrujo_-_Mazamorra_morada.jpg"
  },
  {
    id: 5,
    name: "Chicha Morada",
    category: "drink",
    description: "Sweet drink made from purple corn",
    ingredients: ["purple corn", "pineapple", "apple", "cinnamon", "cloves"],
    isSpicy: false,
    imageUrl: "https://upload.wikimedia.org/wikipedia/commons/f/fa/Chicha_Morada_2017.jpg"
  }
];

// Export the data
export default foods;
```

### Paso 2: Crea las rutas (src/routes/foodRoutes.js)

Crea el archivo `src/routes/foodRoutes.js`:

```javascript
// Import express and create router
import express from 'express';
import foods from '../data/foods.js';

// Create router
const foodRouter = express.Router();

// GET all foods with optional filtering
foodRouter.get('/', (req, res) => {
  const { search, category, isSpicy } = req.query;
  let filteredFoods = foods;

  // Filter by search term if provided
  if (search) {
    filteredFoods = filteredFoods.filter(food => 
      food.name.toLowerCase().includes(search.toLowerCase())
    );
  }
  
  // Filter by category if provided
  if (category) {
    filteredFoods = filteredFoods.filter(food => 
      food.category.toLowerCase() === category.toLowerCase()
    );
  }
  
  // Filter by spiciness if provided
  if (isSpicy !== undefined) {
    // Convert string to boolean - !! coerces to boolean
    filteredFoods = filteredFoods.filter(food => 
      food.isSpicy === (isSpicy === 'true')
    );
  }
  
  res.json(filteredFoods);
});

// GET food by ID
foodRouter.get('/:id', (req, res) => {
  // Convert id from string to number using unary + operator
  const id = +req.params.id;
  
  // Find food with matching id
  const food = foods.find(food => food.id === id);
  
  // Return 404 if food not found
  if (!food) {
    return res.status(404).json({ message: 'Food not found' });
  }
  
  // Return the found food
  res.json(food);
});

// Export the router
export default foodRouter;
```

### Paso 3: Actualiza el archivo principal (src/index.js)

Ahora reemplaza el contenido del archivo `src/index.js` con lo siguiente:

```javascript
// Imports
import express from 'express';
import cors from 'cors';
import foodRouter from './routes/foodRoutes.js';

// Create Express application
const app = express();
const PORT = 3000;

// Apply middleware
app.use(cors());         // Enable CORS for all routes
app.use(express.json()); // Parse JSON request bodies

// Home route
app.get('/', (req, res) => {
  res.json({
    message: "Welcome to the Peruvian Food API",
    endpoints: {
      getAllFoods: "/api/foods",
      getFoodById: "/api/foods/1",
      filterFoods: "/api/foods?search=chi",
      combinedFilters: "/api/foods?search=gallina&category=main&isSpicy=true"
    }
  });
});

// Register food routes
app.use('/api/foods', foodRouter);

// Handle 404 errors
app.use((req, res) => {
  res.status(404).json({ message: "Route not found" });
});

// Start the server
app.listen(PORT, () => {
  console.log(`Server is running on http://localhost:${PORT}`);
});
```

> **Nota**: Cuando usas módulos ES, es necesario incluir la extensión `.js` en las importaciones de archivos locales.
>
> **¿Qué es middleware?** En Express, middleware son funciones intermedias que tienen acceso a los objetos de petición (req), respuesta (res) y a la siguiente función middleware (next). Estas funciones pueden ejecutar código, modificar los objetos de petición y respuesta, y finalizar el ciclo de petición-respuesta. En este caso, `cors()` permite peticiones de otros dominios y `express.json()` permite recibir datos en formato JSON.

## 6. Probando tu API

### Paso 1: Inicia el servidor (si no está en ejecución)

```bash
npm run dev
```

Verás el mensaje: `Server is running on http://localhost:3000`

### Paso 2: Prueba los endpoints

Abre tu navegador o usa una herramienta como Postman para probar:

1. **Página inicial**: `http://localhost:3000/`
2. **Todas las comidas**: `http://localhost:3000/api/foods`
3. **Comida por ID**: `http://localhost:3000/api/foods/1`
4. **Buscar por nombre**: `http://localhost:3000/api/foods?search=chi`
5. **Filtrar por categoría**: `http://localhost:3000/api/foods?category=dessert`
6. **Filtrar por picante**: `http://localhost:3000/api/foods?isSpicy=true`
7. **Filtro por categoría y picante**: `http://localhost:3000/api/foods?category=main&isSpicy=true`
8. **Búsqueda combinada completa**: `http://localhost:3000/api/foods?search=gallina&category=main&isSpicy=true`

> **Tip**: Para probar mejor tus endpoints, considera usar [Postman](https://www.postman.com/downloads/) o [Insomnia](https://insomnia.rest/download), que son herramientas diseñadas para probar APIs.

## 7. Siguientes pasos

¡Felicidades! Has creado tu primera API RESTful con Node.js y npm. A continuación, puedes mejorarla:

### Mejoras sugeridas:

1. **Implementar operaciones CRUD completas**:
   ```javascript
   // POST example to create a new food
   foodRouter.post('/', (req, res) => {
     const { name, category, description, ingredients, isSpicy, imageUrl } = req.body;
     
     // Validate required fields
     if (!name || !category) {
       return res.status(400).json({ message: 'Name and category are required' });
     }
     
     // Create new food with auto-incremented ID
     const newFood = {
       id: foods.length + 1,
       name,
       category,
       description,
       ingredients: ingredients || [],
       isSpicy: isSpicy || false,
       imageUrl: imageUrl || ''
     };
     
     // Add to collection
     foods.push(newFood);
     
     // Return created food with 201 status
     res.status(201).json(newFood);
   });
   ```

2. **Separar la lógica en controladores**:
   ```javascript
   // src/controllers/foodController.js
   import foods from '../data/foods.js';
   
   // Get all foods with filtering
   export const getAllFoods = (req, res) => {
     const { search, category, isSpicy } = req.query;
     let filteredFoods = foods;
   
     if (search) filteredFoods = filteredFoods.filter(food => 
       food.name.toLowerCase().includes(search.toLowerCase())
     );
     
     // More filtering logic...
     
     res.json(filteredFoods);
   };
   
   // Get food by ID
   export const getFoodById = (req, res) => {
     const id = +req.params.id;
     const food = foods.find(food => food.id === id);
     
     if (!food) return res.status(404).json({ message: 'Food not found' });
     
     res.json(food);
   };
   ```

3. **Conectar con una base de datos**:
   En un entorno de producción, deberías usar una base de datos real en lugar de datos en memoria.

## 📚 Glosario rápido de npm

### Tipos de dependencias

- **Dependencias de producción**: Necesarias para que la aplicación funcione.
  ```bash
  npm install express
  ```

- **Dependencias de desarrollo**: Solo necesarias durante el desarrollo.
  ```bash
  npm install --save-dev nodemon
  ```

- **Dependencias globales**: Instaladas en todo el sistema, no solo en el proyecto.
  ```bash
  npm install -g typescript
  ```

### Comandos npm útiles

| Comando                   | Descripción                                             |
| ------------------------- | ------------------------------------------------------- |
| `npm init`                | Inicializa un nuevo proyecto                            |
| `npm install`             | Instala todas las dependencias listadas en package.json |
| `npm run <script>`        | Ejecuta un script definido en package.json              |
| `npm uninstall <paquete>` | Desinstala un paquete                                   |
| `npm update`              | Actualiza todas las dependencias                        |

### ¿Qué es npx?

npx es una herramienta que viene con npm y permite:
- Ejecutar paquetes sin instalarlos globalmente
- Ejecutar comandos desde los módulos del proyecto

Ejemplo:
```bash
npx create-react-app my-app
```

### Convenciones de nomenclatura en Node.js

| Elemento    | Convención                      | Ejemplo                           |
| ----------- | ------------------------------- | --------------------------------- |
| Archivos    | Camel case o kebab case         | `foodRoutes.js`, `food-routes.js` |
| Rutas       | Sustantivos en plural           | `/api/foods`, `/api/users`        |
| Controllers | Nombre de modelo + función      | `foodController.js`               |
| Variables   | Descriptivas según su propósito | `foodRouter`, `app`               |
| Funciones   | Verbos + sustantivo             | `getAllFoods`, `getFoodById`      |

---

Esta guía te ha dado los primeros pasos para crear APIs con Node.js. A medida que te sientas más cómodo, puedes explorar características más avanzadas como autenticación, bases de datos y despliegue en la nube. ¡Buena suerte en tu viaje como desarrollador backend! 🚀