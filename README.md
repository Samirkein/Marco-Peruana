<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Marco Peruana | Soluciones Integrales para la Industria</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f8fafc; /* Light gray background */
        }
        /* Custom scrollbar for a cleaner look */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f1f1;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb {
            background: #888;
            border-radius: 10px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #555;
        }

        /* Chatbot specific styles */
        #chatbot-container {
            position: fixed;
            bottom: 20px;
            right: 20px;
            z-index: 1000;
        }
        #chat-window {
            width: 350px;
            height: 500px;
            background-color: white;
            border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
            display: flex;
            flex-direction: column;
            overflow: hidden;
            transform: translateY(100%);
            opacity: 0;
            transition: transform 0.3s ease-out, opacity 0.3s ease-out;
        }
        #chat-window.open {
            transform: translateY(0);
            opacity: 1;
        }
        #chat-header {
            background-color: #003366;
            color: white;
            padding: 15px 20px;
            border-top-left-radius: 15px;
            border-top-right-radius: 15px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-weight: bold;
        }
        #chat-messages {
            flex-grow: 1;
            padding: 20px;
            overflow-y: auto;
            background-color: #f0f2f5;
        }
        .message {
            margin-bottom: 15px;
            display: flex;
            max-width: 80%;
        }
        .message.user {
            justify-content: flex-end;
            margin-left: auto;
        }
        .message.bot {
            justify-content: flex-start;
            margin-right: auto;
        }
        .message-bubble {
            padding: 10px 15px;
            border-radius: 15px;
            line-height: 1.4;
            word-wrap: break-word;
        }
        .message.user .message-bubble {
            background-color: #007bff; /* Blue for user */
            color: white;
            border-bottom-right-radius: 5px;
        }
        .message.bot .message-bubble {
            background-color: #e2e8f0; /* Light gray for bot */
            color: #333;
            border-bottom-left-radius: 5px;
        }
        #chat-input-container {
            display: flex;
            padding: 15px;
            border-top: 1px solid #e2e8f0;
        }
        #chat-input {
            flex-grow: 1;
            border: 1px solid #cbd5e0;
            border-radius: 20px;
            padding: 10px 15px;
            font-size: 1rem;
            outline: none;
        }
        #chat-send-button {
            background-color: #003366;
            color: white;
            border: none;
            border-radius: 20px;
            padding: 10px 15px;
            margin-left: 10px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        #chat-send-button:hover {
            background-color: #002244;
        }
        #chatbot-fab {
            background-color: #003366;
            color: white;
            width: 60px;
            height: 60px;
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 2rem;
            cursor: pointer;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            transition: transform 0.2s ease;
        }
        #chatbot-fab:hover {
            transform: scale(1.05);
        }
        .loading-dots {
            display: flex;
            align-items: center;
            justify-content: center;
            margin-top: 10px;
        }
        .loading-dots span {
            display: inline-block;
            width: 8px;
            height: 8px;
            background-color: #888;
            border-radius: 50%;
            margin: 0 3px;
            animation: bounce 1.4s infinite ease-in-out both;
        }
        .loading-dots span:nth-child(1) { animation-delay: -0.32s; }
        .loading-dots span:nth-child(2) { animation-delay: -0.16s; }
        .loading-dots span:nth-child(3) { animation-delay: 0s; }

        @keyframes bounce {
            0%, 80%, 100% { transform: translateY(0); }
            40% { transform: translateY(-10px); }
        }

        /* Hero Section with Background Image */
        #inicio {
            background-image: url('https://encrypted-tbn3.gstatic.com/images?q=tbn:ANd9GcSXiW2fZoVg2fZjMwGeEN4r2hntOsrUT40jgkVPjWDnIvyP-Y9l');
            background-size: cover;
            background-repeat: no-repeat;
            background-position: center;
            position: relative; /* Needed for overlay */
        }

        /* Overlay for text readability */
        #inicio::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0, 51, 102, 0.6); /* Dark blue overlay with transparency */
            z-index: 1; /* Ensure overlay is above background image but below text */
            border-radius: inherit; /* Inherit border-radius from parent */
        }

        #inicio .container {
            position: relative; /* Ensure text is above the overlay */
            z-index: 2;
        }

        /* Cart icon styling */
        .cart-icon-container {
            position: relative;
            margin-left: 20px;
        }
        .cart-count {
            position: absolute;
            top: -8px;
            right: -8px;
            background-color: #ef4444; /* Red color for count */
            color: white;
            border-radius: 50%;
            padding: 2px 6px;
            font-size: 0.75rem;
            font-weight: bold;
            min-width: 20px;
            text-align: center;
        }

        /* Modals styles */
        #cart-modal, #project-idea-modal, #service-recommendation-modal {
            background-color: rgba(0, 0, 0, 0.7);
            transition: opacity 0.3s ease-in-out;
        }
        #cart-modal.hidden, #project-idea-modal.hidden, #service-recommendation-modal.hidden {
            opacity: 0;
            pointer-events: none;
        }
        #cart-modal-content, #project-idea-modal-content, #service-recommendation-modal-content {
            transform: translateY(-20px);
            transition: transform 0.3s ease-in-out;
        }
        #cart-modal.hidden #cart-modal-content, #project-idea-modal.hidden #project-idea-modal-content, #service-recommendation-modal.hidden #service-recommendation-modal-content {
            transform: translateY(0);
        }
    </style>
