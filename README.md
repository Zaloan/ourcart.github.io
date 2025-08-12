# ourcart.github.io
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Our Cart</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body { background: linear-gradient(to right, #f8f9fa, #e9ecef); }
        .fade-in { animation: fadeIn 0.4s ease-in-out; }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-5px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .titulo {
            font-family: 'Segoe UI', sans-serif;
            font-weight: bold;
        }
    </style>
</head>
<body>

<div class="container py-5">
    <h1 class="text-center mb-4 titulo text-primary">🛒 Our Cart</h1>

    <div class="card shadow-lg p-4">
        <div class="input-group mb-3">
            <input id="item" type="text" class="form-control" placeholder="Añadir producto..." autofocus>
            <button class="btn btn-primary" onclick="agregarItem()">
                <i class="bi bi-plus-circle"></i> Agregar
            </button>
        </div>

        <ul id="lista" class="list-group mb-3"></ul>

        <div class="text-center">
            <button class="btn btn-success me-2" onclick="exportarLista()">
                <i class="bi bi-download"></i> Exportar Lista
            </button>
            <label class="btn btn-info text-white">
                <i class="bi bi-upload"></i> Importar Lista
                <input type="file" id="importarArchivo" hidden onchange="importarLista(event)">
            </label>
        </div>
    </div>
</div>

<!-- Bootstrap Icons -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.10.5/font/bootstrap-icons.css">

<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/12.1.0/firebase-app.js";
  import { getDatabase, ref, push, onValue, remove } from "https://www.gstatic.com/firebasejs/12.1.0/firebase-database.js";

  const firebaseConfig = {
    apiKey: "AIzaSyC-OGg7mA3DciS0eb8oOU5ts07eq2WLJjs",
    authDomain: "our-cart-53ae9.firebaseapp.com",
    databaseURL: "https://our-cart-53ae9-default-rtdb.firebaseio.com",
    projectId: "our-cart-53ae9",
    storageBucket: "our-cart-53ae9.firebasestorage.app",
    messagingSenderId: "64844496111",
    appId: "1:64844496111:web:43d11e7dd87791f0ec6f1a",
    measurementId: "G-HX3D440TE2"
  };

  const app = initializeApp(firebaseConfig);
  const db = getDatabase(app);

  const itemInput = document.getElementById("item");
  const listaUl = document.getElementById("lista");

  // Agregar producto
  window.agregarItem = () => {
    const item = itemInput.value.trim();
    if (item) {
      push(ref(db, "lista"), item);
      itemInput.value = "";
    }
  };

  // Eliminar producto
  window.eliminarItem = (key) => {
    remove(ref(db, "lista/" + key));
  };

  // Escuchar lista en tiempo real
  onValue(ref(db, "lista"), (snapshot) => {
    listaUl.innerHTML = "";
    snapshot.forEach((childSnapshot) => {
      const key = childSnapshot.key;
      const value = childSnapshot.val();
      listaUl.innerHTML += `
        <li class="list-group-item d-flex justify-content-between align-items-center fade-in">
          <span>${value}</span>
          <button class="btn btn-danger btn-sm" onclick="eliminarItem('${key}')">
            <i class="bi bi-trash"></i>
          </button>
        </li>`;
    });
  });
</script>

</body>
</html>
