<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerador de Citações e Pesquisa de Conhecimento (Gemini API)</title>
    <!-- Carrega Tailwind CSS para estilização e responsividade -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;500;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f7fafc;
        }
        .content-area {
            min-height: 200px;
        }
        .content-text {
            white-space: pre-wrap;
            word-wrap: break-word;
            line-height: 1.6;
        }
        .tab-button.active {
            border-color: #3b82f6;
            color: #3b82f6;
            background-color: #eff6ff;
        }
        .copy-button:active {
            transform: scale(0.98);
        }
        .history-item {
            cursor: pointer;
            transition: background-color 0.15s;
        }
        .history-item:hover {
            background-color: #f0f4f8;
        }
        .citation {
            font-size: 0.75rem; /* text-xs */
            color: #4b5563; /* text-gray-600 */
            display: block;
            margin-top: 0.5rem;
        }
        .citation a {
            color: #2563eb;
            text-decoration: none;
        }
        .citation a:hover {
            text-decoration: underline;
        }
        /* Ajuste do espaçamento após a remoção do h1 */
        .w-full.max-w-4xl {
            padding-top: 1rem;
        }
    </style>
</head>
<body class="p-4 sm:p-8 flex items-start justify-center min-h-screen">

    <!-- Container Principal Flexível -->
    <div class="w-full max-w-4xl">

        <!-- O TÍTULO PRINCIPAL FOI REMOVIDO AQUI -->

        <!-- Controle de Abas -->
        <div class="flex border-b border-gray-200 mb-6 mt-4">
            <button id="tab-quote" onclick="selectTab('quote')" 
                    class="tab-button active flex-1 py-3 px-4 text-sm sm:text-lg font-semibold rounded-t-xl transition duration-150 border-b-4 border-transparent hover:border-blue-300">
                ✨ Gerador de Citações (Amor e Carinho)
            </button>
            <button id="tab-search" onclick="selectTab('search')" 
                    class="tab-button flex-1 py-3 px-4 text-sm sm:text-lg font-semibold rounded-t-xl transition duration-150 border-b-4 border-transparent hover:border-blue-300">
                📚 Pesquisa de Conhecimento (Web)
            </button>
        </div>
        
        <!-- Conteúdo Principal - Grid Responsivo -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">

            <!-- Coluna 1: Gerador de Citações -->
            <div id="content-quote" class="tab-content lg:col-span-2 bg-white p-6 sm:p-8 rounded-2xl shadow-2xl border border-gray-100">
                
                <h2 class="text-2xl font-bold text-center text-gray-800 mb-4">Gerar Inspiração Personalizada</h2>

                <!-- Controles de Personalização -->
                <div class="space-y-4 mb-6 p-4 bg-blue-50 rounded-xl border border-blue-200">
                    <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                        <!-- Assunto -->
                        <div>
                            <label for="topic-input" class="block text-sm font-medium text-gray-700 mb-1">Assunto da Citação:</label>
                            <input type="text" id="topic-input" placeholder="Ex: Amor, Coragem, O futuro..." value="Amor expresso em voz alta"
                                   class="w-full p-3 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 transition duration-150 shadow-sm">
                        </div>
    
                        <!-- Tom/Estilo -->
                        <div>
                            <label for="style-input" class="block text-sm font-medium text-gray-700 mb-1">Tom/Estilo:</label>
                            <select id="style-input"
                                    class="w-full p-3 border border-gray-300 rounded-lg bg-white focus:ring-blue-500 focus:border-blue-500 transition duration-150 shadow-sm">
                                <option value="Poético e Profundo">Poético e Profundo (Padrão)</option>
                                <option value="Direto e Motivacional">Direto e Motivacional</option>
                                <option value="Cômico e Leve">Cômico e Leve</option>
                                <option value="Filosófico e Reflexivo">Filosófico e Reflexivo</option>
                            </select>
                        </div>
                    </div>
    
                    <!-- Seletor de Ícone -->
                    <div>
                        <label for="icon-input" class="block text-sm font-medium text-gray-700 mb-1">Ícone Decorativo:</label>
                        <select id="icon-input"
                                class="w-full p-3 border border-gray-300 rounded-lg bg-white text-xl focus:ring-blue-500 focus:border-blue-500 transition duration-150 shadow-sm">
                            <option value="💡">💡 (Ideia)</option>
                            <option value="❤️">❤️ (Coração)</option>
                            <option value="✨" selected>✨ (Brilho)</option>
                            <option value="🌱">🌱 (Crescimento)</option>
                            <option value="🚀">🚀 (Inovação)</option>
                            <option value="🧠">🧠 (Mente)</option>
                            <option value="🕊️">🕊️ (Paz)</option>
                        </select>
                    </div>
                </div>
                
                <!-- Contêiner de Exibição da Citação -->
                <div id="quote-container" class="content-area bg-gray-50 p-6 rounded-xl border border-gray-200 mb-6 flex items-center justify-center text-center shadow-inner">
                    <p id="quote-text" class="content-text text-xl italic text-gray-600 font-medium">
                        Defina sua inspiração e clique em "Gerar Citação".
                    </p>
                </div>
    
                <!-- Mensagem de Status (Carregamento / Erro) -->
                <div class="min-h-[20px] mb-4 text-center">
                    <p id="quote-status-message" class="text-blue-600 text-sm font-semibold"></p>
                </div>
    
                <!-- Botões de Ação -->
                <div class="flex flex-col sm:flex-row space-y-4 sm:space-y-0 sm:space-x-4">
                    
                    <!-- Botão Gerar Nova Citação -->
                    <button id="generate-button" onclick="generateQuote()" 
                            class="w-full py-3 px-4 bg-blue-600 text-white font-semibold rounded-lg shadow-lg hover:bg-blue-700 focus:outline-none focus:ring-4 focus:ring-blue-500/50 transition ease-in-out duration-150">
                        Gerar Citação Personalizada
                    </button>
                    
                    <!-- Botão Copiar -->
                    <button id="quote-copy-button" onclick="copyQuote()" disabled
                            class="copy-button w-full py-3 px-4 bg-green-500 text-white font-semibold rounded-lg shadow-lg hover:bg-green-600 focus:outline-none focus:ring-4 focus:ring-green-500/50 transition ease-in-out duration-150 disabled:opacity-50 disabled:cursor-not-allowed">
                        Copiar Citação
                    </button>
                </div>
                
                <!-- Mensagem de Copiado (Aparece ao clicar em Copiar) -->
                <p id="quote-copy-message" class="mt-4 text-center text-sm font-semibold text-green-600 opacity-0 transition-opacity duration-300"></p>
            </div>

            <!-- Coluna 2: Pesquisa de Conhecimento OU Histórico (Alterna entre os dois com a aba) -->
            <div class="lg:col-span-1">
                
                <!-- Pesquisa de Conhecimento (Visível quando a aba 'search' está ativa) -->
                <div id="content-search" class="tab-content hidden bg-white p-6 sm:p-6 rounded-2xl shadow-2xl border border-gray-100 lg:h-full flex flex-col">
                    <h2 class="text-2xl font-bold text-gray-800 mb-4 text-center">Pesquisa em Tempo Real na Web</h2>
                    
                    <input type="text" id="search-query-input" placeholder="Digite seu tópico de pesquisa (Ex: O que é IA generativa?)"
                           class="w-full p-3 border border-gray-300 rounded-lg focus:ring-red-500 focus:border-red-500 transition duration-150 shadow-sm mb-4">
                    
                    <button id="search-button" onclick="performSearch()" 
                            class="w-full py-3 px-4 bg-red-600 text-white font-semibold rounded-lg shadow-lg hover:bg-red-700 focus:outline-none focus:ring-4 focus:ring-red-500/50 transition ease-in-out duration-150 mb-4">
                        Pesquisar
                    </button>
                    
                    <!-- Contêiner de Exibição dos Resultados da Pesquisa -->
                    <div id="search-results-container" class="content-area flex-grow bg-gray-50 p-4 rounded-xl border border-gray-200 shadow-inner overflow-auto">
                        <p id="search-results-text" class="content-text text-sm text-gray-600">
                            Digite um tópico e clique em "Pesquisar" para ver um resumo do conhecimento atualizado da web.
                        </p>
                    </div>

                    <!-- Mensagem de Status da Pesquisa -->
                    <div class="min-h-[20px] mt-4 text-center">
                        <p id="search-status-message" class="text-red-600 text-sm font-semibold"></p>
                    </div>
                </div>

                <!-- Histórico de Citações (Visível quando a aba 'quote' está ativa) -->
                <div id="history-section" class="tab-content lg:h-full bg-white p-6 sm:p-6 rounded-2xl shadow-2xl border border-gray-100">
                    <h2 class="text-xl font-bold text-gray-800 mb-4 flex items-center">
                        <span class="mr-2 text-xl">💾</span> Histórico de Citações
                    </h2>
                    <div id="history-list" class="space-y-3">
                        <p id="empty-history" class="text-gray-500 italic text-sm">Nenhuma citação salva ainda.</p>
                        <!-- Citações serão injetadas aqui -->
                    </div>
                    <button id="clear-history-button" onclick="clearHistory()"
                            class="mt-4 w-full py-2 text-sm text-red-600 border border-red-300 rounded-lg hover:bg-red-50 transition duration-150">
                        Limpar Histórico
                    </button>
                </div>
            </div>
            
        </div>
    </div>

    <script type="module">
        // Variáveis globais para configuração da API
        const apiKey = ""; // A chave API será injetada em tempo de execução
        const API_MODEL = "gemini-2.5-flash-preview-09-2025";
        const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/${API_MODEL}:generateContent?key=${apiKey}`;

        // Chave do localStorage para o histórico
        const HISTORY_STORAGE_KEY = 'gemini_quote_history';
        const MAX_HISTORY_ITEMS = 5;

        // Elementos do DOM - Citações
        const quoteTextElement = document.getElementById('quote-text');
        const quoteStatusMessageElement = document.getElementById('quote-status-message');
        const generateButton = document.getElementById('generate-button');
        const quoteCopyButton = document.getElementById('quote-copy-button');
        const quoteCopyMessageElement = document.getElementById('quote-copy-message');
        
        // Elementos do DOM - Pesquisa
        const searchQueryInput = document.getElementById('search-query-input');
        const searchResultsTextElement = document.getElementById('search-results-text');
        const searchStatusMessageElement = document.getElementById('search-status-message');
        const searchButton = document.getElementById('search-button');

        // Elementos do DOM - Histórico
        const historyListElement = document.getElementById('history-list');
        const emptyHistoryMessage = document.getElementById('empty-history');
        const clearHistoryButton = document.getElementById('clear-history-button');
        
        // Elementos de input - Citações
        const topicInput = document.getElementById('topic-input');
        const styleInput = document.getElementById('style-input');
        const iconInput = document.getElementById('icon-input'); 

        // --- Funções de Histórico (localStorage) ---

        function loadHistory() {
            try {
                const historyJson = localStorage.getItem(HISTORY_STORAGE_KEY);
                return historyJson ? JSON.parse(historyJson) : [];
            } catch (e) {
                console.error("Erro ao carregar histórico do localStorage:", e);
                return [];
            }
        }

        function saveQuoteToHistory(quoteWithIcon) {
            const history = loadHistory();
            history.unshift(quoteWithIcon);
            const limitedHistory = history.slice(0, MAX_HISTORY_ITEMS);
            
            try {
                localStorage.setItem(HISTORY_STORAGE_KEY, JSON.stringify(limitedHistory));
                displayHistory(limitedHistory);
            } catch (e) {
                console.error("Erro ao salvar histórico no localStorage:", e);
            }
        }
        
        function displayHistory(history) {
            historyListElement.innerHTML = '';
            
            if (history.length === 0) {
                emptyHistoryMessage.style.display = 'block';
                clearHistoryButton.style.display = 'none';
                return;
            }

            emptyHistoryMessage.style.display = 'none';
            clearHistoryButton.style.display = 'block';

            history.forEach((item) => {
                const historyItem = document.createElement('div');
                historyItem.className = 'history-item p-3 rounded-lg border border-gray-200 text-sm text-gray-700 truncate';
                historyItem.textContent = item;
                historyItem.title = item;
                
                historyItem.onclick = () => {
                    copyTextToClipboard(item, true, quoteCopyMessageElement);
                };

                historyListElement.appendChild(historyItem);
            });
        }
        
        window.clearHistory = function() {
            localStorage.removeItem(HISTORY_STORAGE_KEY);
            displayHistory([]);
            quoteTextElement.textContent = 'Defina sua inspiração e clique em "Gerar Citação".';
            updateQuoteUI(false, 'Histórico limpo.');
        }

        // --- Funções de Utilidade (UI e API) ---
        
        // Alterna entre as abas
        window.selectTab = function(tabName) {
            const tabs = ['quote', 'search'];
            tabs.forEach(tab => {
                document.getElementById(`tab-${tab}`).classList.remove('active');
                document.getElementById(`content-${tab}`).classList.add('hidden');
            });
            document.getElementById(`tab-${tabName}`).classList.add('active');
            document.getElementById(`content-${tabName}`).classList.remove('hidden');

            // Mantém o histórico visível ao lado das citações
            document.getElementById('history-section').classList.remove('hidden');
        }

        // Atualiza a UI do Gerador de Citações
        function updateQuoteUI(isLoading, message = '') {
            generateButton.disabled = isLoading;
            generateButton.textContent = isLoading ? 'Gerando...' : 'Gerar Citação Personalizada';
            quoteStatusMessageElement.textContent = message;
            
            const isQuotePresent = quoteTextElement.textContent.trim() !== '' && 
                                   !quoteTextElement.textContent.includes('Defina sua inspiração') &&
                                   !quoteTextElement.textContent.includes('ERRO') &&
                                   !quoteTextElement.textContent.includes('bloqueada');
            
            quoteCopyButton.disabled = isLoading || !isQuotePresent;
        }

        // Atualiza a UI da Pesquisa de Conhecimento
        function updateSearchUI(isLoading, message = '') {
            searchButton.disabled = isLoading;
            searchButton.textContent = isLoading ? 'Pesquisando...' : 'Pesquisar';
            searchStatusMessageElement.textContent = message;
            searchQueryInput.disabled = isLoading;
        }


        // Função para realizar a chamada à API com retries (Backoff Exponencial)
        async function fetchWithExponentialBackoff(payload, maxRetries = 5) {
            // A chave API é injetada automaticamente no nosso ambiente, então removemos a checagem manual de erro.
            // if (apiKey === "") {
            //     throw new Error("ERRO DE CONFIGURAÇÃO: Insira sua Chave API no código para uso externo.");
            // }

            for (let i = 0; i < maxRetries; i++) {
                try {
                    const response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });

                    if (response.ok) {
                        return response.json();
                    }

                    if (response.status === 429 || response.status >= 500) {
                        throw new Error(`API error with status ${response.status}. Retrying...`);
                    } else {
                        const errorBody = await response.json();
                        throw new Error(`API failed: ${errorBody.error?.message || response.statusText}`);
                    }
                } catch (error) {
                    if (i === maxRetries - 1) {
                        throw error;
                    }
                    const delay = Math.pow(2, i) * 1000;
                    await new Promise(resolve => setTimeout(resolve, delay));
                }
            }
        }

        // Função para copiar o texto para a área de transferência
        function copyTextToClipboard(text, isHistory = false, messageElement = quoteCopyMessageElement) {
            const tempInput = document.createElement('textarea');
            tempInput.value = text;
            document.body.appendChild(tempInput);
            
            tempInput.select();
            tempInput.setSelectionRange(0, 99999);
            
            try {
                document.execCommand('copy');
                
                messageElement.textContent = isHistory ? '✅ Citação do histórico copiada!' : '✅ Copiado para a área de transferência!';
                messageElement.style.opacity = '1';

                setTimeout(() => {
                    messageElement.style.opacity = '0';
                }, 3000);

            } catch (err) {
                messageElement.textContent = '❌ Falha ao copiar. Tente selecionar o texto manualmente.';
                messageElement.style.opacity = '1';
                console.error('Erro ao copiar:', err);
            }

            document.body.removeChild(tempInput);
        }

        // --- Funções do Gerador de Citações ---
        window.generateQuote = async function() {
            const topic = topicInput.value.trim();
            const style = styleInput.value;
            const icon = iconInput.value; 

            if (!topic) {
                updateQuoteUI(false, 'Por favor, insira um assunto para a citação.');
                quoteTextElement.textContent = 'O assunto da citação não pode estar vazio.';
                return;
            }

            updateQuoteUI(true, `Gerando citação sobre "${topic}" no estilo "${style}"...`);
            quoteTextElement.innerHTML = '';
            quoteCopyMessageElement.style.opacity = '0';

            // PROMPT de sistema para gerar citações de amor e carinho
            const systemPrompt = `Você é um mentor de vida e filosofia focado em criar citações de amor e carinho, pois a citação será usada para uma surpresa romântica para uma pessoa que gosta de se sentir amada em voz alta. Sua tarefa é criar uma citação inspiradora única e motivacional, com um toque ${style}. A citação deve ter no máximo duas frases. Formate a saída como um bloco de texto simples, sem aspas, nomes de autor ou qualquer introdução.`;
            
            const userQuery = `Gere uma citação original sobre o assunto: "${topic}".`;

            const payload = {
                contents: [{ parts: [{ text: userQuery }] }],
                systemInstruction: {
                    parts: [{ text: systemPrompt }]
                },
                generationConfig: { 
                    temperature: 0.9,
                    maxOutputTokens: 1024 
                }
            };

            try {
                const result = await fetchWithExponentialBackoff(payload);
                const generatedText = result.candidates?.[0]?.content?.parts?.[0]?.text;
                
                if (generatedText) {
                    const finalQuote = generatedText.trim();
                    const quoteWithIcon = `${icon} ${finalQuote}`;
                    
                    quoteTextElement.innerHTML = `<span class="text-3xl mr-2">${icon}</span> ${finalQuote}`;
                    updateQuoteUI(false, 'Citação gerada com sucesso! Use-a com carinho!');
                    saveQuoteToHistory(quoteWithIcon);

                } else {
                    const blockReason = result.candidates?.[0]?.finishReason;
                    console.error('Resposta da API sem texto. Objeto de resultado completo:', result);
                    
                    let errorMessage = 'ERRO: Falha ao gerar texto.';
                    if (blockReason === 'SAFETY') {
                        errorMessage = '⚠️ A citação foi bloqueada pelo filtro de segurança da API. Tente outro tema.';
                    } else if (blockReason === 'MAX_TOKENS') { 
                        errorMessage = '⚠️ O modelo atingiu o limite de tokens e cortou a citação. Tente um assunto mais conciso.';
                    }
                    
                    quoteTextElement.textContent = errorMessage;
                    updateQuoteUI(false, 'Houve um erro.');
                }

            } catch (error) {
                quoteTextElement.textContent = error.message;
                updateQuoteUI(false, 'Houve um erro grave.');
                console.error('API Error:', error);
            }
        };

        window.copyQuote = function() {
            const quoteElementWithIcon = quoteTextElement.textContent.trim();
            
            if (quoteElementWithIcon && quoteCopyButton.disabled === false) {
                copyTextToClipboard(quoteElementWithIcon, false, quoteCopyMessageElement);
            }
        };

        // --- Função de Pesquisa de Conhecimento ---
        window.performSearch = async function() {
            const searchQuery = searchQueryInput.value.trim();

            if (!searchQuery) {
                updateSearchUI(false, 'Por favor, insira um tópico para pesquisar.');
                searchResultsTextElement.textContent = 'O campo de pesquisa não pode estar vazio.';
                return;
            }

            updateSearchUI(true, `Pesquisando na web sobre "${searchQuery}"...`);
            searchResultsTextElement.innerHTML = '<p class="text-center text-sm text-gray-500">Buscando e resumindo informações...</p>';

            const systemPrompt = "Você é um assistente de pesquisa e resumo. Use as ferramentas de busca para encontrar informações relevantes sobre o tópico do usuário e forneça um resumo conciso e completo (máximo 5 parágrafos) em Português. Inclua todas as fontes no final da sua resposta, formatadas como links clicáveis com o título da fonte.";

            const payload = {
                contents: [{ parts: [{ text: searchQuery }] }],
                // Habilita a pesquisa na web (grounding)
                tools: [{ "google_search": {} }],
                systemInstruction: {
                    parts: [{ text: systemPrompt }]
                },
                generationConfig: { 
                    temperature: 0.3,
                    maxOutputTokens: 2048 
                }
            };

            try {
                const result = await fetchWithExponentialBackoff(payload);
                const candidate = result.candidates?.[0];
                const generatedText = candidate?.content?.parts?.[0]?.text;
                
                if (generatedText) {
                    let htmlContent = generatedText.trim().replace(/\n/g, '<br>');
                    
                    // 1. Extrai as fontes de citação (citações)
                    let sources = [];
                    const groundingMetadata = candidate.groundingMetadata;
                    if (groundingMetadata && groundingMetadata.groundingAttributions) {
                        sources = groundingMetadata.groundingAttributions
                            .map(attribution => ({
                                uri: attribution.web?.uri,
                                title: attribution.web?.title,
                            }))
                            .filter(source => source.uri && source.title)
                            // Remove duplicatas baseadas no URI
                            .filter((source, index, self) => index === self.findIndex((t) => (t.uri === source.uri))); 
                    }

                    // 2. Adiciona as fontes de citação ao conteúdo final
                    if (sources.length > 0) {
                        htmlContent += '<br><br><div class="text-sm font-semibold mt-4 text-gray-700 border-t pt-2">Fontes de Pesquisa:</div>';
                        sources.forEach((source, index) => {
                            htmlContent += `<span class="citation"><a href="${source.uri}" target="_blank">${index + 1}. ${source.title}</a></span>`;
                        });
                    }

                    // 3. Exibe o resultado
                    searchResultsTextElement.innerHTML = htmlContent;
                    updateSearchUI(false, 'Pesquisa concluída e resumo gerado.');

                } else {
                    const blockReason = candidate?.finishReason;
                    console.error('Resposta da API sem texto. Objeto de resultado completo:', result);
                    
                    let errorMessage = 'ERRO: Falha ao pesquisar.';
                    if (blockReason === 'SAFETY') {
                        errorMessage = '⚠️ A pesquisa foi bloqueada pelo filtro de segurança. Tente um tópico diferente.';
                    }
                    
                    searchResultsTextElement.textContent = errorMessage;
                    updateSearchUI(false, 'Houve um erro.');
                }

            } catch (error) {
                searchResultsTextElement.textContent = error.message;
                updateSearchUI(false, 'Houve um erro grave na comunicação com a API.');
                console.error('Search API Error:', error);
            }
        };


        // Configuração inicial ao carregar a página
        window.onload = function() {
            // Seleciona a aba de cotações por padrão
            selectTab('quote');
            // Inicializa a UI
            updateQuoteUI(false, 'Pronto para gerar sua citação.');
            updateSearchUI(false, 'Pronto para pesquisar na web.');
            // Carrega e exibe o histórico ao iniciar
            displayHistory(loadHistory());
        };
    </script>
</body>
</html>
