# 🛒 Comparativa entre Bases de Datos SQL y NoSQL en un E-commerce

## 🧾 Presentación

El presente trabajo tiene como propósito analizar y comparar el funcionamiento de las bases de datos **SQL** y **NoSQL**, mediante la simulación de un entorno de **comercio electrónico (e-commerce)**.  
Se crearon dos modelos de bases de datos que representan la gestión de **usuarios, productos y pedidos**, con el fin de evidenciar las ventajas y desventajas que cada enfoque ofrece según las necesidades de una empresa moderna.

---

## 💡 Introducción

En la actualidad, las organizaciones requieren sistemas de información que sean **eficientes, escalables y capaces de procesar datos en tiempo real**.  
En este contexto, los modelos de bases de datos **SQL** y **NoSQL** representan dos paradigmas de almacenamiento ampliamente utilizados.  

Las bases de datos **SQL** destacan por su estructura rígida, su modelo relacional y su cumplimiento estricto de las propiedades **ACID** (Atomicidad, Consistencia, Aislamiento y Durabilidad).  
Por otro lado, las bases **NoSQL** ofrecen flexibilidad, escalabilidad horizontal y una mayor capacidad para manejar datos semiestructurados o no estructurados, como los que se generan en sistemas web y móviles.

El caso práctico que se desarrolla a continuación corresponde a un **e-commerce** que maneja usuarios, productos y pedidos.  
Este escenario permite comparar cómo se comportan ambos enfoques en la gestión de información dinámica y con altas demandas de consulta.

---

## ⚙️ Desarrollo de la Actividad

### 🧩 Modelo NoSQL — MongoDB (E-commerce híbrido)

Para el modelo NoSQL se utilizó **MongoDB**, aprovechando su naturaleza documental y su facilidad para escalar horizontalmente.  
Se creó una base de datos llamada `ecommerce_hibrido` con tres colecciones principales: `usuarios`, `productos` y `pedidos`.  
A continuación se presenta el script utilizado en formato híbrido, donde los pedidos contienen tanto la referencia al producto (`producto_id`) como los datos embebidos (`nombre`, `precio_compra` y `cantidad`).

