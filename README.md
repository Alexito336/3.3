# 3.3
html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Gestión de Productos</title>
  <style>
    body {
      background-color: black;
      color: navy;
      font-family: Arial, sans-serif;
      padding: 20px;
    }

    h1 {
      text-align: center;
      color: navy;
    }

    form {
      margin-bottom: 20px;
      background: #111;
      padding: 20px;
      border: 2px solid navy;
      border-radius: 10px;
      max-width: 500px;
      margin: auto;
    }

    label {
      display: block;
      margin: 10px 0 5px;
      color: navy;
    }

    input {
      width: 100%;
      padding: 8px;
      border: 1px solid navy;
      background-color: #222;
      color: navy;
      border-radius: 5px;
    }

    button {
      margin-top: 15px;
      padding: 10px 20px;
      border: 2px solid navy;
      background-color: navy;
      color: white;
      cursor: pointer;
      border-radius: 5px;
      transition: background-color 0.3s;
    }

    button:hover {
      background-color: white;
      color: navy;
    }

    table {
      width: 90%;
      margin: 20px auto;
      border-collapse: collapse;
      background: #111;
      color: navy;
      border: 2px solid navy;
    }

    th, td {
      border: 1px solid navy;
      padding: 10px;
      text-align: center;
    }

    th {
      background-color: navy;
      color: white;
    }

    .action-btn {
      padding: 5px 10px;
      margin: 0 3px;
      border-radius: 5px;
      cursor: pointer;
      border: 1px solid navy;
    }

    .delete-btn {
      background-color: crimson;
      color: white;
    }

    .delete-btn:hover {
      background-color: darkred;
    }
  </style>
</head>
<body>
  <h1>Gestión de Productos</h1>

  <form id="formProducto">
    <label for="nombre">Nombre:</label>
    <input type="text" id="nombre" required>

    <label for="precio">Precio:</label>
    <input type="number" id="precio" required min="0">

    <label for="cantidad">Cantidad:</label>
    <input type="number" id="cantidad" required min="1">

    <button type="submit">Agregar Producto</button>
  </form>

  <table id="tablaProductos">
    <thead>
      <tr>
        <th>ID</th>
        <th>Nombre</th>
        <th>Precio</th>
        <th>Cantidad</th>
        <th>Acciones</th>
      </tr>
    </thead>
    <tbody>
      <!-- Productos se cargarán aquí -->
    </tbody>
  </table>

  <script>
    let productos = [
      { id: 1, nombre: "Laptop", precio: 1200, cantidad: 5 },
      { id: 2, nombre: "Teclado", precio: 250, cantidad: 10 },
    ];
    let nextId = 3;

    const form = document.getElementById('formProducto');
    const tabla = document.getElementById('tablaProductos').querySelector('tbody');

    function renderTabla() {
      tabla.innerHTML = "";
      productos.forEach(producto => {
        const fila = document.createElement("tr");

        fila.innerHTML = `
          <td>${producto.id}</td>
          <td contenteditable="true" oninput="actualizarProducto(${producto.id}, 'nombre', this.innerText)">${producto.nombre}</td>
          <td contenteditable="true" oninput="actualizarProducto(${producto.id}, 'precio', this.innerText)">${producto.precio}</td>
          <td contenteditable="true" oninput="actualizarProducto(${producto.id}, 'cantidad', this.innerText)">${producto.cantidad}</td>
          <td><button class="action-btn delete-btn" onclick="eliminarProducto(${producto.id})">Eliminar</button></td>
        `;
        tabla.appendChild(fila);
      });
    }

    function actualizarProducto(id, campo, valor) {
      const producto = productos.find(p => p.id === id);
      if (!producto) return;

      if (campo === "precio" || campo === "cantidad") {
        const num = parseFloat(valor);
        if (!isNaN(num)) {
          producto[campo] = num;
        }
      } else {
        producto[campo] = valor;
      }
    }

    function eliminarProducto(id) {
      productos = productos.filter(p => p.id !== id);
      renderTabla();
    }

    form.addEventListener("submit", function (e) {
      e.preventDefault();

      const nombre = document.getElementById('nombre').value.trim();
      const precio = parseFloat(document.getElementById('precio').value);
      const cantidad = parseInt(document.getElementById('cantidad').value);

      if (!nombre || isNaN(precio) || isNaN(cantidad)) return;

      productos.push({
        id: nextId++,
        nombre,
        precio,
        cantidad
      });

      form.reset();
      renderTabla();
    });

    // Render inicial
    renderTabla();
  </script>
</body>
</html>
