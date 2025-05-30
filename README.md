<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Descifra el Significado: Empareja el Signo</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Estilos personalizados para el juego */
        body {
            font-family: 'Inter', sans-serif; /* Fuente Inter para una apariencia moderna */
            background-color: #f0f4f8; /* Color de fondo suave */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh; /* Altura mínima para centrar el contenido */
            padding: 1rem; /* Espaciado general */
        }
        .game-container {
            background-color: #ffffff; /* Fondo blanco para el contenedor del juego */
            border-radius: 1.5rem; /* Bordes redondeados */
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1); /* Sombra suave */
            padding: 2.5rem; /* Relleno interno */
            max-width: 900px; /* Ancho máximo del juego */
            width: 100%;
            text-align: center;
        }
        .image-card, .meaning-box {
            background-color: #e2e8f0; /* Fondo gris claro para las tarjetas */
            border-radius: 0.75rem; /* Bordes redondeados */
            padding: 1rem; /* Relleno */
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            cursor: grab; /* Cursor para indicar que es arrastrable */
            transition: transform 0.2s ease-in-out, box-shadow 0.2s ease-in-out; /* Transiciones suaves */
            min-height: 120px; /* Altura mínima para las tarjetas */
            text-align: center;
            border: 2px solid transparent; /* Borde transparente por defecto */
        }
        .image-card:hover {
            transform: translateY(-5px); /* Efecto al pasar el ratón */
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.15); /* Sombra al pasar el ratón */
        }
        .meaning-box {
            background-color: #f8fafc; /* Fondo más claro para las cajas de significado */
            border: 2px dashed #cbd5e1; /* Borde punteado para indicar que es un área de soltar */
            cursor: default; /* Cursor por defecto */
            min-height: 180px; /* Altura mínima para mostrar el significado */
            justify-content: flex-start; /* Alinear contenido arriba */
            padding-top: 1.5rem;
            position: relative; /* Para posicionar el ícono de la imagen */
        }
        .meaning-box.drag-over {
            border-color: #60a5fa; /* Borde azul al arrastrar sobre ella */
            background-color: #eff6ff; /* Fondo azul claro al arrastrar sobre ella */
        }
        .meaning-box.matched {
            border-color: #34d399; /* Borde verde cuando se empareja correctamente */
            background-color: #d1fae5; /* Fondo verde claro cuando se empareja */
        }
        .meaning-text {
            margin-top: 0.75rem; /* Espacio entre la imagen y el texto del significado */
            color: #4b5563; /* Color de texto gris oscuro */
            font-size: 0.95rem;
            line-height: 1.4;
        }
        .meaning-box img {
            width: 60px; /* Tamaño del ícono de la imagen dentro de la caja de significado */
            height: 60px;
            object-fit: contain;
            margin-bottom: 0.5rem;
            position: absolute; /* Posicionar en la parte superior */
            top: 1rem;
            left: 50%;
            transform: translateX(-50%);
        }
        .image-card img {
            width: 80px; /* Tamaño de las imágenes en el banco */
            height: 80px;
            object-fit: contain;
            margin-bottom: 0.5rem;
        }
        .message-box {
            border-radius: 0.75rem;
            padding: 1rem;
            margin-top: 1.5rem;
            font-weight: 600;
        }
        /* Clases para los tipos de mensajes */
        .message-box.success {
            background-color: #d1fae5; /* green-200 */
            color: #065f46; /* green-800 */
        }
        .message-box.error {
            background-color: #fee2e2; /* red-200 */
            color: #991b1b; /* red-800 */
        }
        .message-box.warning {
            background-color: #fffbeb; /* yellow-200 */
            color: #92400e; /* yellow-800 */
        }
        .message-box.info {
            background-color: #dbeafe; /* blue-200 */
            color: #1e40af; /* blue-800 */
        }

        .hidden {
            display: none !important;
        }
        .restart-button {
            background-color: #3b82f6; /* Azul para el botón de reiniciar */
            color: white;
            padding: 0.75rem 1.5rem;
            border-radius: 0.75rem;
            font-weight: 600;
            transition: background-color 0.2s ease-in-out;
            margin-top: 2rem;
            cursor: pointer;
        }
        .restart-button:hover {
            background-color: #2563eb; /* Azul más oscuro al pasar el ratón */
        }

        /* Estilos para el modal */
        .modal {
            background-color: rgba(0, 0, 0, 0.75);
        }
        .modal-content {
            background-color: #ffffff;
            border-radius: 1.5rem;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
        }
        .generate-meaning-button {
            background-color: #a855f7; /* purple-500 */
            color: white;
            padding: 0.5rem 1rem;
            border-radius: 0.5rem;
            font-size: 0.875rem; /* text-sm */
            font-weight: 600;
            transition: background-color 0.2s ease-in-out;
            margin-top: 1rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .generate-meaning-button:hover {
            background-color: #9333ea; /* purple-600 */
        }
        .meaning-input {
            resize: vertical; /* Permite redimensionar verticalmente el textarea */
        }
    </style>
</head>
<body>
    <div class="game-container">
        <h1 class="text-4xl font-extrabold text-gray-900 mb-4">Descifra el Significado</h1>
        <h2 class="text-2xl font-semibold text-gray-700 mb-6">Empareja el Signo</h2>
        <p class="text-gray-600 mb-8">
            ¡Hola, Detective de Signos! Arrastra cada objeto a una de las cajas de significado. Luego, piensa: ¿Qué te viene a la mente cuando lo ves?
        </p>

        <button id="ask-professor-button" class="bg-indigo-600 text-white px-4 py-2 rounded-lg font-semibold hover:bg-indigo-700 transition-colors mb-8 flex items-center justify-center mx-auto">
            Pregunta al Profesor Saussure ✨
        </button>

        <div id="image-bank" class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4 mb-8">
            </div>

        <div id="meaning-boxes" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            </div>

        <div id="message-box" class="message-box hidden">
            </div>

        <button id="restart-button" class="restart-button hidden">
            Reiniciar Juego
        </button>
    </div>

    <div id="saussure-modal" class="modal fixed inset-0 flex items-center justify-center p-4 z-50 hidden">
        <div class="modal-content p-8 w-full max-w-md relative">
            <h3 class="text-2xl font-bold text-gray-800 mb-4">Pregunta al Profesor Saussure ✨</h3>
            <p class="text-gray-600 mb-6">¡El Profesor está listo para resolver tus dudas semióticas!</p>
            
            <textarea id="saussure-question-input" class="w-full p-3 border border-gray-300 rounded-lg mb-4 focus:outline-none focus:ring-2 focus:ring-blue-500" rows="4" placeholder="Escribe tu pregunta sobre semiótica o Saussure aquí..."></textarea>
            
            <button id="ask-saussure-button" class="bg-blue-600 text-white px-6 py-3 rounded-lg font-semibold hover:bg-blue-700 transition-colors w-full flex items-center justify-center">
                <span id="ask-saussure-text">Preguntar</span>
                <svg id="ask-saussure-spinner" class="animate-spin h-5 w-5 text-white ml-2 hidden" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle><path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path></svg>
            </button>
            
            <div id="saussure-answer-output" class="mt-6 p-4 bg-gray-100 rounded-lg border border-gray-200 text-gray-700 text-left max-h-60 overflow-y-auto">
                <p class="text-gray-500">La respuesta del Profesor aparecerá aquí...</p>
            </div>
            
            <button id="close-saussure-modal" class="absolute top-4 right-4 text-gray-500 hover:text-gray-700 text-2xl font-bold">&times;</button>
        </div>
    </div>

    <script>
        // Datos del juego: imágenes y sus significados
        // Cada objeto tiene un 'id', la ruta de la 'imagen', y su 'significado' asociado.
        const gameData = [
            { id: 'apple', image: 'https://placehold.co/100x100/FF5733/FFFFFF?text=Manzana', meaning: 'Fruta, alimento, salud, conocimiento.' },
            { id: 'clock', image: 'https://placehold.co/100x100/3366FF/FFFFFF?text=Reloj', meaning: 'Tiempo, hora, puntualidad, urgencia.' },
            { id: 'chair', image: 'https://placehold.co/100x100/33FF57/FFFFFF?text=Silla', meaning: 'Descanso, sentarse, mobiliario.' },
            { id: 'stop-sign', image: 'https://placehold.co/100x100/FF0000/FFFFFF?text=STOP', meaning: 'Detenerse, alto, precaución, fin del camino.' }, // Este será el elemento de ejemplo pre-llenado
            { id: 'rain-cloud', image: 'https://placehold.co/100x100/808080/FFFFFF?text=Nube', meaning: 'Agua, mojarse, mal tiempo, crecimiento.' },
            { id: 'light-bulb', image: 'https://placehold.co/100x100/FFD700/FFFFFF?text=Idea', meaning: 'Idea, iluminación, invención, solución.' },
            { id: 'book', image: 'https://placehold.co/100x100/8B4513/FFFFFF?text=Libro', meaning: 'Conocimiento, lectura, historia, estudio.' }, // Nuevo elemento
            { id: 'heart', image: 'https://placehold.co/100x100/FF6347/FFFFFF?text=Corazon', meaning: 'Amor, emoción, vida, afecto.' } // Nuevo elemento
        ];

        // Elementos del DOM
        const imageBank = document.getElementById('image-bank');
        const meaningBoxesContainer = document.getElementById('meaning-boxes');
        const messageBox = document.getElementById('message-box');
        const restartButton = document.getElementById('restart-button');

        // Elementos del modal de Saussure
        const saussureModal = document.getElementById('saussure-modal');
        const saussureQuestionInput = document.getElementById('saussure-question-input');
        const askSaussureButton = document.getElementById('ask-saussure-button');
        const askSaussureText = document.getElementById('ask-saussure-text');
        const askSaussureSpinner = document.getElementById('ask-saussure-spinner');
        const saussureAnswerOutput = document.getElementById('saussure-answer-output');
        const closeSaussureModalButton = document.getElementById('close-saussure-modal');
        const askProfessorButton = document.getElementById('ask-professor-button');

        let draggedItemId = null; // Almacena el ID del elemento que se está arrastrando
        let filledBoxesCount = 0; // Contador de cajas llenas (incluyendo el ejemplo)
        const exampleItemId = 'stop-sign'; // El ID del elemento que mostrará el significado predefinido como ejemplo

        // Función para mezclar un array (algoritmo de Fisher-Yates)
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]]; // Intercambia elementos
            }
        }

        // Función para crear una caja de significado
        function createMeaningBox(item, isExample = false) {
            const box = document.createElement('div');
            box.classList.add('meaning-box', 'p-6', 'rounded-xl', 'shadow-inner', 'relative');
            box.setAttribute('data-meaning-id', item.id); // Asignar el ID del objeto a la caja

            const meaningContent = document.createElement('div');
            meaningContent.classList.add('meaning-content', 'flex', 'flex-col', 'items-center', 'justify-center', 'h-full');
            box.appendChild(meaningContent);

            const initialInstruction = document.createElement('p');
            initialInstruction.classList.add('text-gray-500', 'text-center', 'text-sm', 'initial-instruction');
            initialInstruction.textContent = 'Arrastra un objeto aquí';
            meaningContent.appendChild(initialInstruction);

            const imgPlaceholder = document.createElement('img');
            imgPlaceholder.classList.add('hidden');
            imgPlaceholder.setAttribute('data-image-placeholder', '');
            meaningContent.appendChild(imgPlaceholder);

            const textPlaceholder = document.createElement('p');
            textPlaceholder.classList.add('meaning-text', 'hidden');
            textPlaceholder.setAttribute('data-meaning-text-placeholder', '');
            meaningContent.appendChild(textPlaceholder);

            const meaningInput = document.createElement('textarea');
            meaningInput.classList.add('meaning-input', 'w-full', 'p-2', 'border', 'border-gray-300', 'rounded-md', 'mt-2', 'hidden');
            meaningInput.setAttribute('rows', '3');
            meaningInput.setAttribute('placeholder', 'Escribe tu significado aquí...');
            meaningContent.appendChild(meaningInput);

            const saveMeaningButton = document.createElement('button');
            saveMeaningButton.classList.add('save-meaning-button', 'bg-blue-500', 'text-white', 'px-4', 'py-2', 'rounded-md', 'mt-2', 'hidden', 'hover:bg-blue-600', 'transition-colors');
            saveMeaningButton.textContent = 'Guardar Significado';
            meaningContent.appendChild(saveMeaningButton);

            if (isExample) {
                box.classList.add('matched');
                box.classList.remove('border-dashed');
                initialInstruction.classList.add('hidden');
                imgPlaceholder.src = item.image;
                imgPlaceholder.alt = item.id;
                imgPlaceholder.classList.remove('hidden');
                textPlaceholder.textContent = item.meaning; // Mostrar significado predefinido
                textPlaceholder.classList.remove('hidden');
                filledBoxesCount++; // Incrementar contador para el elemento pre-llenado
            } else {
                // Añadir listeners de arrastrar/soltar para las cajas regulares
                box.addEventListener('dragover', (e) => {
                    e.preventDefault();
                    if (!e.currentTarget.classList.contains('matched')) {
                        e.currentTarget.classList.add('drag-over');
                    }
                });

                box.addEventListener('dragleave', (e) => {
                    e.currentTarget.classList.remove('drag-over');
                });

                box.addEventListener('drop', (e) => {
                    e.preventDefault();
                    e.currentTarget.classList.remove('drag-over');

                    const droppedItemId = e.dataTransfer.getData('text/plain');
                    const droppedItem = gameData.find(data => data.id === droppedItemId);

                    if (e.currentTarget.classList.contains('matched')) {
                        showMessage("¡Esa caja ya tiene un objeto asignado!", 'warning');
                        return;
                    }
                    
                    if (droppedItemId === exampleItemId) {
                        showMessage("El cartel de STOP es un ejemplo y ya está en su lugar.", 'warning');
                        return;
                    }

                    const imageElement = document.querySelector(`.image-card[data-id="${droppedItemId}"]`);
                    if (imageElement) {
                        imageElement.remove();
                    }

                    e.currentTarget.classList.add('matched');
                    e.currentTarget.classList.remove('border-dashed');

                    initialInstruction.classList.add('hidden');
                    imgPlaceholder.src = droppedItem.image;
                    imgPlaceholder.alt = droppedItem.id;
                    imgPlaceholder.classList.remove('hidden');
                    
                    e.currentTarget.setAttribute('data-dropped-item-id', droppedItemId);

                    textPlaceholder.classList.add('hidden');
                    meaningInput.classList.remove('hidden');
                    saveMeaningButton.classList.remove('hidden');

                    showMessage("¡Objeto colocado! Ahora, escribe tu propio significado para este objeto.", 'info');

                    if (!e.currentTarget.hasAttribute('data-save-listener-added')) {
                        saveMeaningButton.addEventListener('click', () => {
                            const userMeaning = meaningInput.value.trim();
                            if (userMeaning) {
                                textPlaceholder.textContent = userMeaning;
                                textPlaceholder.classList.remove('hidden');
                                meaningInput.classList.add('hidden');
                                saveMeaningButton.classList.add('hidden');
                                filledBoxesCount++;
                                showMessage("¡Significado guardado! Ahora puedes generar una interpretación alternativa.", 'success');
                                addGenerateMeaningButton(e.currentTarget, e.currentTarget.getAttribute('data-dropped-item-id')); 
                                checkGameCompletion();
                            } else {
                                showMessage("Por favor, escribe un significado antes de guardar.", 'warning');
                            }
                        });
                        e.currentTarget.setAttribute('data-save-listener-added', 'true');
                    }
                });
            }
            return box;
        }

        // Inicializa el juego
        function initializeGame() {
            // Reiniciar el estado del juego
            imageBank.innerHTML = '';
            meaningBoxesContainer.innerHTML = '';
            messageBox.classList.add('hidden');
            restartButton.classList.add('hidden');
            filledBoxesCount = 0;

            // Obtener el elemento de ejemplo
            const exampleItem = gameData.find(item => item.id === exampleItemId);
            // Obtener los elementos no-ejemplo y mezclarlos para el banco de imágenes
            const nonExampleItems = gameData.filter(item => item.id !== exampleItemId);
            shuffleArray(nonExampleItems);

            // Crear la primera caja de significado con el ejemplo pre-llenado
            const exampleMeaningBox = createMeaningBox(exampleItem, true);
            meaningBoxesContainer.appendChild(exampleMeaningBox);

            // Crear las demás cajas de significado vacías
            // Necesitamos tantas cajas vacías como elementos no-ejemplo
            for (let i = 0; i < nonExampleItems.length; i++) {
                // Pasamos un item dummy porque la caja no se "empareja" con un item específico al inicio,
                // sino que espera cualquier drop. El ID de la caja no importa para el drop inicial.
                // Sin embargo, para mantener la estructura y la consistencia con `data-meaning-id`,
                // podemos usar el ID del item que *podría* ir ahí si fuera un emparejamiento fijo,
                // o simplemente un ID único. Para este caso, usaremos el ID del item correspondiente
                // de `nonExampleItems` para que cada caja tenga un `data-meaning-id` único.
                const dummyItemForBox = nonExampleItems[i];
                const newBox = createMeaningBox(dummyItemForBox, false);
                meaningBoxesContainer.appendChild(newBox);
            }


            // Crear las tarjetas de imagen arrastrables para los elementos no-ejemplo
            nonExampleItems.forEach(item => {
                const card = document.createElement('div');
                card.classList.add('image-card', 'p-4', 'rounded-xl', 'shadow-md', 'hover:shadow-lg', 'cursor-grab', 'transform', 'hover:-translate-y-1');
                card.setAttribute('draggable', 'true');
                card.setAttribute('data-id', item.id);
                card.innerHTML = `<img src="${item.image}" alt="${item.id}" class="w-20 h-20 object-contain mb-2 mx-auto rounded-md">
                                  <p class="text-gray-800 font-medium capitalize">${item.id.replace('-', ' ')}</p>`;
                imageBank.appendChild(card);

                card.addEventListener('dragstart', (e) => {
                    draggedItemId = item.id;
                    e.dataTransfer.setData('text/plain', item.id);
                    e.currentTarget.classList.add('opacity-50');
                });

                card.addEventListener('dragend', (e) => {
                    e.currentTarget.classList.remove('opacity-50');
                });
            });

            checkGameCompletion(); // Comprobar la finalización inicialmente para el elemento pre-llenado
        }

        // Función para añadir el botón de "Generar Significado"
        function addGenerateMeaningButton(meaningBoxElement, itemId) {
            // No añadir el botón para el elemento de ejemplo (STOP)
            if (itemId === exampleItemId) {
                return;
            }
            // Comprobar si el botón ya existe para evitar duplicados
            if (meaningBoxElement.querySelector('.generate-meaning-button')) {
                return;
            }

            const generateMeaningButton = document.createElement('button');
            generateMeaningButton.classList.add('generate-meaning-button', 'mt-2', 'px-3', 'py-1', 'bg-purple-500', 'text-white', 'rounded-md', 'text-sm', 'hover:bg-purple-600', 'transition-colors', 'flex', 'items-center', 'justify-center', 'mx-auto');
            generateMeaningButton.innerHTML = `Genera un Significado ✨`;
            generateMeaningButton.setAttribute('data-item-id', itemId);
            meaningBoxElement.querySelector('.meaning-content').appendChild(generateMeaningButton);

            generateMeaningButton.addEventListener('click', async () => {
                // Mostrar indicador de carga
                const originalButtonText = generateMeaningButton.innerHTML;
                generateMeaningButton.innerHTML = `<svg class="animate-spin h-5 w-5 text-white mr-2" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle><path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path></svg> Generando...`;
                generateMeaningButton.disabled = true;

                const prompt = `Proporciona una interpretación cultural o un significado alternativo para el objeto "${itemId.replace('-', ' ')}". Explica brevemente cómo su significado puede variar según el contexto o la cultura. Sé conciso y educativo.`;

                try {
                    let chatHistory = [];
                    chatHistory.push({ role: "user", parts: [{ text: prompt }] });
                    const payload = { contents: chatHistory };
                    const apiKey = ""; // La clave API se proporciona en tiempo de ejecución por Canvas
                    const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

                    const response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });
                    const result = await response.json();

                    if (result.candidates && result.candidates.length > 0 &&
                        result.candidates[0].content && result.candidates[0].content.parts &&
                        result.candidates[0].content.parts.length > 0) {
                        const text = result.candidates[0].content.parts[0].text;
                        showMessage(`✨ Significado Alternativo para "${itemId.replace('-', ' ')}": ${text}`, 'info', false);
                    } else {
                        showMessage("No se pudo generar un significado alternativo. Inténtalo de nuevo.", 'error');
                    }
                } catch (error) {
                    console.error("Error al llamar a la API de Gemini:", error);
                    showMessage("Error al generar el significado. Verifica tu conexión.", 'error');
                } finally {
                    generateMeaningButton.innerHTML = originalButtonText;
                    generateMeaningButton.disabled = false;
                }
            });
        }

        // Función para comprobar si el juego ha terminado
        function checkGameCompletion() {
            // La condición de finalización es que todas las cajas (gameData.length) estén llenas.
            // filledBoxesCount se incrementa cuando el ejemplo se carga o cuando el usuario guarda un significado.
            if (filledBoxesCount === gameData.length) {
                showMessage("¡Felicidades, Detective de Signos! Has emparejado todos los signos correctamente y explorado sus significados. ¡Eres un maestro del significado!", 'success', true);
                restartButton.classList.remove('hidden');
            }
        }

        // Muestra mensajes al usuario
        function showMessage(msg, type, isFinal = false) {
            messageBox.textContent = msg;
            // Eliminar todas las clases de tipo anteriores
            messageBox.classList.remove('hidden', 'bg-bfdbfe', 'text-1e40af', 'bg-red-200', 'text-red-800', 'bg-green-200', 'text-green-800', 'bg-yellow-200', 'text-yellow-800', 'bg-blue-200', 'text-blue-800');

            // Asignar clases de Tailwind según el tipo de mensaje
            if (type === 'success') {
                messageBox.classList.add('success');
            } else if (type === 'error') {
                messageBox.classList.add('error');
            } else if (type === 'warning') {
                messageBox.classList.add('warning');
            } else if (type === 'info') {
                messageBox.classList.add('info');
            } else {
                messageBox.classList.add('bg-bfdbfe', 'text-1e40af'); // Azul predeterminado
            }

            // Si es un mensaje final, no lo ocultamos automáticamente
            if (!isFinal) {
                setTimeout(() => {
                    messageBox.classList.add('hidden');
                }, 3000); // Oculta el mensaje después de 3 segundos
            }
        }

        // Event listener para el botón de reiniciar
        restartButton.addEventListener('click', initializeGame);

        // Event listener para abrir el modal de Saussure
        askProfessorButton.addEventListener('click', () => {
            saussureModal.classList.remove('hidden');
            saussureAnswerOutput.innerHTML = '<p class="text-gray-500">La respuesta del Profesor aparecerá aquí...</p>'; // Restablecer salida
            saussureQuestionInput.value = ''; // Limpiar entrada
        });

        // Event listener para cerrar el modal de Saussure
        closeSaussureModalButton.addEventListener('click', () => {
            saussureModal.classList.add('hidden');
        });

        // Event listener para preguntar al Profesor Saussure
        askSaussureButton.addEventListener('click', async () => {
            const question = saussureQuestionInput.value.trim();
            if (!question) {
                showMessage("Por favor, escribe tu pregunta para el Profesor.", 'warning');
                return;
            }

            // Mostrar estado de carga
            askSaussureText.textContent = 'Pensando...';
            askSaussureSpinner.classList.remove('hidden');
            askSaussureButton.disabled = true;
            saussureAnswerOutput.innerHTML = '<p class="text-gray-500">El Profesor está consultando sus libros...</p>';

            try {
                let chatHistory = [];
                chatHistory.push({ role: "user", parts: [{ text: `Explica brevemente y de forma sencilla, como si fuera para un principiante en semiótica, la siguiente pregunta: "${question}". Enfócate en conceptos de Saussure si aplica.` }] });
                const payload = { contents: chatHistory };
                const apiKey = ""; // La clave API se proporciona en tiempo de ejecución por Canvas
                const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey}`;

                const response = await fetch(apiUrl, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                const result = await response.json();

                if (result.candidates && result.candidates.length > 0 &&
                    result.candidates[0].content && result.candidates[0].content.parts &&
                    result.candidates[0].content.parts.length > 0) {
                    const text = result.candidates[0].content.parts[0].text;
                    saussureAnswerOutput.innerHTML = `<p>${text}</p>`;
                } else {
                    saussureAnswerOutput.innerHTML = '<p class="text-red-500">Lo siento, el Profesor no pudo encontrar una respuesta clara en este momento. Intenta reformular tu pregunta.</p>';
                }
            } catch (error) {
                console.error("Error al llamar a la API de Gemini para Saussure:", error);
                saussureAnswerOutput.innerHTML = '<p class="text-red-500">Hubo un error al contactar al Profesor Saussure. Por favor, revisa tu conexión a internet.</p>';
            } finally {
                askSaussureText.textContent = 'Preguntar';
                askSaussureSpinner.classList.add('hidden');
                askSaussureButton.disabled = false;
            }
        });


        // Iniciar el juego cuando la ventana se carga
        window.onload = initializeGame;
    </script>
</body>
</html>
