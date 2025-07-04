# xu-mood-matcher
Projeto Interativo com Xu Brasil Store
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Xu Mood Matcher - Xu Brasil Store</title>
    <!-- Inclui Tailwind CSS para estilização rápida e responsiva -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Define a fonte Inter como padrão */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f0f0; /* Fundo cinza claro */
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }
        .container {
            background-color: #ffffff;
            border-radius: 1.5rem; /* Cantos arredondados */
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
            padding: 2rem;
            max-width: 600px;
            width: 100%;
            text-align: center;
        }
        .xu-logo {
            font-size: 4rem; /* Tamanho grande para o "Xu" */
            font-weight: bold;
            margin-bottom: 1rem;
            transition: color 0.5s ease-in-out; /* Transição suave de cor */
        }
        .mood-button {
            @apply bg-blue-500 text-white font-semibold py-3 px-6 rounded-xl shadow-md hover:shadow-lg transition-all duration-300 ease-in-out transform hover:-translate-y-1;
            margin: 0.5rem;
            min-width: 120px;
            cursor: pointer;
        }
        .mood-button:hover {
            filter: brightness(1.1); /* Pequeno brilho ao passar o mouse */
        }
        .result-section {
            margin-top: 2rem;
            padding-top: 1.5rem;
            border-top: 1px solid #eee;
        }
        .result-link {
            @apply inline-block bg-purple-600 text-white font-bold py-3 px-8 rounded-xl shadow-lg hover:bg-purple-700 transition-colors duration-300 mt-4;
        }

        /* Cores para as personalidades Xu */
        .xu-vibrante { color: #FF8C00; /* Laranja vibrante */ }
        .xu-tranquilo { color: #66CCEE; /* Azul céu */ }
        .xu-classico { color: #333333; /* Preto quase grafite */ }
        .xu-romantico { color: #FFB6C1; /* Rosa claro */ }
        .xu-urbano { color: #4A4A4A; /* Cinza chumbo */ }
    </style>
</head>
<body>
    <div class="container">
        <h1 class="text-3xl md:text-4xl font-bold text-gray-800 mb-4">Descubra o seu <span id="xuLogo" class="xu-logo">Xu</span> de hoje!</h1>
        <p class="text-gray-600 mb-8 text-lg">Qual a sua vibe? Escolha uma opção e encontre os produtos que combinam com você!</p>

        <div class="grid grid-cols-2 sm:grid-cols-3 gap-4">
            <button class="mood-button bg-orange-500" data-mood="vibrante">Vibrante</button>
            <button class="mood-button bg-blue-400" data-mood="tranquilo">Tranquilo</button>
            <button class="mood-button bg-gray-700" data-mood="classico">Clássico</button>
            <button class="mood-button bg-pink-400" data-mood="romantico">Romântico</button>
            <button class="mood-button bg-indigo-600" data-mood="urbano">Urbano</button>
            <button class="mood-button bg-green-500" data-mood="aventureiro">Aventureiro</button>
        </div>

        <div id="resultSection" class="result-section hidden">
            <h2 class="text-2xl font-semibold text-gray-700 mb-3">Seu Xu de hoje é: <span id="moodResult" class="font-bold"></span>!</h2>
            <p id="moodDescription" class="text-gray-600 mb-4"></p>
            <a id="productLink" href="#" class="result-link">Ver Produtos Xu!</a>
        </div>
    </div>

    <script>
        // Seleciona os elementos HTML
        const moodButtons = document.querySelectorAll('.mood-button');
        const xuLogo = document.getElementById('xuLogo');
        const resultSection = document.getElementById('resultSection');
        const moodResult = document.getElementById('moodResult');
        const moodDescription = document.getElementById('moodDescription');
        const productLink = document.getElementById('productLink');

        // Mapeia os moods para suas cores e descrições/links
        const xuMoods = {
            vibrante: {
                colorClass: 'xu-vibrante',
                description: 'Cheio(a) de energia, alegria e pronto(a) para brilhar!',
                link: 'https://www.seusite.com.br/colecao/vibrante' // SUBSTITUA PELA SUA URL REAL
            },
            tranquilo: {
                colorClass: 'xu-tranquilo',
                description: 'Em busca de paz, conforto e leveza para o seu dia a dia.',
                link: 'https://www.seusite.com.br/colecao/tranquilo' // SUBSTITUA PELA SUA URL REAL
            },
            classico: {
                colorClass: 'xu-classico',
                description: 'Elegância e sofisticação são a sua marca, com um toque atemporal.',
                link: 'https://www.seusite.com.br/colecao/classico' // SUBSTITUA PELA SUA URL REAL
            },
            romantico: {
                colorClass: 'xu-romantico',
                description: 'Delicadeza e charme em cada detalhe, com um toque de doçura.',
                link: 'https://www.seusite.com.br/colecao/romantico' // SUBSTITUA PELA SUA URL REAL
            },
            urbano: {
                colorClass: 'xu-urbano',
                description: 'Moderno(a), prático(a) e com um estilo que domina as ruas da cidade.',
                link: 'https://www.seusite.com.br/colecao/urbano' // SUBSTITUA PELA SUA URL REAL
            },
            aventureiro: {
                colorClass: 'xu-vibrante', // Pode usar uma cor existente ou criar uma nova
                description: 'Sempre em busca de novas experiências e pronto(a) para explorar!',
                link: 'https://www.seusite.com.br/colecao/aventureiro' // SUBSTITUA PELA SUA URL REAL
            }
        };

        // Adiciona um "listener" para cada botão de humor
        moodButtons.forEach(button => {
            button.addEventListener('click', () => {
                const selectedMood = button.dataset.mood; // Pega o valor do atributo 'data-mood'
                const moodData = xuMoods[selectedMood];

                // Remove todas as classes de cor existentes do logo
                Object.values(xuMoods).forEach(mood => {
                    xuLogo.classList.remove(mood.colorClass);
                });

                // Adiciona a classe de cor correspondente ao humor selecionado
                xuLogo.classList.add(moodData.colorClass);

                // Atualiza o texto do resultado
                moodResult.textContent = selectedMood.charAt(0).toUpperCase() + selectedMood.slice(1); // Capitaliza a primeira letra
                moodDescription.textContent = moodData.description;
                productLink.href = moodData.link;

                // Mostra a seção de resultados
                resultSection.classList.remove('hidden');

                // Rola para a seção de resultados para melhor UX em mobile
                resultSection.scrollIntoView({ behavior: 'smooth', block: 'start' });
            });
        });
    </script>
</body>
</html>