```javascript
/* global use, db */

// Seleccionar la base de datos
use('ecommerce_hibrido');


// Crear colecciones
db.createCollection('usuarios');
db.createCollection('productos');
db.createCollection('pedidos');

// Insertar usuarios
db.usuarios.insertMany([
  { _id: ObjectId(), nombre: "Carlos", correo: "carlos@example.com" },
  { _id: ObjectId(), nombre: "Ana", correo: "ana@example.com" },
  { _id: ObjectId(), nombre: "Luis", correo: "luis@example.com" },
  { _id: ObjectId(), nombre: "María", correo: "maria@example.com" },
  { _id: ObjectId(), nombre: "Pedro", correo: "pedro@example.com" },
  { _id: ObjectId(), nombre: "Sofía", correo: "sofia@example.com" },
  { _id: ObjectId(), nombre: "Jorge", correo: "jorge@example.com" },
  { _id: ObjectId(), nombre: "Laura", correo: "laura@example.com" },
  { _id: ObjectId(), nombre: "Andrés", correo: "andres@example.com" },
  { _id: ObjectId(), nombre: "Lucía", correo: "lucia@example.com" }
]);


const productos = [
  { _id: ObjectId(), nombre: "Laptop Lenovo", precio: 5800000, stock: 15, categoria: "Electrónica" },
  { _id: ObjectId(), nombre: "Camiseta Nike", precio: 300000, stock: 50, categoria: "Ropa" },
  { _id: ObjectId(), nombre: "Silla de Oficina", precio: 1200000, stock: 20, categoria: "Hogar" },
  { _id: ObjectId(), nombre: "Pelota Adidas", precio: 180000, stock: 35, categoria: "Deportes" },
  { _id: ObjectId(), nombre: "Audífonos Sony", precio: 1300000, stock: 25, categoria: "Electrónica" },
  { _id: ObjectId(), nombre: "Libro MongoDB", precio: 120000, stock: 100, categoria: "Libros" },
  { _id: ObjectId(), nombre: "Teclado Mecánico", precio: 32000, stock: 30, categoria: "Electrónica" }
];
db.productos.insertMany(productos);;


// Insertar pedido (modelo híbrido)
db.pedidos.insertMany([
  {
    usuario: "Carlos",
    fecha: new Date(),
    productos: [
      { producto_id: productos[0]._id, 
        nombre: productos[0].nombre, 
        precio_compra: productos[0].precio, 
        cantidad: 1 },
      { producto_id: productos[1]._id, 
        nombre: productos[1].nombre, 
        precio_compra: productos[1].precio, 
        cantidad: 2 }
    ],
    total: productos[0].precio + (productos[1].precio * 2)
  },
  {
    usuario: "Ana",
    fecha: new Date(),
    productos: [
      { producto_id: productos[2]._id, 
        nombre: productos[2].nombre, 
        precio_compra: productos[2].precio, 
        cantidad: 1 }
    ],
    total: productos[2].precio
  },
  {
    usuario: "Luis",
    fecha: new Date(),
    productos: [
      { producto_id: productos[3]._id, 
        nombre: productos[3].nombre, 
        precio_compra: productos[3].precio, 
        cantidad: 1 },
      { producto_id: productos[4]._id, 
        nombre: productos[4].nombre, 
        precio_compra: productos[4].precio, 
        cantidad: 1 }
    ],
    total: productos[4].precio + productos[3].precio
  },
  {
    usuario: "María",
    fecha: new Date(),
    productos: [
      { producto_id: productos[5]._id, 
        nombre: productos[5].nombre, 
        precio_compra: productos[5].precio, 
        cantidad: 2 }
    ],
    total: productos[5].precio * 2
  },
  {
    usuario: "Pedro",
    fecha: new Date(),
    productos: [
      { producto_id: productos[6]._id, 
        nombre: productos[6].nombre, 
        precio_compra: productos[6].precio, 
        cantidad: 1 }
    ],
    total: productos[6].precio
  },
  {
    usuario: "Sofía",
    fecha: new Date(),
    productos: [
      { producto_id: productos[0]._id, 
        nombre: productos[0].nombre, 
        precio_compra: productos[0].precio, 
        cantidad: 1 }
    ],
    total: productos[0].precio
  },
  {
    usuario: "Jorge",
    fecha: new Date(),
    productos: [
      { producto_id: productos[4]._id, 
        nombre: productos[4].nombre, 
        precio_compra: productos[4].precio, 
        cantidad: 1 },
      { producto_id: productos[5]._id, 
        nombre: productos[5].nombre, 
        precio_compra: productos[5].precio, 
        cantidad: 1 }
    ],
    total: productos[4].precio +productos[5].precio
  },
  {
    usuario: "Laura",
    fecha: new Date(),
    productos: [
      { producto_id: productos[1]._id, 
        nombre: productos[1].nombre, 
        precio_compra: productos[1].precio, 
        cantidad: 3 }
    ],
    total: productos[1].precio * 3
  },
  {
    usuario: "Andrés",
    fecha: new Date(),
    productos: [
      { producto_id: productos[2]._id, 
        nombre: productos[2].nombre, 
        precio_compra: productos[2].precio, 
        cantidad: 1 },
      { producto_id: productos[6]._id, 
        nombre: productos[6].nombre, 
        precio_compra: productos[6].precio, 
        cantidad: 1 }
    ],
    total: productos[2].precio + productos[6].precio
  },
  {
    usuario: "Lucía",
    fecha: new Date(),
    productos: [
      { producto_id: productos[3]._id, 
        nombre: productos[3].nombre, 
        precio_compra: productos[3].precio, 
        cantidad: 2 }
    ],
    total: productos[3].precio * 2
  },
  // adicionales (para llegar a 25)
  { usuario: "Carlos", 
    fecha: new Date(), 
    productos: [{ 
        producto_id: productos[5]._id, 
        nombre: productos[5].nombre, 
        precio_compra: productos[5].precio, 
        cantidad: 1 }
    ], 
        total: productos[5].precio },
  { usuario: "Ana", 
    fecha: new Date(), 
    productos: [
        { producto_id: productos[1]._id, 
            nombre: productos[1].nombre, 
            precio_compra: productos[1].precio, 
            cantidad: 2 }
        ], 
        total: productos[1].precio * 2 },
  { usuario: "Luis", 
    fecha: new Date(), 
    productos: [
        { producto_id: productos[0]._id, 
            nombre: productos[0].nombre, 
            precio_compra: productos[0].precio, 
            cantidad: 1 }
        ], 
        total: productos[0].precio },
  { usuario: "María", 
    fecha: new Date(), 
    productos: [
        { producto_id: productos[4]._id, 
            nombre: productos[4].nombre, 
            precio_compra: productos[4].precio, 
            cantidad: 2 }
        ], 
        total: productos[4].precio * 2 },
  { usuario: "Pedro", 
    fecha: new Date(), 
    productos: [
        { producto_id: productos[6]._id, 
            nombre: productos[6].nombre, 
            precio_compra: productos[6].precio, 
            cantidad: 1 }
        ], 
        total: productos[6].precio },
  { usuario: "Sofía", 
    fecha: new Date(), 
    productos: [
        { producto_id: productos[2]._id, 
            nombre: productos[2].nombre, 
            precio_compra: productos[2].precio, 
            cantidad: 2 }
        ], 
        total: productos[2].precio * 2 },
  { usuario: "Jorge", 
    fecha: new Date(), 
    productos: [
        { producto_id: productos[3]._id, 
            nombre: productos[3].nombre, 
            precio_compra: productos[3].precio, 
            cantidad: 3 }
        ], 
        total: productos[3].precio * 3 },
  { usuario: "Laura", 
    fecha: new Date(), 
    productos: [
        { producto_id: productos[0]._id, 
            nombre: productos[0].nombre, 
            precio_compra: productos[0].precio, 
            cantidad: 1 }
        ], 
        total: productos[0].precio},
  { usuario: "Andrés", 
    fecha: new Date(), 
    productos: [
        { producto_id: productos[5]._id, 
            nombre: productos[5].nombre, 
            precio_compra: productos[5].precio, 
            cantidad: 4 }
        ], 
        total: productos[5].precio * 4 },
  { usuario: "Lucía", 
    fecha: new Date(), 
    productos: [
        { producto_id: productos[4]._id, 
            nombre: productos[4].nombre, 
            precio_compra: productos[4].precio, 
            cantidad: 1 }
        ], 
        total: productos[4].precio},
  { usuario: "Carlos", 
    fecha: new Date(), 
    productos: [
        { producto_id: productos[1]._id, 
            nombre: productos[1].nombre, 
            precio_compra: productos[1].precio, 
            cantidad: 1 }
        ], 
        total: productos[1].precio},
  { usuario: "Ana", 
    fecha: new Date(), 
    productos: [
        { producto_id: productos[3]._id, 
            nombre: productos[3].nombre, 
            precio_compra: productos[3].precio, 
            cantidad: 2 }
        ], 
        total: productos[3].precio * 2 },
  { usuario: "Luis", 
    fecha: new Date(), 
    productos: [
        { producto_id: productos[2]._id, 
            nombre: productos[2].nombre, 
            precio_compra: productos[2].precio, 
            cantidad: 1 }
        ], 
        total: productos[2].precio },
  { usuario: "María", 
    fecha: new Date(), 
    productos: [
        { producto_id: productos[6]._id, 
            nombre: productos[6].nombre, 
            precio_compra: productos[6].precio, 
            cantidad: 2 }
        ], 
        total: productos[6].precio * 2 },
  { usuario: "Pedro", 
    fecha: new Date(), 
    productos: [
        { producto_id: productos[0]._id, 
            nombre: productos[0].nombre, 
            precio_compra: productos[0].precio, 
            cantidad: 1 }
        ], 
        total: productos[0].precio}
]);
```
---

