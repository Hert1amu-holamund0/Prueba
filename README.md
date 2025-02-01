<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Sistema de Votación con Burbujas</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 20px;
      background-color: #f9fafb;
    }

    h1 {
      color: #333;
    }

    .category-container {
      margin: 20px 0;
    }

    .category {
      color: white;
      padding: 10px 20px;
      border-radius: 50px;
      font-size: 16px;
      cursor: pointer;
      display: inline-block;
      transition: all 0.2s ease;
    }

    .bubble-container {
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
      justify-content: center;
      max-width: 600px;
      margin-top: 20px;
    }

    .bubble {
      background-color: #f472b6;
      color: white;
      padding: 10px 15px;
      border-radius: 50%;
      cursor: pointer;
      font-size: 14px;
      transition: all 0.2s ease;
      text-align: center;
    }

    .bubble.selected {
      background-color: #16a34a;
      transform: scale(1.2);
    }

    .add-bubble {
      background-color: #eab308;
      color: white;
      padding: 10px 15px;
      border-radius: 50%;
      cursor: pointer;
      font-size: 14px;
      text-align: center;
      transition: all 0.2s ease;
    }

    .add-bubble:hover {
      background-color: #d97706;
    }

    .category:hover, .bubble:hover {
      transform: scale(1.1);
    }
  </style>
</head>
<body>
  <h1>Sistema de Votación con Burbujas</h1>

  <div id="categories-container"></div>
  <div class="bubble-container">
    <div class="add-bubble" onclick="addNewResponse()">+ Añadir Respuesta</div>
  </div>

  <script>
    // Array para guardar las categorías y sus respuestas
    let categories = [];

    // Función para pedir las categorías y sus respuestas
    function createCategories() {
      let numCategories = prompt("¿Cuántas categorías deseas crear?");
      if (numCategories && !isNaN(numCategories)) {
        numCategories = parseInt(numCategories);
        for (let i = 0; i < numCategories; i++) {
          let categoryName = prompt(`Ingresa el nombre de la categoría ${i + 1}:`);
          let categoryColor = getRandomColor(); // Color aleatorio para la categoría
          let numResponses = prompt(`¿Cuántas respuestas quieres para la categoría "${categoryName}"?`);
          numResponses = parseInt(numResponses);
          let responses = [];
          
          for (let j = 0; j < numResponses; j++) {
            let response = prompt(`Ingresa la respuesta ${j + 1} para la categoría "${categoryName}":`);
            responses.push(response);
          }

          categories.push({
            name: categoryName,
            color: categoryColor,
            responses: responses
          });
        }
        renderCategories();
      } else {
        alert("Por favor ingresa un número válido.");
      }
    }

    // Función para renderizar las categorías y respuestas
    function renderCategories() {
      const container = document.getElementById('categories-container');
      container.innerHTML = ''; // Limpiar contenido previo

      categories.forEach((category, index) => {
        const categoryElement = document.createElement('div');
        categoryElement.classList.add('category');
        categoryElement.style.backgroundColor = category.color;
        categoryElement.textContent = `Categoría: ${category.name}`;
        categoryElement.ondblclick = function() { increaseCategorySize(categoryElement); };
        container.appendChild(categoryElement);

        const bubbleContainer = document.createElement('div');
        bubbleContainer.classList.add('bubble-container');
        category.responses.forEach(response => {
          const bubble = document.createElement('div');
          bubble.classList.add('bubble');
          bubble.textContent = response;
          bubble.onclick = function() { toggleVote(bubble); };
          bubbleContainer.appendChild(bubble);
        });

        container.appendChild(bubbleContainer);
      });
    }

    // Función para alternar la selección de las burbujas (voto)
    function toggleVote(bubble) {
      bubble.classList.toggle('selected');
    }

    // Función para aumentar el tamaño de la categoría al hacer doble clic
    function increaseCategorySize(categoryElement) {
      const currentSize = parseInt(window.getComputedStyle(categoryElement).fontSize);
      categoryElement.style.fontSize = `${currentSize + 3}px`;
    }

    // Función para añadir una nueva respuesta personalizada en cualquier categoría
    function addNewResponse() {
      const newResponse = prompt("Ingresa tu nueva respuesta:");
      if (newResponse) {
        const categoryIndex = prompt(`¿A qué categoría deseas añadir la respuesta?\n(Ingresa un número del 1 al ${categories.length})`);
        const category = categories[categoryIndex - 1];
        if (category) {
          category.responses.push(newResponse);
          renderCategories();
        } else {
          alert("Categoría no válida.");
        }
      }
    }

    // Función para generar un color aleatorio para cada categoría
    function getRandomColor() {
      const letters = '0123456789ABCDEF';
      let color = '#';
      for (let i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      return color;
    }

    // Iniciar el proceso de creación de categorías al cargar la página
    window.onload = createCategories;
  </script>
</body>
</html>
