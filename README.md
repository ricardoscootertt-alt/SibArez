<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Comercial SibArez - Pedido Online</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;800&display=swap');
        
        body {
            font-family: 'Poppins', sans-serif;
            background-color: #1e3a8a; /* Azul Principal */
            color: #ffffff;
        }

        .sibarez-red { color: #ef4444; }
        .bg-card { background-color: #ffffff; color: #1e3a8a; }
        
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #1e3a8a; }
        ::-webkit-scrollbar-thumb { background: #ef4444; border-radius: 10px; }

        .cart-modal {
            transform: translateX(100%);
            transition: transform 0.3s ease-in-out;
        }
        .cart-modal.active {
            transform: translateX(0);
        }
    </style>
</head>
<body class="min-h-screen pb-20">

    <!-- Header -->
    <nav class="bg-blue-950 p-4 sticky top-0 z-40 shadow-2xl border-b-2 border-red-600">
        <div class="container mx-auto flex justify-between items-center">
            <h1 class="text-2xl md:text-3xl font-black italic tracking-tighter">
                COMERCIAL <span class="sibarez-red">SIBAREZ</span>
            </h1>
            <button onclick="toggleCart()" class="relative bg-red-600 p-2 rounded-full hover:bg-red-700 transition">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z" />
                </svg>
                <span id="cart-count" class="absolute -top-2 -right-2 bg-white text-red-600 text-xs font-bold px-2 py-1 rounded-full border-2 border-red-600">0</span>
            </button>
        </div>
    </nav>

    <!-- Banner -->
    <header class="py-8 text-center px-4">
        <h2 class="text-3xl md:text-4xl font-extrabold mb-2 uppercase tracking-wide">Faça seu Pedido Online</h2>
        <p class="text-blue-200">Preços baixos e entrega rápida na sua casa!</p>
    </header>

    <!-- Produtos Grid -->
    <main id="product-grid" class="container mx-auto px-4 grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4 md:gap-6">
        <!-- Produtos injetados via JS -->
    </main>

    <!-- Carrinho Lateral -->
    <div id="cart-overlay" onclick="toggleCart()" class="fixed inset-0 bg-black/60 z-40 hidden"></div>
    <div id="cart-sidebar" class="cart-modal fixed top-0 right-0 h-full w-full max-w-md bg-white z-50 shadow-2xl p-6 overflow-y-auto text-blue-900">
        <div class="flex justify-between items-center mb-6">
            <h3 class="text-2xl font-bold border-b-4 border-red-600">Seu Carrinho</h3>
            <button onclick="toggleCart()" class="text-gray-400 hover:text-red-600 text-xl font-bold">FECHAR</button>
        </div>

        <div id="cart-items" class="space-y-4 mb-6">
            <p class="text-gray-400 italic text-center py-10">Carrinho vazio...</p>
        </div>

        <div id="cart-summary" class="border-t pt-4 hidden">
            <div class="flex justify-between text-xl font-bold mb-6">
                <span>Subtotal:</span>
                <span id="cart-total" class="text-red-600">R$ 0,00</span>
            </div>

            <hr class="mb-6">

            <h4 class="font-bold mb-3 text-blue-800 uppercase text-sm">📍 Endereço de Entrega</h4>
            <div class="space-y-3 mb-6">
                <input type="text" id="cust-name" placeholder="Seu Nome" class="w-full p-3 border rounded bg-gray-50 outline-none focus:border-blue-500">
                <input type="text" id="cust-address" placeholder="Rua, Número e Bairro" class="w-full p-3 border rounded bg-gray-50 outline-none focus:border-blue-500">
            </div>

            <h4 class="font-bold mb-3 text-blue-800 uppercase text-sm">💳 Forma de Pagamento</h4>
            <div class="grid grid-cols-1 gap-2 mb-6">
                <label class="flex items-center p-3 border rounded cursor-pointer hover:bg-gray-50 transition">
                    <input type="radio" name="payment" value="PIX" checked onclick="togglePaymentFields()" class="mr-3">
                    <span>PIX</span>
                </label>
                <label class="flex items-center p-3 border rounded cursor-pointer hover:bg-gray-50 transition">
                    <input type="radio" name="payment" value="Cartão" onclick="togglePaymentFields()" class="mr-3">
                    <div class="flex flex-col">
                        <span>Cartão (Débito/Crédito)</span>
                        <span class="text-[10px] text-red-500 font-bold uppercase">* Sem parcelamento</span>
                    </div>
                </label>
                <label class="flex items-center p-3 border rounded cursor-pointer hover:bg-gray-50 transition">
                    <input type="radio" name="payment" value="Dinheiro" onclick="togglePaymentFields()" class="mr-3">
                    <span>Dinheiro em Espécie</span>
                </label>
            </div>

            <!-- Campo de Troco (Oculto por padrão) -->
            <div id="change-field" class="hidden mb-6 p-4 bg-yellow-50 border-l-4 border-yellow-400">
                <p class="text-sm font-bold mb-2 text-yellow-800">Precisa de troco para quanto?</p>
                <div class="flex gap-2 items-center">
                    <span class="font-bold">R$</span>
                    <input type="number" id="change-amount" placeholder="Ex: 50,00" class="w-full p-2 border rounded outline-none focus:border-yellow-600">
                </div>
                <p id="change-info" class="text-xs mt-2 text-yellow-700 font-semibold"></p>
            </div>

            <textarea id="cust-obs" placeholder="Alguma observação adicional?" class="w-full p-3 border rounded bg-gray-50 outline-none focus:border-blue-500 mb-6 h-20"></textarea>

            <button onclick="checkout()" class="w-full bg-green-600 text-white font-black py-4 rounded-xl hover:bg-green-700 transition shadow-lg flex items-center justify-center gap-2">
                <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24"><path d="M.057 24l1.687-6.163c-1.041-1.804-1.588-3.849-1.587-5.946.003-6.556 5.338-11.891 11.893-11.891 3.181.001 6.167 1.24 8.413 3.488 2.246 2.248 3.484 5.232 3.483 8.413-.003 6.557-5.338 11.892-11.893 11.892-1.997-.001-3.951-.5-5.688-1.448l-6.308 1.654zm6.757-4.051c1.535.912 3.101 1.391 4.709 1.391 5.44 0 9.863-4.424 9.866-9.864.001-2.636-1.026-5.115-2.893-6.983-1.867-1.868-4.346-2.895-6.981-2.896-5.442 0-9.866 4.423-9.869 9.863-.001 1.77.461 3.498 1.336 5.054l-1.012 3.702 3.844-.997zm11.366-7.327c-.314-.157-1.855-.916-2.144-1.02-.289-.104-.499-.157-.709.157s-.814 1.02-1.024 1.256-.42.262-.734.105c-.314-.157-1.328-.489-2.53-1.561-.933-.831-1.563-1.858-1.747-2.172s-.02-.485.137-.641c.141-.141.314-.367.471-.55.157-.183.209-.314.314-.524s.052-.393-.026-.55c-.079-.157-.709-1.711-.971-2.339-.255-.611-.515-.529-.709-.539s-.394-.01-.603-.01c-.209 0-.551.079-.84.393s-1.099 1.074-1.099 2.618 1.125 3.037 1.282 3.246c.157.209 2.214 3.382 5.362 4.742.748.324 1.333.517 1.789.66.752.239 1.436.205 1.977.125.603-.09 1.855-.759 2.118-1.492.262-.733.262-1.362.183-1.492s-.289-.209-.603-.366z"/></svg>
                PEDIR PELO WHATSAPP
            </button>
        </div>
    </div>

    <!-- Mensagem Flutuante -->
    <div id="toast" class="fixed bottom-10 left-1/2 -translate-x-1/2 bg-red-600 px-6 py-3 rounded-full font-bold shadow-lg transition-all opacity-0 pointer-events-none z-50">
        🚀 Adicionado ao carrinho!
    </div>

    <script>
        const products = [
            { id: 1, name: "Leite Integral 1L", price: 4.49, emoji: "🥛" },
            { id: 2, name: "Maçã Gala (Kg)", price: 6.99, emoji: "🍎" },
            { id: 3, name: "Pão Francês (Kg)", price: 12.90, emoji: "🥖" },
            { id: 4, name: "Frango Inteiro", price: 18.90, emoji: "🍗" },
            { id: 5, name: "Arroz Tio João 1kg", price: 5.80, emoji: "🌾" },
            { id: 6, name: "Feijão Carioca 1kg", price: 7.50, emoji: "🥘" },
            { id: 7, name: "Café Melitta 250g", price: 11.20, emoji: "☕" },
            { id: 8, name: "Óleo de Soja 900ml", price: 6.45, emoji: "🧴" }
        ];

        let cart = [];

        function renderProducts() {
            const grid = document.getElementById('product-grid');
            grid.innerHTML = products.map(p => `
                <div class="bg-card p-4 rounded-xl shadow-md flex flex-col justify-between border-b-4 border-red-100">
                    <div class="text-5xl text-center mb-4">${p.emoji}</div>
                    <div>
                        <h4 class="font-bold text-sm md:text-base leading-tight mb-1 h-10 overflow-hidden">${p.name}</h4>
                        <p class="text-xl font-black text-blue-900 mb-3">R$ ${p.price.toFixed(2).replace('.',',')}</p>
                    </div>
                    <button onclick="addToCart(${p.id})" class="w-full bg-blue-900 text-white py-2 rounded-lg font-bold text-sm hover:bg-red-600 transition">
                        + Carrinho
                    </button>
                </div>
            `).join('');
        }

        function addToCart(id) {
            const product = products.find(p => p.id === id);
            const existing = cart.find(item => item.id === id);
            if(existing) existing.quantity++;
            else cart.push({ ...product, quantity: 1 });
            updateCart();
            showToast();
        }

        function updateCart() {
            const list = document.getElementById('cart-items');
            const totalEl = document.getElementById('cart-total');
            const countEl = document.getElementById('cart-count');
            const summary = document.getElementById('cart-summary');
            
            const totalItems = cart.reduce((acc, i) => acc + i.quantity, 0);
            countEl.innerText = totalItems;

            if(cart.length === 0) {
                list.innerHTML = '<p class="text-gray-400 italic text-center py-10">Carrinho vazio...</p>';
                summary.classList.add('hidden');
                return;
            }

            summary.classList.remove('hidden');
            let total = 0;
            list.innerHTML = cart.map(item => {
                const subtotal = item.price * item.quantity;
                total += subtotal;
                return `
                    <div class="flex justify-between items-center bg-gray-50 p-3 rounded-lg border">
                        <div class="flex-1">
                            <p class="font-bold text-sm">${item.name}</p>
                            <div class="flex items-center gap-2 mt-1">
                                <button onclick="changeQty(${item.id}, -1)" class="bg-gray-200 px-2 rounded">-</button>
                                <span class="font-bold text-blue-600">${item.quantity}</span>
                                <button onclick="changeQty(${item.id}, 1)" class="bg-gray-200 px-2 rounded">+</button>
                            </div>
                        </div>
                        <div class="text-right">
                            <p class="font-bold">R$ ${subtotal.toFixed(2)}</p>
                            <button onclick="removeItem(${item.id})" class="text-red-500 text-xs mt-1">Remover</button>
                        </div>
                    </div>
                `;
            }).join('');

            totalEl.innerText = `R$ ${total.toFixed(2).replace('.',',')}`;
            togglePaymentFields(); // Atualiza cálculo de troco se estiver aberto
        }

        function changeQty(id, delta) {
            const item = cart.find(i => i.id === id);
            item.quantity += delta;
            if(item.quantity <= 0) removeItem(id);
            else updateCart();
        }

        function removeItem(id) {
            cart = cart.filter(i => i.id !== id);
            updateCart();
        }

        function toggleCart() {
            document.getElementById('cart-sidebar').classList.toggle('active');
            document.getElementById('cart-overlay').classList.toggle('hidden');
        }

        function togglePaymentFields() {
            const method = document.querySelector('input[name="payment"]:checked').value;
            const field = document.getElementById('change-field');
            if(method === 'Dinheiro') field.classList.remove('hidden');
            else field.classList.add('hidden');
        }

        function showToast() {
            const toast = document.getElementById('toast');
            toast.style.opacity = "1";
            setTimeout(() => toast.style.opacity = "0", 1500);
        }

        function checkout() {
            const name = document.getElementById('cust-name').value;
            const address = document.getElementById('cust-address').value;
            const method = document.querySelector('input[name="payment"]:checked').value;
            const changeVal = document.getElementById('change-amount').value;
            const obs = document.getElementById('cust-obs').value;

            if(!name || !address) {
                alert("⚠️ Informe seu nome e endereço de entrega!");
                return;
            }

            const total = cart.reduce((acc, i) => acc + (i.price * i.quantity), 0);
            
            let message = `🛒 *PEDIDO ONLINE - SIBAREZ*\n\n`;
            message += `👤 *Nome:* ${name}\n`;
            message += `📍 *Entrega:* ${address}\n`;
            message += `💳 *Pagamento:* ${method}\n`;
            
            if(method === 'Cartão') {
                message += `⚠️ _(Lembrando: Sem parcelamento)_\n`;
            }

            if(method === 'Dinheiro' && changeVal) {
                const troco = parseFloat(changeVal) - total;
                if(troco > 0) {
                    message += `💵 *Troco para:* R$ ${parseFloat(changeVal).toFixed(2)}\n`;
                    message += `↩️ *Levar troco de:* R$ ${troco.toFixed(2)}\n`;
                } else if (troco < 0) {
                    alert("O valor em dinheiro deve ser maior que o total da compra!");
                    return;
                }
            }

            if(obs) message += `📝 *Obs:* ${obs}\n`;
            message += `\n--------------------------\n`;
            
            cart.forEach(item => {
                message += `▪️ ${item.quantity}x ${item.name} (R$ ${item.price.toFixed(2)})\n`;
            });
            
            message += `--------------------------\n`;
            message += `💰 *TOTAL: R$ ${total.toFixed(2).replace('.',',')}*\n\n`;
            message += `🏠 *Pode entregar agora no endereço acima?*`;

            const phone = "558487738157";
            const url = `https://wa.me/${phone}?text=${encodeURIComponent(message)}`;
            window.open(url, '_blank');
        }

        window.onload = renderProducts;
    </script>
</body>
</html>