## 📊 Análisis comparativo

| Criterio | SQL | NoSQL |
|-----------|------|--------|
| **Estructura** | Rígida, basada en tablas y relaciones. | Flexible, basada en documentos JSON. |
| **Escalabilidad** | Vertical (más hardware). | Horizontal (más nodos). |
| **Transacciones** | Cumple propiedades ACID. | Eventualmente consistente. |
| **Velocidad de consulta** | Alta en estructuras pequeñas. | Óptima en grandes volúmenes de datos. |
| **Adecuado para** | Finanzas, contabilidad, inventarios. | Comercio electrónico, redes sociales, big data. |

---
# 📊 Consultas de Productos Vendidos Agrupados — MongoDB

En esta sección se presentan consultas prácticas para obtener reportes de productos vendidos, agrupaciones por categorías y usuarios, utilizando el lenguaje de agregación de **MongoDB**.  

---

## 🧮 1. Total de productos vendidos (agrupados por nombre)

**Consulta MongoDB**

```javascript
use("ecommerce_hibrido");
db.pedidos.aggregate([
  { $unwind: "$productos" },
  { $group: {
      _id: "$productos.nombre",
      total_vendido: { $sum: "$productos.cantidad" },
      ingresos_totales: { $sum: { $multiply: [ "$productos.precio_compra", "$productos.cantidad" ] } }
  }},
  { $sort: { ingresos_totales: -1 } }
]);
```