</head>
<body class="text-gray-800">

    <!-- Header Section -->
    <header class="bg-white shadow-md py-4 px-6 md:px-12 sticky top-0 z-50 rounded-b-lg">
        <div class="container mx-auto flex flex-col md:flex-row items-center justify-between">
            <!-- Logo -->
            <a href="#" class="flex items-center mb-4 md:mb-0">
                <img src="https://encrypted-tbn3.gstatic.com/images?q=tbn:ANd9GcSXiW2fZoVg2fZjMwGeEN4r2hntOsrUT40jgkVPjWDnIvyP-Y9l" onerror="this.onerror=null;this.src='https://placehold.co/150x50/003366/FFFFFF?text=MARCO+LOGO';" alt="" class="h-12 rounded-md shadow-sm">
            </a>

            <!-- Navigation -->
            <nav class="flex flex-wrap justify-center md:justify-end space-x-4 md:space-x-8 text-lg font-medium">
                <a href="#inicio" class="text-blue-800 hover:text-blue-600 transition-colors duration-300 px-3 py-2 rounded-md">Inicio</a>
                <a href="#quienes-somos" class="text-gray-700 hover:text-blue-600 transition-colors duration-300 px-3 py-2 rounded-md">Quiénes Somos</a>
                <a href="#productos" class="text-gray-700 hover:text-blue-600 transition-colors duration-300 px-3 py-2 rounded-md">Productos</a>
                <a href="#servicios" class="text-700 hover:text-blue-600 transition-colors duration-300 px-3 py-2 rounded-md">Servicios</a>
                <a href="#blog" class="text-gray-700 hover:text-blue-600 transition-colors duration-300 px-3 py-2 rounded-md">Blog</a>
                <a href="#contacto" class="text-gray-700 hover:text-blue-600 transition-colors duration-300 px-3 py-2 rounded-md">Contacto</a>
                <!-- Cart Icon -->
                <div class="cart-icon-container cursor-pointer" id="open-cart-modal">
                    <svg class="w-8 h-8 text-gray-700 hover:text-blue-600 transition-colors duration-300" fill="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                        <path d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13-2.293 15.293c-.63.63-.182 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z"></path>
                    </svg>
                    <span id="cart-count" class="cart-count">0</span>
                </div>
            </nav>
        </div>
    </header>

    <!-- Hero Section -->
    <section id="inicio" class="relative text-white py-24 md:py-32 overflow-hidden rounded-b-xl shadow-lg">
        <div class="container mx-auto text-center px-6">
            <h1 class="text-4xl md:text-6xl font-extrabold leading-tight mb-6 animate-fade-in-down">
                Soluciones Integrales para la Industria
            </h1>
            <p class="text-lg md:text-2xl mb-10 opacity-90 animate-fade-in-up">
                Líderes en equipos y servicios para los sectores de Pesca y Minería.
            </p>
            <a href="#contacto" class="inline-block bg-yellow-500 hover:bg-yellow-600 text-blue-900 font-bold py-4 px-8 rounded-full shadow-lg transform hover:scale-105 transition-all duration-300 ease-in-out">
                Contáctanos Hoy
            </a>
        </div>
    </section>

    <!-- Quiénes Somos Section -->
    <section id="quienes-somos" class="py-16 md:py-24 bg-white rounded-lg shadow-md mx-6 md:mx-12 my-12">
        <div class="container mx-auto px-6 md:px-12 flex flex-col md:flex-row items-center gap-10">
            <div class="md:w-1/2">
                <h2 class="text-3xl md:text-4xl font-bold text-blue-800 mb-6">Quiénes Somos</h2>
                <p class="text-lg leading-relaxed mb-4">
                    En Marco Peruana, hemos sido pioneros en el desarrollo de soluciones innovadoras y la mejora continua de nuestros servicios desde 1965. Nuestra trayectoria se basa en la confianza de nuestros clientes y en un compromiso inquebrantable con la excelencia.
                </p>
                <p class="text-lg leading-relaxed mb-4">
                    Nos especializamos en ofrecer una amplia gama de equipos y servicios de alta calidad, diseñados para optimizar las operaciones en los sectores de pesca, minería, transporte refrigerado y más. Contamos con un equipo de expertos dedicados a brindar soporte técnico y soluciones personalizadas.
                </p>
                <a href="#contacto" class="inline-block bg-blue-700 hover:bg-blue-800 text-white font-semibold py-3 px-6 rounded-lg shadow-md transition-all duration-300">
                    Conoce más sobre nosotros
                </a>
            </div>
            <div class="md:w-1/2 flex justify-center">
                <img src="https://placehold.co/500x350/E0F2F7/003366?text=Nuestra+Historia" alt="" class="rounded-lg shadow-xl w-full max-w-md">
            </div>
        </div>
    </section>

    <!-- Productos Section -->
    <section id="productos" class="py-16 md:py-24 bg-gray-50 rounded-lg shadow-md mx-6 md:mx-12 my-12">
        <div class="container mx-auto px-6 md:px-12 text-center">
            <h2 class="text-3xl md:text-4xl font-bold text-blue-800 mb-12">Nuestros Productos</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                <!-- Product Card 1 -->
                <div class="bg-white p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1">
                    <img src="https://www.marco.com.pe/wp-content/uploads/2021/09/banners-oleohidraulica_Marco-Peruana.jpg" onerror="this.onerror=null;this.src='https://placehold.co/300x200/B3E0FF/003366?text=Oleohidráulica+Minera';" alt="" class="w-full h-48 object-cover rounded-md mb-6">
                    <h3 class="text-2xl font-semibold text-blue-700 mb-3">Oleohidráulica Minera</h3>
                    <p class="text-gray-600 mb-4">
                        Soluciones hidráulicas robustas y eficientes para la exigente industria minera, garantizando rendimiento y durabilidad.
                    </p>
                    <button class="add-to-cart-btn mt-4 w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-300" data-product-id="prod1" data-product-name="Oleohidráulica Minera">Agregar al Carrito</button>
                </div>
                <!-- Product Card 2 -->
                <div class="bg-white p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1">
                    <img src="https://www.marco.com.pe/wp-content/uploads/2021/09/olehidraulica_Movil.jpg" onerror="this.onerror=null;this.src='https://placehold.co/300x200/B3E0FF/003366?text=Oleohidráulica+Móvil';" alt="" class="w-full h-48 object-cover rounded-md mb-6">
                    <h3 class="text-2xl font-semibold text-blue-700 mb-3">Oleohidráulica Móvil</h3>
                    <p class="text-gray-600 mb-4">
                        Componentes y sistemas hidráulicos para maquinaria móvil, optimizando la productividad en campo.
                    </p>
                    <button class="add-to-cart-btn mt-4 w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-300" data-product-id="prod2" data-product-name="Oleohidráulica Móvil">Agregar al Carrito</button>
                </div>
                <!-- Product Card 3 -->
                <div class="bg-white p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1">
                    <img src="https://www.marco.com.pe/wp-content/uploads/2021/07/banner-FRIO.jpg" onerror="this.onerror=null;this.src='https://placehold.co/300x200/B3E0FF/003366?text=Transporte+Refrigerado';" alt="" class="w-full h-48 object-cover rounded-md mb-6">
                    <h3 class="text-2xl font-semibold text-blue-700 mb-3">Transporte Refrigerado</h3>
                    <p class="text-gray-600 mb-4">
                        Equipos de refrigeración de última generación para el transporte de productos perecederos.
                    </p>
                    <button class="add-to-cart-btn mt-4 w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-300" data-product-id="prod3" data-product-name="Transporte Refrigerado">Agregar al Carrito</button>
                </div>
                <!-- Product Card 4 -->
                <div class="bg-white p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1">
                    <img src="https://www.marco.com.pe/wp-content/uploads/2021/09/spxbanners-Lub-Industrial_Marco-Peruana.jpg" onerror="this.onerror=null;this.src='https://placehold.co/300x200/B3E0FF/003366?text=Herramientas+Hidráulicas';" alt="" class="w-full h-48 object-cover rounded-md mb-6">
                    <h3 class="text-2xl font-semibold text-blue-700 mb-3">Herramientas Hidráulicas</h3>
                    <p class="text-gray-600 mb-4">
                        Amplia gama de herramientas hidráulicas de alto rendimiento para diversas aplicaciones industriales.
                    </p>
                    <button class="add-to-cart-btn mt-4 w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-300" data-product-id="prod4" data-product-name="Herramientas Hidráulicas">Agregar al Carrito</button>
                </div>
                <!-- Product Card 5 -->
                <div class="bg-white p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1">
                    <img src="https://cloudtec.pe/567-home_default/xhm6a.jpg" onerror="this.onerror=null;this.src='https://placehold.co/300x200/B3E0FF/003366?text=Bombas+y+Motores';" alt="" class="w-full h-48 object-cover rounded-md mb-6">
                    <h3 class="text-2xl font-semibold text-blue-700 mb-3">Bombas y Motores</h3>
                    <p class="text-gray-600 mb-4">
                        Bombas y motores hidráulicos de precisión para sistemas de alta exigencia.
                    </p>
                    <button class="add-to-cart-btn mt-4 w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-300" data-product-id="prod5" data-product-name="Bombas y Motores">Agregar al Carrito</button>
                </div>
                <!-- Product Card 6 -->
                <div class="bg-white p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1">
                    <img src="https://grupohidraulica.com/wp-content/uploads/2024/02/valvula-cierre-estacion-bombeo-agua-es-tuberia-tanques-agua-sala-industrial-suministro-agua-alta-pres-min-1024x683.jpg" onerror="this.onerror=null;this.src='https://placehold.co/300x200/B3E0FF/003366?text=Válvulas+y+Controles';" alt="" class="w-full h-48 object-cover rounded-md mb-6">
                    <h3 class="text-2xl font-semibold text-blue-700 mb-3">Válvulas y Controles</h3>
                    <p class="text-gray-600 mb-4">
                        Válvulas y sistemas de control para una gestión precisa de fluidos hidráulicos.
                    </p>
                    <button class="add-to-cart-btn mt-4 w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-300" data-product-id="prod6" data-product-name="Válvulas y Controles">Agregar al Carrito</button>
                </div>
            </div>
        </div>
    </section>

    <!-- Proyectos Especiales a Medida Section -->
    <section id="proyectos-especiales" class="py-16 md:py-24 bg-white rounded-lg shadow-md mx-6 md:mx-12 my-12">
        <div class="container mx-auto px-6 md:px-12 text-center">
            <h2 class="text-3xl md:text-4xl font-bold text-blue-800 mb-8">Proyectos Especiales a Medida</h2>

            <!-- Filter Section -->
            <div class="flex flex-col md:flex-row items-center justify-between mb-8">
                <p class="text-lg font-medium text-gray-700 mb-4 md:mb-0">Filtra aquí los productos por Marca</p>
                <div class="relative inline-block text-left">
                    <select id="brand-filter" class="block appearance-none w-full bg-white border border-gray-300 hover:border-gray-400 px-4 py-2 pr-8 rounded-lg shadow leading-tight focus:outline-none focus:shadow-outline text-gray-700">
                        <option value="all">Todas las marcas</option>
                        <option value="marco">MARCO</option>
                        <option value="recsol">RECSOL</option>
                        <option value="eaton">EATON</option>
                        <!-- Add more brands as needed -->
                    </select>
                    <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
                        <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 6.757 7.586 5.343 9z"/></svg>
                    </div>
                </div>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                <!-- Project Card 1: Chute Molino SAG -->
                <div class="bg-gray-50 p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1">
                    <img src="https://placehold.co/300x200/B3E0FF/003366?text=Chute+Molino+SAG" alt="" class="w-full h-48 object-cover rounded-md mb-6">
                    <h3 class="text-2xl font-semibold text-blue-700 mb-3">Chute Molino SAG</h3>
                    <p class="text-gray-600 mb-4">
                        Diseño y fabricación de chutes a medida para molinos SAG, optimizando el flujo de material.
                    </p>
                    <button class="add-to-cart-btn mt-4 w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-300" data-product-id="proj1" data-product-name="Chute Molino SAG">Agregar al Carrito</button>
                </div>
                <!-- Project Card 2: Equipo Enrrollador de Faja -->
                <div class="bg-gray-50 p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1">
                    <img src="https://placehold.co/300x200/B3E0FF/003366?text=Equipo+Enrrollador+de+Faja" alt="" class="w-full h-48 object-cover rounded-md mb-6">
                    <h3 class="text-2xl font-semibold text-blue-700 mb-3">Equipo Enrrollador de Faja</h3>
                    <p class="text-gray-600 mb-4">
                        Soluciones personalizadas para el enrollado eficiente de fajas transportadoras en minería.
                    </p>
                    <button class="add-to-cart-btn mt-4 w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-300" data-product-id="proj2" data-product-name="Equipo Enrrollador de Faja">Agregar al Carrito</button>
                </div>
                <!-- Project Card 3: Extractor de Contraeje -->
                <div class="bg-gray-50 p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1">
                    <img src="https://placehold.co/300x200/B3E0FF/003366?text=Extractor+de+Contraeje" alt="" class="w-full h-48 object-cover rounded-md mb-6">
                    <h3 class="text-2xl font-semibold text-blue-700 mb-3">Extractor de Contraeje</h3>
                    <p class="text-gray-600 mb-4">
                        Diseño y fabricación de extractores hidráulicos para contraejes, facilitando el mantenimiento.
                    </p>
                    <button class="add-to-cart-btn mt-4 w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-300" data-product-id="proj3" data-product-name="Extractor de Contraeje">Agregar al Carrito</button>
                </div>
                <!-- Project Card 4: Fabricación de UPH -->
                <div class="bg-gray-50 p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1">
                    <img src="https://placehold.co/300x200/B3E0FF/003366?text=Fabricación+de+UPH" alt="" class="w-full h-48 object-cover rounded-md mb-6">
                    <h3 class="text-2xl font-semibold text-blue-700 mb-3">Fabricación de UPH</h3>
                    <p class="text-gray-600 mb-4">
                        Unidades de Potencia Hidráulica (UPH) fabricadas a medida para diversas aplicaciones industriales.
                    </p>
                    <button class="add-to-cart-btn mt-4 w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-300" data-product-id="proj4" data-product-name="Fabricación de UPH">Agregar al Carrito</button>
                </div>
                <!-- Project Card 5: Inching Drive -->
                <div class="bg-gray-50 p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1">
                    <img src="https://placehold.co/300x200/B3E0FF/003366?text=Inching+Drive" alt="" class="w-full h-48 object-cover rounded-md mb-6">
                    <h3 class="text-2xl font-semibold text-blue-700 mb-3">Inching Drive</h3>
                    <p class="text-gray-600 mb-4">
                        Sistemas de inching drive para control de movimiento preciso en maquinaria pesada.
                    </p>
                    <button class="add-to-cart-btn mt-4 w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-300" data-product-id="proj5" data-product-name="Inching Drive">Agregar al Carrito</button>
                </div>
                <!-- Project Card 6: Manipulador de Corazas -->
                <div class="bg-gray-50 p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1">
                    <img src="https://placehold.co/300x200/B3E0FF/003366?text=Manipulador+de+Corazas" alt="" class="w-full h-48 object-cover rounded-md mb-6">
                    <h3 class="text-2xl font-semibold text-blue-700 mb-3">Manipulador de Corazas</h3>
                    <p class="text-gray-600 mb-4">
                        Equipos especializados para la manipulación segura y eficiente de corazas de molino.
                    </p>
                    <button class="add-to-cart-btn mt-4 w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-300" data-product-id="proj6" data-product-name="Manipulador de Corazas">Agregar al Carrito</button>
                </div>
                <!-- Project Card 7: Plataforma Hidráulica de Elevación -->
                <div class="bg-gray-50 p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1">
                    <img src="https://placehold.co/300x200/B3E0FF/003366?text=Plataforma+Hidráulica" alt="" class="w-full h-48 object-cover rounded-md mb-6">
                    <h3 class="text-2xl font-semibold text-blue-700 mb-3">Plataforma Hidráulica de Elevación</h3>
                    <p class="text-gray-600 mb-4">
                        Plataformas hidráulicas diseñadas a medida para necesidades específicas de elevación.
                    </p>
                    <button class="add-to-cart-btn mt-4 w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-300" data-product-id="proj7" data-product-name="Plataforma Hidráulica de Elevación">Agregar al Carrito</button>
                </div>
                <!-- Project Card 8: Válvulas de Doble Cuchilla -->
                <div class="bg-gray-50 p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1">
                    <img src="https://placehold.co/300x200/B3E0FF/003366?text=Válvulas+Doble+Cuchilla" alt="" class="w-full h-48 object-cover rounded-md mb-6">
                    <h3 class="text-2xl font-semibold text-blue-700 mb-3">Válvulas de Doble Cuchilla</h3>
                    <p class="text-gray-600 mb-4">
                        Fabricación de válvulas de doble cuchilla para control de flujo en aplicaciones industriales.
                    </p>
                    <button class="add-to-cart-btn mt-4 w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-300" data-product-id="proj8" data-product-name="Válvulas de Doble Cuchilla">Agregar al Carrito</button>
                </div>
                <!-- Project Card 9: Winche de Maniobra WM-2505 -->
                <div class="bg-gray-50 p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1">
                    <img src="https://placehold.co/300x200/B3E0FF/003366?text=Winche+Maniobra" alt="" class="w-full h-48 object-cover rounded-md mb-6">
                    <h3 class="text-2xl font-semibold text-blue-700 mb-3">Winche de Maniobra WM-2505</h3>
                    <p class="text-gray-600 mb-4">
                        Winches de maniobra de alta capacidad diseñados para operaciones de carga pesada.
                    </p>
                    <button class="add-to-cart-btn mt-4 w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-300" data-product-id="proj9" data-product-name="Winche de Maniobra WM-2505">Agregar al Carrito</button>
                </div>

                <!-- New Card for Project Idea Generator -->
                <div class="bg-gradient-to-br from-blue-600 to-blue-800 p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1 flex flex-col justify-center items-center text-white">
                    <h3 class="text-2xl font-semibold mb-4">¿Necesitas una Solución a Medida?</h3>
                    <p class="mb-6 text-center opacity-90">
                        Describe tu necesidad y nuestro asistente de IA te ayudará a generar una idea de proyecto.
                    </p>
                    <button id="open-project-idea-modal" class="bg-yellow-400 hover:bg-yellow-500 text-blue-900 font-bold py-3 px-6 rounded-full shadow-lg transition-all duration-300 transform hover:scale-105">
                        Generar Idea de Proyecto ✨
                    </button>
                </div>

            </div>
        </div>
    </section>

    <!-- Servicios Section -->
    <section id="servicios" class="py-16 md:py-24 bg-white rounded-lg shadow-md mx-6 md:mx-12 my-12">
        <div class="container mx-auto px-6 md:px-12 text-center">
            <h2 class="text-3xl md:text-4xl font-bold text-blue-800 mb-12">Nuestros Servicios</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                <!-- Service Card 1 -->
                <div class="bg-gray-50 p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1">
                    <img src="https://placehold.co/100x100/D1E9FF/003366?text=Servicio+Flushing" alt="" class="mx-auto mb-6 h-24 w-24 object-contain rounded-full">
                    <h3 class="text-2xl font-semibold text-blue-700 mb-3">Servicio de Flushing</h3>
                    <p class="text-gray-600 mb-4">
                        Limpieza profunda de tuberías y mangueras hidráulicas para prolongar la vida útil de sus equipos.
                    </p>
                </div>
                <!-- Service Card 2 -->
                <div class="bg-gray-50 p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1">
                    <img src="https://placehold.co/100x100/D1E9FF/003366?text=Montaje+Unidades" alt="" class="mx-auto mb-6 h-24 w-24 object-contain rounded-full">
                    <h3 class="text-2xl font-semibold text-blue-700 mb-3">Montaje de Unidades Hidráulicas</h3>
                    <p class="text-gray-600 mb-4">
                        Ensamblaje profesional de unidades hidráulicas, garantizando un funcionamiento óptimo.
                    </p>
                </div>
                <!-- Service Card 3 -->
                <div class="bg-gray-50 p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1">
                    <img src="https://placehold.co/100x100/D1E9FF/003366?text=Reparación+Componentes" alt="" class="mx-auto mb-6 h-24 w-24 object-contain rounded-full">
                    <h3 class="text-2xl font-semibold text-blue-700 mb-3">Reparación de Componentes</h3>
                    <p class="text-gray-600 mb-4">
                        Servicio técnico especializado para la reparación y mantenimiento de componentes hidráulicos.
                    </p>
                </div>
                <!-- New Card for Service Recommendation -->
                <div class="bg-gradient-to-br from-green-600 to-green-800 p-8 rounded-xl shadow-lg hover:shadow-xl transition-shadow duration-300 transform hover:-translate-y-1 flex flex-col justify-center items-center text-white">
                    <h3 class="text-2xl font-semibold mb-4">¿Necesitas Ayuda con un Problema?</h3>
                    <p class="mb-6 text-center opacity-90">
                        Describe tu problema y te recomendaremos el servicio adecuado.
                    </p>
                    <button id="open-service-recommendation-modal" class="bg-yellow-400 hover:bg-yellow-500 text-green-900 font-bold py-3 px-6 rounded-full shadow-lg transition-all duration-300 transform hover:scale-105">
                        Obtener Recomendación de Servicio ✨
                    </button>
                </div>
            </div>
        </div>
    </section>

    <!-- Brands/Partners Section -->
    <section class="py-16 md:py-24 bg-gray-100 rounded-lg shadow-md mx-6 md:mx-12 my-12">
        <div class="container mx-auto px-6 md:px-12 text-center">
            <h2 class="text-3xl md:text-4xl font-bold text-blue-800 mb-12">Nuestras Marcas Aliadas</h2>
            <div class="flex flex-wrap justify-center items-center gap-8 md:gap-16">
                <img src="https://placehold.co/150x80/FFFFFF/003366?text=RECSOL" alt="" class="h-20 opacity-80 hover:opacity-100 transition-opacity duration-300 rounded-md shadow-sm">
                <img src="https://placehold.co/150x80/FFFFFF/003366?text=AEROQUIP" alt="" class="h-20 opacity-80 hover:opacity-100 transition-opacity duration-300 rounded-md shadow-sm">
                <img src="https://placehold.co/150x80/FFFFFF/003366?text=EATON" alt="" class="h-20 opacity-80 hover:opacity-100 transition-opacity duration-300 rounded-md shadow-sm">
                <img src="https://placehold.co/150x80/FFFFFF/003366?text=POWER+TEAM" alt="" class="h-20 opacity-80 hover:opacity-100 transition-opacity duration-300 rounded-md shadow-sm">
                <img src="https://placehold.co/150x80/FFFFFF/003366?text=SUN+HYDRAULICS" alt="" class="h-20 opacity-80 hover:opacity-100 transition-opacity duration-300 rounded-md shadow-sm">
            </div>
        </div>
    </section>

    <!-- Contact Section -->
    <section id="contacto" class="py-16 md:py-24 bg-blue-800 text-white rounded-lg shadow-md mx-6 md:mx-12 my-12">
        <div class="container mx-auto px-6 md:px-12 text-center">
            <h2 class="text-3xl md:text-4xl font-bold mb-12">Contáctanos</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 gap-10 items-center">
                <div class="text-left md:text-center">
                    <p class="text-xl mb-4">¿Tienes alguna pregunta o necesitas una cotización?</p>
                    <p class="text-lg mb-2"><strong>Teléfono:</strong> +51 1 123 4567</p>
                    <p class="text-lg mb-2"><strong>Email:</strong> info@marco.com.pe</p>
                    <p class="text-lg mb-6"><strong>Dirección:</strong> Av. Principal 123, Lima, Perú</p>
                    <div class="flex justify-center md:justify-start space-x-4">
                        <a href="#" class="text-white hover:text-yellow-400 transition-colors duration-300">
                            <svg class="w-8 h-8" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true"><path d="M8.29 20.251c7.547 0 11.675-6.253 11.675-11.675 0-.178 0-.355-.007-.532A8.318 8.318 0 0022 5.92a8.19 8.19 0 01-2.357.646 4.11 4.11 0 001.804-2.27 8.222 8.222 0 01-2.605.996 4.107 4.107 0 00-6.993 3.743 11.65 11.65 0 01-8.457-4.287 4.106 4.106 0 001.27 5.477A4.073 4.073 0 01.8 10.70c-.045 1.265.34 2.486 1.026 3.474a4.073 4.073 0 001.27 5.477A4.073 4.073 0 01.8 10.70c-.045 1.265.34 2.486 1.026 3.474a4.073 4.073 0 01-1.854-5.043c.28-.08.567-.126.857-.126a4.107 4.107 0 003.834 2.85 8.233 8.233 0 01-2.078.21 8.27 8.27 0 01-1.5-.14 4.107 4.107 0 003.834 2.85z"></path></svg>
                        </a>
                        <a href="#" class="text-white hover:text-yellow-400 transition-colors duration-300">
                            <svg class="w-8 h-8" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true"><path fill-rule="evenodd" d="M22 12c0-5.523-4.477-10-10-10S2 6.477 2 12c0 4.991 3.657 9.128 8.438 9.878v-6.987h-2.54V12h2.54V9.797c0-2.506 1.492-3.89 3.777-3.89 1.094 0 2.238.195 2.238.195v2.46h-1.26c-1.243 0-1.63.771-1.63 1.562V12h2.773l-.443 2.89h-2.33V22H12c5.523 0 10-4.477 10-10z" clip-rule="evenodd"></path></svg>
                        </a>
                        <a href="#" class="text-white hover:text-yellow-400 transition-colors duration-300">
                            <svg class="w-8 h-8" fill="currentColor" viewBox="0 0 24 24" aria-hidden="true"><path fill-rule="evenodd" d="M12 2C6.477 2 2 6.477 2 12c0 4.418 2.86 8.16 6.84 9.513.5.092.682-.217.682-.483 0-.237-.008-.868-.013-1.7C6.73 18.37 6.13 17.23 6.13 17.23c-.43-.91-1.05-1.15-1.05-1.15-.69-.47.05-.46.05-.46.76.05 1.16.78 1.16.78.67 1.15 1.76.82 2.19.63.07-.49.26-.82.4-.99-1.68-.18-3.45-.84-3.45-3.73 0-.82.29-1.49.78-2.01-.08-.18-.34-.95.08-1.99 0 0 .64-.2 2.1.77.6-.17 1.23-.26 1.88-.26.65 0 1.28.09 1.88.26 1.46-.97 2.1-.77 2.1-.77.42 1.04.16 1.81.08 1.99.49.52.78 1.2.78 2.01 0 2.89-1.77 3.54-3.45 3.73.26.22.5.66.5 1.33 0 .96-.008 1.73-.008 1.96 0 .26.18.58.69.48C19.14 20.16 22 16.42 22 12c0-5.523-4.477-10-10-10z" clip-rule="evenodd"></path></svg>
                        </a>
                    </div>
                </div>
                <div class="bg-blue-700 p-8 rounded-xl shadow-lg">
                    <form class="space-y-6">
                        <div>
                            <label for="name" class="block text-lg font-medium mb-2">Nombre Completo</label>
                            <input type="text" id="name" name="name" class="w-full p-3 rounded-md bg-blue-600 border border-blue-500 placeholder-blue-200 text-white focus:ring-yellow-500 focus:border-yellow-500" placeholder="Tu nombre">
                        </div>
                        <div>
                            <label for="email" class="block text-lg font-medium mb-2">Correo Electrónico</label>
                            <input type="email" id="email" name="email" class="w-full p-3 rounded-md bg-blue-600 border border-blue-500 placeholder-blue-200 text-white focus:ring-yellow-500 focus:border-yellow-500" placeholder="tu.correo@ejemplo.com">
                        </div>
                        <div>
                            <label for="message" class="block text-lg font-medium mb-2">Mensaje</label>
                            <textarea id="message" name="message" rows="5" class="w-full p-3 rounded-md bg-blue-600 border border-blue-500 placeholder-blue-200 text-white focus:ring-yellow-500 focus:border-yellow-500" placeholder="Escribe tu mensaje aquí..."></textarea>
                        </div>
                        <button type="submit" class="w-full bg-yellow-500 hover:bg-yellow-600 text-blue-900 font-bold py-3 px-6 rounded-lg shadow-md transition-all duration-300 transform hover:scale-105">
                            Enviar Mensaje
                        </button>
                    </form>
                </div>
            </div>
        </div>
    </section>

    <!-- Footer Section -->
    <footer class="bg-gray-900 text-gray-300 py-8 px-6 md:px-12 rounded-t-lg shadow-inner">
        <div class="container mx-auto text-center text-sm">
            <p>&copy; 2024 Marco Peruana. Todos los derechos reservados.</p>
            <div class="flex justify-center space-x-6 mt-4">
                <a href="#" class="hover:text-white transition-colors duration-300">Política de Privacidad</a>
                <a href="#" class="hover:text-white transition-colors duration-300">Términos de Servicio</a>
                <a href="#" class="hover:text-white transition-colors duration-300">Mapa del Sitio</a>
            </div>
        </div>
    </footer>

    <!-- Chatbot Container -->
    <div id="chatbot-container">
        <!-- Floating Action Button -->
        <div id="chatbot-fab" class="mb-4">
            ✨
        </div>

        <!-- Chat Window -->
        <div id="chat-window">
            <div id="chat-header">
                <span>Asistente de Marco Peruana</span>
                <button id="close-chat" class="text-white text-xl leading-none">&times;</button>
            </div>
            <div id="chat-messages">
                <!-- Messages will be appended here -->
                <div class="message bot">
                    <div class="message-bubble">
                        ¡Hola! Soy tu asistente de Marco Peruana. ¿En qué puedo ayudarte hoy?
                    </div>
                </div>
            </div>
            <div id="chat-input-container">
                <input type="text" id="chat-input" placeholder="Escribe tu mensaje...">
                <button id="chat-send-button">Enviar</button>
            </div>
        </div>
    </div>

    <!-- Cart Modal -->
    <div id="cart-modal" class="hidden fixed inset-0 bg-gray-800 bg-opacity-75 flex items-center justify-center z-[1001]">
        <div id="cart-modal-content" class="bg-white p-6 rounded-lg shadow-xl w-11/12 md:w-1/2 lg:w-1/3 max-h-[90vh] flex flex-col">
            <div class="flex justify-between items-center mb-4">
                <h2 class="text-2xl font-bold text-blue-800">Tu Carrito de Compras</h2>
                <button id="close-cart-modal" class="text-gray-500 hover:text-gray-700 text-3xl leading-none">&times;</button>
            </div>
            <div id="cart-items-list" class="flex-grow overflow-y-auto border-t border-b border-gray-200 py-4 mb-4">
                <!-- Cart items will be rendered here -->
            </div>
            <div class="flex justify-between items-center">
                <button id="clear-cart-btn" class="bg-red-500 hover:bg-red-600 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-300">Vaciar Carrito</button>
                <button class="bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-300">Proceder al Pago</button>
            </div>
        </div>
    </div>

    <!-- Project Idea Generator Modal -->
    <div id="project-idea-modal" class="hidden fixed inset-0 bg-gray-800 bg-opacity-75 flex items-center justify-center z-[1002]">
        <div id="project-idea-modal-content" class="bg-white p-6 rounded-lg shadow-xl w-11/12 md:w-2/3 lg:w-1/2 max-h-[90vh] flex flex-col">
            <div class="flex justify-between items-center mb-4">
                <h2 class="text-2xl font-bold text-blue-800">Generar Idea de Proyecto ✨</h2>
                <button id="close-project-idea-modal" class="text-gray-500 hover:text-gray-700 text-3xl leading-none">&times;</button>
            </div>
            <div class="flex-grow overflow-y-auto py-4 mb-4">
                <textarea id="project-idea-input" class="w-full p-3 rounded-md border border-gray-300 focus:ring-blue-500 focus:border-blue-500 mb-4 h-32 resize-y" placeholder="Describe tu necesidad o idea de proyecto (ej: 'Necesito una solución para automatizar el transporte de minerales en una mina subterránea')."></textarea>
                <button id="generate-project-btn" class="w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-300">
                    Generar Idea
                </button>
                <div id="project-idea-output" class="mt-6 p-4 bg-gray-100 rounded-md text-left text-gray-700 hidden">
                    <p class="font-semibold mb-2">Idea de Proyecto Generada:</p>
                    <div id="generated-text"></div>
                    <div class="loading-dots hidden mt-4">
                        <span></span><span></span><span></span>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Service Recommendation Modal -->
    <div id="service-recommendation-modal" class="hidden fixed inset-0 bg-gray-800 bg-opacity-75 flex items-center justify-center z-[1003]">
        <div id="service-recommendation-modal-content" class="bg-white p-6 rounded-lg shadow-xl w-11/12 md:w-2/3 lg:w-1/2 max-h-[90vh] flex flex-col">
            <div class="flex justify-between items-center mb-4">
                <h2 class="text-2xl font-bold text-blue-800">Recomendador de Servicios ✨</h2>
                <button id="close-service-recommendation-modal" class="text-gray-500 hover:text-gray-700 text-3xl leading-none">&times;</button>
            </div>
            <div class="flex-grow overflow-y-auto py-4 mb-4">
                <textarea id="service-problem-input" class="w-full p-3 rounded-md border border-gray-300 focus:ring-blue-500 focus:border-blue-500 mb-4 h-32 resize-y" placeholder="Describe el problema o la necesidad que tienes (ej: 'Mis equipos hidráulicos tienen fugas frecuentes y necesito mantenimiento preventivo')."></textarea>
                <button id="get-service-recommendation-btn" class="w-full bg-green-600 hover:bg-green-700 text-white font-semibold py-2 px-4 rounded-lg transition-colors duration-300">
                    Obtener Recomendación
                </button>
                <div id="service-recommendation-output" class="mt-6 p-4 bg-gray-100 rounded-md text-left text-gray-700 hidden">
                    <p class="font-semibold mb-2">Servicio Recomendado:</p>
                    <div id="recommended-service-text"></div>
                    <div class="loading-dots hidden mt-4">
                        <span></span><span></span><span></span>
                    </div>
                </div>
            </div>
        </div>
    </div>


    <script>
        const chatFab = document.getElementById('chatbot-fab');
        const chatWindow = document.getElementById('chat-window');
        const closeChatButton = document.getElementById('close-chat');
        const chatMessages = document.getElementById('chat-messages');
        const chatInput = document.getElementById('chat-input');
        const chatSendButton = document.getElementById('chat-send-button');
        const cartCountSpan = document.getElementById('cart-count');
        const addToCartButtons = document.querySelectorAll('.add-to-cart-btn');
        const openCartModalBtn = document.getElementById('open-cart-modal');
        const cartModal = document.getElementById('cart-modal');
        const closeCartModalBtn = document.getElementById('close-cart-modal');
        const cartItemsList = document.getElementById('cart-items-list');
        const clearCartBtn = document.getElementById('clear-cart-btn');

        const openProjectIdeaModalBtn = document.getElementById('open-project-idea-modal');
        const projectIdeaModal = document.getElementById('project-idea-modal');
        const closeProjectIdeaModalBtn = document.getElementById('close-project-idea-modal');
        const projectIdeaInput = document.getElementById('project-idea-input');
        const generateProjectBtn = document.getElementById('generate-project-btn');
        const projectIdeaOutput = document.getElementById('project-idea-output');
        const generatedTextDiv = document.getElementById('generated-text');
        const projectLoadingDots = projectIdeaOutput.querySelector('.loading-dots');

        const openServiceRecommendationModalBtn = document.getElementById('open-service-recommendation-modal');
        const serviceRecommendationModal = document.getElementById('service-recommendation-modal');
        const closeServiceRecommendationModalBtn = document = document.getElementById('close-service-recommendation-modal');
        const serviceProblemInput = document.getElementById('service-problem-input');
        const getServiceRecommendationBtn = document.getElementById('get-service-recommendation-btn');
        const serviceRecommendationOutput = document.getElementById('service-recommendation-output');
        const recommendedServiceTextDiv = document.getElementById('recommended-service-text');
        const serviceLoadingDots = serviceRecommendationOutput.querySelector('.loading-dots');


        let cart = []; // Array to store products in the cart, now with quantity: [{id: 'prod1', name: 'Product A', quantity: 1}]

        // Function to update cart count display and render cart items in modal
        function updateCartDisplay() {
            const totalItems = cart.reduce((sum, item) => sum + item.quantity, 0);
            cartCountSpan.textContent = totalItems;
            renderCartItems();
        }

        // Function to render items in the cart modal
        function renderCartItems() {
            cartItemsList.innerHTML = ''; // Clear existing items

            if (cart.length === 0) {
                const emptyMessage = document.createElement('p');
                emptyMessage.classList.add('text-gray-500', 'text-center', 'py-4');
                emptyMessage.textContent = 'Tu carrito está vacío.';
                cartItemsList.appendChild(emptyMessage);
            } else {
                cart.forEach((item, index) => {
                    const itemDiv = document.createElement('div');
                    itemDiv.classList.add('flex', 'justify-between', 'items-center', 'py-2', 'border-b', 'border-gray-100', 'last:border-b-0');
                    itemDiv.innerHTML = `
                        <span class="text-gray-700 font-medium">${item.name}</span>
                        <div class="flex items-center space-x-2">
                            <button class="decrease-quantity-btn text-blue-500 hover:text-blue-700 font-bold text-lg px-2 rounded-md" data-product-id="${item.id}">-</button>
                            <span class="text-gray-800 font-semibold">${item.quantity}</span>
                            <button class="increase-quantity-btn text-blue-500 hover:text-blue-700 font-bold text-lg px-2 rounded-md" data-product-id="${item.id}">+</button>
                            <button class="remove-from-cart-btn text-red-500 hover:text-red-700 text-lg ml-2" data-product-id="${item.id}">&times;</button>
                        </div>
                    `;
                    cartItemsList.appendChild(itemDiv);
                });

                // Add event listeners for quantity change and remove buttons
                document.querySelectorAll('.increase-quantity-btn').forEach(button => {
                    button.addEventListener('click', (event) => {
                        const productId = event.target.dataset.productId;
                        updateQuantity(productId, 1); // Increase quantity by 1
                    });
                });

                document.querySelectorAll('.decrease-quantity-btn').forEach(button => {
                    button.addEventListener('click', (event) => {
                        const productId = event.target.dataset.productId;
                        updateQuantity(productId, -1); // Decrease quantity by 1
                    });
                });

                document.querySelectorAll('.remove-from-cart-btn').forEach(button => {
                    button.addEventListener('click', (event) => {
                        const productId = event.target.dataset.productId;
                        removeCartItem(productId);
                    });
                });
            }
        }

        // Function to add or increment product in cart
        function addToCart(productId, productName) {
            const existingItemIndex = cart.findIndex(item => item.id === productId);

            if (existingItemIndex > -1) {
                cart[existingItemIndex].quantity += 1;
            } else {
                cart.push({ id: productId, name: productName, quantity: 1 });
            }
            updateCartDisplay();
            console.log('Product added to cart:', { id: productId, name: productName });
            // Automatically open the cart modal after adding a product
            cartModal.classList.remove('hidden');
            renderCartItems();
        }

        // Function to remove product from cart
        function removeCartItem(productId) {
            cart = cart.filter(item => item.id !== productId);
            updateCartDisplay();
            console.log('Product removed from cart:', productId);
        }

        // Function to update quantity of a product in cart
        function updateQuantity(productId, change) {
            const itemIndex = cart.findIndex(item => item.id === productId);
            if (itemIndex > -1) {
                cart[itemIndex].quantity += change;
                if (cart[itemIndex].quantity <= 0) {
                    removeCartItem(productId); // Remove if quantity drops to 0 or less
                } else {
                    updateCartDisplay();
                }
            }
        }

        // Add event listeners to "Add to Cart" buttons
        addToCartButtons.forEach(button => {
            button.addEventListener('click', (event) => {
                const productId = event.target.dataset.productId;
                const productName = event.target.dataset.productName;
                addToCart(productId, productName);
            });
        });

        // Open cart modal
        openCartModalBtn.addEventListener('click', () => {
            cartModal.classList.remove('hidden');
            renderCartItems(); // Ensure cart items are rendered when opening
        });

        // Close cart modal
        closeCartModalBtn.addEventListener('click', () => {
            cartModal.classList.add('hidden');
        });

        // Close modal when clicking outside content
        cartModal.addEventListener('click', (event) => {
            if (event.target === cartModal) {
                cartModal.classList.add('hidden');
            }
        });

        // Clear cart
        clearCartBtn.addEventListener('click', () => {
            cart = []; // Empty the cart array
            updateCartDisplay(); // Update display
        });


        // Chatbot functionality (existing code)

        // Toggle chat window visibility
        chatFab.addEventListener('click', () => {
            chatWindow.classList.toggle('open');
        });

        closeChatButton.addEventListener('click', () => {
            chatWindow.classList.remove('open');
        });

        // Function to add a message to the chat
        function addMessage(text, sender) {
            const messageDiv = document.createElement('div');
            messageDiv.classList.add('message', sender);
            const bubbleDiv = document.createElement('div');
            bubbleDiv.classList.add('message-bubble');
            bubbleDiv.textContent = text;
            messageDiv.appendChild(bubbleDiv);
            chatMessages.appendChild(messageDiv);
            chatMessages.scrollTop = chatMessages.scrollHeight; // Scroll to bottom
        }

        // Function to show loading dots for chatbot
        function showLoading() {
            const loadingDiv = document.createElement('div');
            loadingDiv.classList.add('message', 'bot', 'loading-indicator');
            loadingDiv.innerHTML = `
                <div class="message-bubble">
                    <div class="loading-dots">
                        <span></span><span></span><span></span>
                    </div>
                </div>
            `;
            chatMessages.appendChild(loadingDiv);
            chatMessages.scrollTop = chatMessages.scrollHeight;
        }

        // Function to remove loading dots for chatbot
        function hideLoading() {
            const loadingIndicator = document.querySelector('.loading-indicator');
            if (loadingIndicator) {
                loadingIndicator.remove();
            }
        }

        // Handle sending messages for chatbot
        chatSendButton.addEventListener('click', sendMessage);
        chatInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                sendMessage();
            }
        });

        async function sendMessage() {
            const userMessage = chatInput.value.trim();
            if (userMessage === '') return;

            addMessage(userMessage, 'user');
            chatInput.value = '';
            showLoading(); // Show loading indicator

            // Simulate a knowledge base for Marco Peruana's products and services
            const knowledgeBase = `
                Marco Peruana es una empresa líder en soluciones integrales para la industria, especialmente en los sectores de pesca y minería.
                Ofrecemos productos como:
                - Oleohidráulica Minera: Soluciones hidráulicas robustas para minería.
                - Oleohidráulica Móvil: Componentes y sistemas hidráulicos para maquinaria móvil.
                - Equipos para Transporte Refrigerado: Equipos de refrigeración de última generación.
                - Herramientas Hidráulicas: Amplia gama de herramientas de alto rendimiento.
                - Bombas y Motores: Bombas y motores hidráulicos de precisión.
                - Válvulas y Controles: Válvulas y sistemas de control para fluidos hidráulicos.
                - Proyectos Especiales a Medida: Chute Molino SAG, Equipo Enrrollador de Faja, Extractor de Contraeje, Fabricación de UPH, Inching Drive, Manipulador de Corazas, Plataforma Hidráulica de Elevación, Válvulas de Doble Cuchilla, Winche de Maniobra WM-2505.

                Nuestros servicios incluyen:
                - Servicio de Flushing de tuberías y mangueras hidráulicas: Limpieza profunda para prolongar la vida útil.
                - Servicio de Montaje de Unidades Hidráulicas: Ensamblaje profesional.
                - Reparación y mantenimiento de componentes hidráulicos: Servicio técnico especializado.

                Nuestras marcas aliadas son Recsol, Aeroquip, Eaton, Power Team y Sun Hydraulics.
                Estamos en Av. Principal 123, Lima, Perú. Teléfono: +51 1 123 4567. Email: info@marco.com.pe.
                Operamos desde 1965.
                Además, ahora puedes agregar productos a tu carrito de compras, verlos en el modal del carrito, eliminar productos y editar sus cantidades. También puedes generar ideas de proyectos especiales a medida utilizando nuestro asistente de IA, y obtener recomendaciones de servicios.
            `;

            const prompt = `Basándote en la siguiente información sobre Marco Peruana, responde a la pregunta del usuario.
            Si la pregunta no está relacionada con los productos o servicios de Marco Peruana, o si la información no es suficiente para responder, indica amablemente que no puedes ayudar con eso.

            Información de Marco Peruana:
            ${knowledgeBase}

            Pregunta del usuario: "${userMessage}"

            Respuesta:`;

            try {
                let chatHistory = [];
                chatHistory.push({ role: "user", parts: [{ text: prompt }] });
                const payload = { contents: chatHistory };
                const apiKey = ""; // Canvas will provide this key
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                const result = await response.json();

                hideLoading(); // Hide loading indicator

                if (result.candidates && result.candidates.length > 0 &&
                    result.candidates[0].content && result.candidates[0].content.parts &&
                    result.candidates[0].content.parts.length > 0) {
                    const botResponse = result.candidates[0].content.parts[0].text;
                    addMessage(botResponse, 'bot');
                } else {
                    addMessage('Lo siento, no pude obtener una respuesta en este momento. Por favor, inténtalo de nuevo más tarde.', 'bot');
                    console.error('Unexpected API response structure:', result);
                }
            } catch (error) {
                hideLoading(); // Hide loading indicator
                addMessage('Hubo un error al comunicarse con el asistente. Por favor, inténtalo de nuevo.', 'bot');
                console.error('Error calling Gemini API:', error);
            }
        }

        // Project Idea Generator Logic
        openProjectIdeaModalBtn.addEventListener('click', () => {
            projectIdeaModal.classList.remove('hidden');
            projectIdeaInput.value = ''; // Clear previous input
            generatedTextDiv.innerHTML = ''; // Clear previous output
            projectIdeaOutput.classList.add('hidden'); // Hide output area initially
        });

        closeProjectIdeaModalBtn.addEventListener('click', () => {
            projectIdeaModal.classList.add('hidden');
        });

        projectIdeaModal.addEventListener('click', (event) => {
            if (event.target === projectIdeaModal) {
                projectIdeaModal.classList.add('hidden');
            }
        });

        generateProjectBtn.addEventListener('click', generateProjectIdea);

        async function generateProjectIdea() {
            const userIdea = projectIdeaInput.value.trim();
            if (userIdea === '') {
                generatedTextDiv.textContent = 'Por favor, describe tu necesidad o idea de proyecto para generar una propuesta.';
                projectIdeaOutput.classList.remove('hidden');
                return;
            }

            // Show loading dots
            projectIdeaOutput.classList.remove('hidden');
            generatedTextDiv.innerHTML = ''; // Clear previous text
            projectLoadingDots.classList.remove('hidden');

            const projectKnowledgeBase = `
                Marco Peruana es especialista en soluciones industriales, incluyendo:
                - Oleohidráulica Minera y Móvil (bombas, motores, cilindros, válvulas, unidades de potencia, mangueras).
                - Equipos para Transporte Refrigerado.
                - Herramientas Hidráulicas (gatos, bombas, prensas).
                - Fabricación y reparación de componentes.
                - Proyectos Especiales a Medida (chutes, enrolladores de faja, extractores, UPH, inching drives, manipuladores de corazas, plataformas hidráulicas, válvulas de doble cuchilla, winches de maniobra).
                Podemos diseñar, fabricar, integrar y dar mantenimiento a soluciones complejas.
            `;

            const projectPrompt = `Eres un consultor de proyectos para Marco Peruana. Basándote en la siguiente descripción de las capacidades de Marco Peruana y la necesidad del cliente, genera una idea de proyecto detallada. Incluye el posible tipo de solución, beneficios clave y un breve resumen de cómo Marco Peruana podría abordarlo. Sé conciso y profesional.

            Capacidades de Marco Peruana:
            ${projectKnowledgeBase}

            Necesidad del cliente: "${userIdea}"

            Idea de Proyecto:`;

            try {
                let chatHistory = [];
                chatHistory.push({ role: "user", parts: [{ text: projectPrompt }] });
                const payload = { contents: chatHistory };
                const apiKey = ""; // Canvas will provide this key
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                const result = await response.json();

                // Hide loading dots
                projectLoadingDots.classList.add('hidden');

                if (result.candidates && result.candidates.length > 0 &&
                    result.candidates[0].content && result.candidates[0].content.parts &&
                    result.candidates[0].content.parts.length > 0) {
                    const generatedProject = result.candidates[0].content.parts[0].text;
                    generatedTextDiv.textContent = generatedProject;
                } else {
                    generatedTextDiv.textContent = 'Lo siento, no pude generar una idea de proyecto en este momento. Por favor, inténtalo de nuevo con una descripción diferente.';
                    console.error('Unexpected API response structure for project idea:', result);
                }
            } catch (error) {
                // Hide loading dots
                projectLoadingDots.classList.add('hidden');
                generatedTextDiv.textContent = 'Hubo un error al generar la idea de proyecto. Por favor, inténtalo de nuevo.';
                console.error('Error calling Gemini API for project idea:', error);
            }
        }

        // Service Recommendation Logic
        openServiceRecommendationModalBtn.addEventListener('click', () => {
            serviceRecommendationModal.classList.remove('hidden');
            serviceProblemInput.value = ''; // Clear previous input
            recommendedServiceTextDiv.innerHTML = ''; // Clear previous output
            serviceRecommendationOutput.classList.add('hidden'); // Hide output area initially
        });

        closeServiceRecommendationModalBtn.addEventListener('click', () => {
            serviceRecommendationModal.classList.add('hidden');
        });

        serviceRecommendationModal.addEventListener('click', (event) => {
            if (event.target === serviceRecommendationModal) {
                serviceRecommendationModal.classList.add('hidden');
            }
        });

        getServiceRecommendationBtn.addEventListener('click', getServiceRecommendation);

        async function getServiceRecommendation() {
            const userProblem = serviceProblemInput.value.trim();
            if (userProblem === '') {
                recommendedServiceTextDiv.textContent = 'Por favor, describe tu problema para obtener una recomendación de servicio.';
                serviceRecommendationOutput.classList.remove('hidden');
                return;
            }

            // Show loading dots
            serviceRecommendationOutput.classList.remove('hidden');
            recommendedServiceTextDiv.innerHTML = ''; // Clear previous text
            serviceLoadingDots.classList.remove('hidden');

            const serviceKnowledgeBase = `
                Marco Peruana ofrece los siguientes servicios:
                - Servicio de Flushing: Limpieza profunda de tuberías y mangueras hidráulicas para prolongar la vida útil de sus equipos, eliminando contaminantes y residuos.
                - Montaje de Unidades Hidráulicas: Ensamblaje profesional de unidades hidráulicas, asegurando su correcto funcionamiento y optimización.
                - Reparación de Componentes: Servicio técnico especializado para la reparación y mantenimiento de componentes hidráulicos, incluyendo bombas, motores, cilindros y válvulas.
                - Además, ofrecemos proyectos especiales a medida para soluciones complejas.
            `;

            const servicePrompt = `Eres un experto en servicios industriales de Marco Peruana. Basándote en la siguiente descripción de los servicios de Marco Peruana y el problema del cliente, recomienda el servicio o producto más adecuado. Explica brevemente por qué es la mejor opción. Sé conciso y profesional.

            Servicios de Marco Peruana:
            ${serviceKnowledgeBase}

            Problema del cliente: "${userProblem}"

            Recomendación de Servicio:`;

            try {
                let chatHistory = [];
                chatHistory.push({ role: "user", parts: [{ text: servicePrompt }] });
                const payload = { contents: chatHistory };
                const apiKey = ""; // Canvas will provide this key
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });

                const result = await response.json();

                // Hide loading dots
                serviceLoadingDots.classList.add('hidden');

                if (result.candidates && result.candidates.length > 0 &&
                    result.candidates[0].content && result.candidates[0].content.parts &&
                    result.candidates[0].content.parts.length > 0) {
                    const recommendedService = result.candidates[0].content.parts[0].text;
                    recommendedServiceTextDiv.textContent = recommendedService;
                } else {
                    recommendedServiceTextDiv.textContent = 'Lo siento, no pude generar una recomendación en este momento. Por favor, inténtalo de nuevo con una descripción diferente.';
                    console.error('Unexpected API response structure for service recommendation:', result);
                }
            } catch (error) {
                // Hide loading dots
                serviceLoadingDots.classList.add('hidden');
                recommendedServiceTextDiv.textContent = 'Hubo un error al obtener la recomendación de servicio. Por favor, inténtalo de nuevo.';
                console.error('Error calling Gemini API for service recommendation:', error);
            }
        }
    </script>

</body>
</html>
