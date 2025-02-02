<!DOCTYPE html>
<html lang="pt-BR">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HenriqueTech</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
        }

        header {
            background-color: #333;
            color: white;
            padding: 15px;
            text-align: center;
        }

        nav {
            background-color: #444;
            color: white;
            padding: 10px;
            text-align: center;
        }

        nav a {
            color: white;
            text-decoration: none;
            margin: 0 15px;
        }

        .container {
            padding: 20px;
            max-width: 1200px;
            margin: auto;
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
        }

        .product {
            background-color: white;
            border-radius: 5px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
            padding: 15px;
            text-align: center;
        }

        .product img {
            width: 500px;
            height: 500px;
            object-fit: cover;
            border-radius: 5px;
        }

        .product h3 {
            color: #333;
        }

        .product p {
            color: #555;
        }

        .btn {
            display: inline-block;
            margin-top: 10px;
            padding: 10px 15px;
            background-color: #25D366;
            color: white;
            text-decoration: none;
            border-radius: 5px;
        }

        .btn:hover {
            background-color: #1ebe5d;
        }

        .back-btn {
            background-color: #555;
        }

        .back-btn:hover {
            background-color: #333;
        }

        .form-group {
            margin-bottom: 15px;
        }

        .form-group label {
            display: block;
            margin-bottom: 5px;
            color: #555;
        }

        .form-group select {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
    </style>
</head>

<body>
    <header>
        <h1>_HenriqueTech </h1>
    </header>

    <nav>
        <a href="#" onclick="showAllProducts()">Início</a>
        <a href="#contato">Contato</a>
    </nav>

    <div class="form-group">
        <label for="productCategory">Categorias:</label>
        <select id="productCategory" onchange="filterProductsByCategory()">
            <!-- As categorias serão carregadas dinamicamente -->
        </select>
    </div>

    <div class="container" id="productList">
        <!-- Produtos carregados automaticamente aparecerão aqui -->
    </div>

    <div id="contato" style="text-align:center; padding:20px;">
        <h2>Contato</h2>
        <p>Entre em contato conosco via WhatsApp:</p>
        <a class="btn" href="https://wa.me/5584996403291" target="_blank">(84) 99640-3291</a>
    </div>

    <script>
        window.onload = function() {
            loadProducts();
            loadCategories();  // Carregar categorias na inicialização
        };

        // Função para carregar os produtos do localStorage
        function loadProducts() {
            const products = JSON.parse(localStorage.getItem('products')) || [];
            displayProducts(products);
        }

        // Função para exibir os produtos na tela
        function displayProducts(products) {
            const productList = document.getElementById('productList');
            productList.innerHTML = '';

            if (products.length === 0) {
                productList.innerHTML = '<p>Nenhum produto adicionado ainda.</p>';
                return;
            }

            products.forEach(product => {
                const productItem = document.createElement('div');
                productItem.classList.add('product');
                productItem.setAttribute('data-category', product.category);

                productItem.innerHTML = `
                    <img src="${product.image}" alt="${product.name}">
                    <h3>${product.name}</h3>
                    <p>Preço: ${product.price}</p>
                    <p>Estoque: ${product.stock}</p>
                    <a class="btn" href="https://wa.me/5584996403291?text=Gostaria%20de%20comprar%20o%20${encodeURIComponent(product.name)}" target="_blank">Comprar via WhatsApp</a>
                `;

                productList.appendChild(productItem);
            });
        }

        // Função para filtrar os produtos pela categoria selecionada
        function filterProductsByCategory() {
            const selectedCategory = document.getElementById('productCategory').value;
            const allProducts = JSON.parse(localStorage.getItem('products')) || [];

            if (selectedCategory === 'all') {
                displayProducts(allProducts);
            } else {
                const filteredProducts = allProducts.filter(product => product.category === selectedCategory);
                displayProducts(filteredProducts);
            }
        }

        // Função para carregar as categorias do localStorage
        function loadCategories() {
            const categories = JSON.parse(localStorage.getItem('categories')) || [];
            const categorySelect = document.getElementById('productCategory');

            categorySelect.innerHTML = '<option value="all">Todas</option>'; // Opção "Todas"
            categories.forEach(category => {
                categorySelect.innerHTML += `<option value="${category}">${category}</option>`;
            });
        }

        // Adiciona o listener de evento de storage para atualizar as categorias em tempo real
        window.addEventListener('storage', function(event) {
            if (event.key === 'categories') {
                loadCategories();  // Atualiza as categorias quando elas mudam no localStorage
            }
        });

        // Função para mostrar todos os produtos ao clicar em "Início"
        function showAllProducts() {
            document.getElementById('productCategory').value = 'all';
            loadProducts();
        }
    </script>
</body>

</html>