**Ejemplo de resultado:**

```json
[
  {
    "_id": "Laptop Lenovo",
    "total_vendido": 5,
    "ingresos_totales": 29000000
  },
  {
    "_id": "Audífonos Sony",
    "total_vendido": 5,
    "ingresos_totales": 6500000
  },
  {
    "_id": "Silla de Oficina",
    "total_vendido": 5,
    "ingresos_totales": 6000000
  },
  {
    "_id": "Camiseta Nike",
    "total_vendido": 8,
    "ingresos_totales": 2400000
  },
  {
    "_id": "Pelota Adidas",
    "total_vendido": 8,
    "ingresos_totales": 1440000
  },
  {
    "_id": "Libro MongoDB",
    "total_vendido": 8,
    "ingresos_totales": 960000
  },
  {
    "_id": "Teclado Mecánico",
    "total_vendido": 5,
    "ingresos_totales": 160000
  }
]
```

**Explicación:**  
- `$unwind` separa los productos dentro del array `productos` de cada pedido.  
- `$group` agrupa por nombre de producto.  
- Calcula la **cantidad total vendida** y los **ingresos generados**.  
- `$sort` ordena los resultados de mayor a menor venta.

---

## 📊 2. Ventas agrupadas por categoría del producto

**Consulta MongoDB**

```javascript
use("ecommerce_hibrido");
db.pedidos.aggregate([
  { $unwind: "$productos" },
  { $lookup: {
      from: "productos",
      localField: "productos.producto_id",
      foreignField: "_id",
      as: "detalle"
  }},
  { $unwind: "$detalle" },
  { $group: {
      _id: "$detalle.categoria",
      total_vendido: { $sum: "$productos.cantidad" },
      ingresos_categoria: { $sum: { $multiply: [ "$productos.precio_compra", "$productos.cantidad" ] } }
  }},
  { $sort: { ingresos_categoria: -1 } }
]);
```

**Ejemplo de resultado:**

```json
[
  {
    "_id": "Electrónica",
    "total_vendido": 15,
    "ingresos_categoria": 35660000
  },
  {
    "_id": "Hogar",
    "total_vendido": 5,
    "ingresos_categoria": 6000000
  },
  {
    "_id": "Ropa",
    "total_vendido": 8,
    "ingresos_categoria": 2400000
  },
  {
    "_id": "Deportes",
    "total_vendido": 8,
    "ingresos_categoria": 1440000
  },
  {
    "_id": "Libros",
    "total_vendido": 8,
    "ingresos_categoria": 960000
  }
]
```
**Explicación:**  
- Se usa `$lookup` para enlazar la colección de productos y obtener la categoría.  
- Se agrupan las ventas por categoría y se suman ingresos.

---

## ✅ Conclusiones

- **MongoDB (NoSQL)** ofrece mayor flexibilidad y escalabilidad, ideal para sistemas de e-commerce con información dinámica.  
- **SQL** es recomendable para operaciones que requieren integridad y control de transacciones.  
- Un **modelo híbrido** que combine ambas tecnologías aprovecha lo mejor de los dos mundos:  
  - NoSQL para operaciones del cliente y productos.  
  - SQL para pagos y reportes contables.  

---

## 📚 Bibliografía

- MongoDB Inc. (2025). *MongoDB Manual – Data Modeling Concepts.*  
- Microsoft Docs. (2024). *SQL vs NoSQL databases.*  
- Red Hat. (2024). *What is NoSQL?*  
- Amazon Web Services. (2025). *Building Scalable E-commerce Architectures.*


