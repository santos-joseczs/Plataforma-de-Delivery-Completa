<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Travessia do Rio - Desafio Lógico</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #1a2980, #26d0ce);
            color: white;
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        header {
            text-align: center;
            margin-bottom: 30px;
            padding: 20px;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }

        h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            color: #ffeb3b;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }

        .game-area {
            display: flex;
            justify-content: space-between;
            align-items: stretch;
            gap: 20px;
            margin-bottom: 30px;
        }

        .river {
            flex: 1;
            background: linear-gradient(to bottom, #1e90ff, #00bfff);
            border-radius: 10px;
            position: relative;
            min-height: 400px;
            display: flex;
            justify-content: center;
            align-items: center;
            box-shadow: inset 0 0 20px rgba(0, 0, 0, 0.3);
        }

        .river:before {
            content: "";
            position: absolute;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 4px;
            height: 100%;
            background: repeating-linear-gradient(
                to bottom,
                transparent,
                transparent 10px,
                white 10px,
                white 20px
            );
        }

        .bank {
            width: 45%;
            min-height: 400px;
            background: linear-gradient(to bottom, #8b4513, #654321);
            border-radius: 15px;
            padding: 20px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.4);
            position: relative;
            overflow: hidden;
        }

        .bank-left {
            border-right: 5px solid #5d2906;
        }

        .bank-right {
            border-left: 5px solid #5d2906;
        }

        .bank:before {
            content: "";
            position: absolute;
            top: 10px;
            left: 10px;
            right: 10px;
            bottom: 10px;
            border: 2px dashed rgba(255, 255, 255, 0.2);
            border-radius: 10px;
            pointer-events: none;
        }

        .bank h3 {
            text-align: center;
            margin-bottom: 20px;
            color: #ffcc80;
            font-size: 1.5em;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
        }

        .people-container {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            justify-content: center;
            min-height: 300px;
        }

        .person {
            width: 100px;
            height: 120px;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
        }

        .person:hover {
            transform: translateY(-5px);
        }

        .person.selected {
            animation: pulse 1s infinite;
            transform: scale(1.1);
        }

        @keyframes pulse {
            0% { box-shadow: 0 0 0 0 rgba(255, 235, 59, 0.7); }
            70% { box-shadow: 0 0 0 10px rgba(255, 235, 59, 0); }
            100% { box-shadow: 0 0 0 0 rgba(255, 235, 59, 0); }
        }

        .person-img {
            width: 80px;
            height: 80px;
            margin: 0 auto 10px;
            border-radius: 50%;
            background: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 40px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
        }

        .person-name {
            text-align: center;
            font-weight: bold;
            font-size: 0.9em;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.5);
            padding: 5px;
            background: rgba(0, 0, 0, 0.3);
            border-radius: 5px;
        }

        .controls {
            background: rgba(0, 0, 0, 0.3);
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 30px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }

        .boat {
            background: linear-gradient(to bottom, #8b4513, #a0522d);
            width: 150px;
            height: 60px;
            border-radius: 30px 30px 10px 10px;
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
            display: flex;
            justify-content: space-around;
            align-items: center;
            padding: 0 10px;
            transition: all 1s ease;
        }

        .boat-seat {
            width: 40px;
            height: 40px;
            background: rgba(139, 69, 19, 0.8);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
        }

        .controls-buttons {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 20px;
        }

        button {
            padding: 15px 30px;
            font-size: 1.1em;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: bold;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .cross-btn {
            background: linear-gradient(to right, #4CAF50, #45a049);
            color: white;
        }

        .cross-btn:hover:not(:disabled) {
            background: linear-gradient(to right, #45a049, #3d8b40);
            transform: scale(1.05);
        }

        .reset-btn {
            background: linear-gradient(to right, #f44336, #d32f2f);
            color: white;
        }

        .reset-btn:hover {
            background: linear-gradient(to right, #d32f2f, #b71c1c);
            transform: scale(1.05);
        }

        button:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none !important;
        }

        .rules {
            background: rgba(0, 0, 0, 0.3);
            padding: 25px;
            border-radius: 15px;
            margin-top: 30px;
        }

        .rules h3 {
            color: #ffcc80;
            margin-bottom: 15px;
            font-size: 1.5em;
        }

        .rules-list {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 15px;
        }

        .rule-item {
            background: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 10px;
            border-left: 4px solid #4CAF50;
        }

        .message {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            padding: 20px 40px;
            background: rgba(0, 0, 0, 0.9);
            color: white;
            border-radius: 10px;
            font-size: 1.2em;
            z-index: 1000;
            display: none;
            animation: slideIn 0.3s ease;
            border: 2px solid #4CAF50;
        }

        @keyframes slideIn {
            from { top: -100px; opacity: 0; }
            to { top: 20px; opacity: 1; }
        }

        .trip-counter {
            position: absolute;
            top: 20px;
            right: 20px;
            background: rgba(0, 0, 0, 0.5);
            padding: 10px 20px;
            border-radius: 20px;
            font-size: 1.2em;
            font-weight: bold;
        }

        .can-row {
            position: absolute;
            top: -25px;
            left: 50%;
            transform: translateX(-50%);
            background: #4CAF50;
            color: white;
            padding: 5px 15px;
            border-radius: 15px;
            font-size: 0.8em;
            white-space: nowrap;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>🚣 Travessia do Rio - Desafio Lógico</h1>
            <p>Ajude todas as 8 pessoas a atravessarem o rio seguindo as regras!</p>
        </header>

        <div class="trip-counter">Viagens: <span id="tripCount">0</span></div>

        <div class="game-area">
            <div class="bank bank-left" id="bankLeft">
                <h3>🌲 Margem Esquerda</h3>
                <div class="people-container" id="peopleLeft"></div>
            </div>

            <div class="river">
                <div class="boat" id="boat">
                    <div class="boat-seat" id="seat1"></div>
                    <div class="boat-seat" id="seat2"></div>
                </div>
            </div>

            <div class="bank bank-right" id="bankRight">
                <h3>🏔️ Margem Direita</h3>
                <div class="people-container" id="peopleRight"></div>
            </div>
        </div>

        <div class="controls">
            <div style="text-align: center; margin-bottom: 20px;">
                <h3>🎮 Controles</h3>
                <p id="statusMessage">Clique em até 2 pessoas para colocá-las no barco, depois clique em "Atravessar"</p>
            </div>
            <div class="controls-buttons">
                <button id="crossBtn" class="cross-btn" disabled>
                    <span>⛵</span> Atravessar Rio
                </button>
                <button id="resetBtn" class="reset-btn">
                    <span>🔄</span> Reiniciar Jogo
                </button>
            </div>
        </div>

        <div class="rules">
            <h3>📜 Regras do Jogo</h3>
            <div class="rules-list">
                <div class="rule-item">⛵ A jangada só pode carregar 2 pessoas por vez</div>
                <div class="rule-item">🚣 Somente Pai, Mãe e Policial sabem remar</div>
                <div class="rule-item">👦 Filhos não podem ficar com Mãe sem Pai presente</div>
                <div class="rule-item">👧 Filhas não podem ficar com Pai sem Mãe presente</div>
                <div class="rule-item">👮 Prisioneira não pode ficar com família sem Policial</div>
            </div>
        </div>
    </div>

    <div class="message" id="message"></div>

    <script>
        // Personagens do jogo com emojis e cores
        const people = [
            { id: 'pai', name: 'Pai', emoji: '👨', color: '#3498db', canRow: true },
            { id: 'mae', name: 'Mãe', emoji: '👩', color: '#e74c3c', canRow: true },
            { id: 'filho1', name: 'Filho 1', emoji: '👦', color: '#2ecc71', canRow: false },
            { id: 'filho2', name: 'Filho 2', emoji: '👦', color: '#27ae60', canRow: false },
            { id: 'filha1', name: 'Filha 1', emoji: '👧', color: '#9b59b6', canRow: false },
            { id: 'filha2', name: 'Filha 2', emoji: '👧', color: '#8e44ad', canRow: false },
            { id: 'policial', name: 'Policial', emoji: '👮', color: '#34495e', canRow: true },
            { id: 'prisioneira', name: 'Prisioneira', emoji: '👩', color: '#e67e22', canRow: false }
        ];

        let gameState = {
            leftBank: [...people.map(p => p.id)],
            rightBank: [],
            boat: [],
            boatPosition: 'left', // 'left' or 'right'
            selectedPeople: [],
            tripCount: 0,
            gameOver: false
        };

        // Elementos DOM
        const bankLeftEl = document.getElementById('peopleLeft');
        const bankRightEl = document.getElementById('peopleRight');
        const boatEl = document.getElementById('boat');
        const seat1El = document.getElementById('seat1');
        const seat2El = document.getElementById('seat2');
        const crossBtn = document.getElementById('crossBtn');
        const resetBtn = document.getElementById('resetBtn');
        const messageEl = document.getElementById('message');
        const tripCountEl = document.getElementById('tripCount');
        const statusMessageEl = document.getElementById('statusMessage');

        // Inicializar jogo
        function initGame() {
            renderPeople();
            updateBoatPosition();
            updateUI();
        }

        // Renderizar pessoas nas margens
        function renderPeople() {
            bankLeftEl.innerHTML = '';
            bankRightEl.innerHTML = '';

            // Margem esquerda
            gameState.leftBank.forEach(personId => {
                const person = people.find(p => p.id === personId);
                if (person) {
                    const personEl = createPersonElement(person);
                    bankLeftEl.appendChild(personEl);
                }
            });

            // Margem direita
            gameState.rightBank.forEach(personId => {
                const person = people.find(p => p.id === personId);
                if (person) {
                    const personEl = createPersonElement(person);
                    bankRightEl.appendChild(personEl);
                }
            });
        }

        // Criar elemento de pessoa
        function createPersonElement(person) {
            const div = document.createElement('div');
            div.className = 'person';
            if (gameState.selectedPeople.includes(person.id)) {
                div.classList.add('selected');
            }
            div.dataset.id = person.id;

            const isInBoat = gameState.boat.includes(person.id);
            if (isInBoat) {
                div.style.opacity = '0.5';
                div.style.cursor = 'default';
            }

            div.innerHTML = `
                <div class="person-img" style="background: ${person.color}">
                    ${person.emoji}
                </div>
                <div class="person-name">${person.name}</div>
                ${person.canRow ? '<div class="can-row">Pode remar</div>' : ''}
            `;

            if (!isInBoat) {
                div.onclick = () => selectPerson(person.id);
            }

            return div;
        }

        // Selecionar/deselecionar pessoa
        function selectPerson(personId) {
            if (gameState.gameOver) return;

            const person = people.find(p => p.id === personId);
            const currentBank = gameState.boatPosition === 'left' ? gameState.leftBank : gameState.rightBank;
            
            // Verificar se a pessoa está na margem atual
            if (!currentBank.includes(personId)) {
                showMessage('Esta pessoa não está nesta margem!');
                return;
            }

            // Verificar se já está no barco
            if (gameState.boat.includes(personId)) {
                showMessage('Esta pessoa já está no barco!');
                return;
            }

            // Verificar se pode ser selecionada (máximo 2 no barco)
            if (gameState.selectedPeople.length >= 2 && !gameState.selectedPeople.includes(personId)) {
                showMessage('O barco só pode levar 2 pessoas!');
                return;
            }

            const index = gameState.selectedPeople.indexOf(personId);
            if (index > -1) {
                // Deselecionar
                gameState.selectedPeople.splice(index, 1);
            } else {
                // Selecionar
                gameState.selectedPeople.push(personId);
            }

            renderPeople();
            updateUI();
        }

        // Atualizar interface
        function updateUI() {
            // Atualizar barco
            seat1El.innerHTML = gameState.boat[0] ? people.find(p => p.id === gameState.boat[0])?.emoji : '';
            seat2El.innerHTML = gameState.boat[1] ? people.find(p => p.id === gameState.boat[1])?.emoji : '';

            // Atualizar botão atravessar
            const canCross = gameState.selectedPeople.length > 0 && 
                            gameState.selectedPeople.length <= 2 &&
                            hasRower(gameState.selectedPeople);
            
            crossBtn.disabled = !canCross;
            
            // Atualizar mensagem de status
            if (gameState.selectedPeople.length === 0) {
                statusMessageEl.textContent = 'Clique em até 2 pessoas para colocá-las no barco';
            } else if (!hasRower(gameState.selectedPeople)) {
                statusMessageEl.textContent = 'O barco precisa de alguém que saiba remar!';
            } else if (gameState.selectedPeople.length === 1) {
                statusMessageEl.textContent = 'Pronto para atravessar! Clique em mais uma pessoa ou atravesse assim';
            } else {
                statusMessageEl.textContent = 'Pronto para atravessar!';
            }

            // Atualizar contador
            tripCountEl.textContent = gameState.tripCount;

            // Verificar vitória
            checkWin();
        }

        // Verificar se há remador
        function hasRower(personIds) {
            return personIds.some(id => {
                const person = people.find(p => p.id === id);
                return person.canRow;
            });
        }

        // Atravessar rio
        function crossRiver() {
            if (gameState.gameOver || gameState.selectedPeople.length === 0) return;

            // Verificar regras do barco
            if (!hasRower(gameState.selectedPeople)) {
                showMessage('❌ O barco precisa de alguém que saiba remar!');
                return;
            }

            // Mover pessoas selecionadas para o barco
            gameState.boat = [...gameState.selectedPeople];
            
            // Remover pessoas da margem atual
            const currentBank = gameState.boatPosition === 'left' ? gameState.leftBank : gameState.rightBank;
            const newBank = currentBank.filter(id => !gameState.boat.includes(id));
            
            if (gameState.boatPosition === 'left') {
                gameState.leftBank = newBank;
            } else {
                gameState.rightBank = newBank;
            }

            // Atualizar posição do barco
            gameState.boatPosition = gameState.boatPosition === 'left' ? 'right' : 'left';
            
            // Adicionar pessoas à nova margem
            if (gameState.boatPosition === 'left') {
                gameState.leftBank.push(...gameState.boat);
            } else {
                gameState.rightBank.push(...gameState.boat);
            }

            // Limpar barco e seleção
            gameState.boat = [];
            gameState.selectedPeople = [];
            
            // Incrementar contador
            gameState.tripCount++;
            
            // Verificar regras
            if (!checkRules()) {
                // Violou regras - voltar estado anterior
                showMessage('❌ Regra violada! O jogo será reiniciado.');
                setTimeout(resetGame, 2000);
                return;
            }

            // Atualizar UI
            updateBoatPosition();
            renderPeople();
            updateUI();
            
            showMessage('✅ Travessia realizada com sucesso!');
        }

        // Verificar regras
        function checkRules() {
            const checkBank = (bank) => {
                const bankPeople = bank.map(id => people.find(p => p.id === id));
                
                // Verificar filhos com mãe sem pai
                const hasMother = bankPeople.some(p => p.id === 'mae');
                const hasFather = bankPeople.some(p => p.id === 'pai');
                const hasSon = bankPeople.some(p => p.id === 'filho1' || p.id === 'filho2');
                
                if (hasMother && hasSon && !hasFather) {
                    showMessage('❌ Filhos não podem ficar com mãe sem pai presente!');
                    return false;
                }

                // Verificar filhas com pai sem mãe
                const hasDaughter = bankPeople.some(p => p.id === 'filha1' || p.id === 'filha2');
                
                if (hasFather && hasDaughter && !hasMother) {
                    showMessage('❌ Filhas não podem ficar com pai sem mãe presente!');
                    return false;
                }

                // Verificar prisioneira com família sem policial
                const hasPrisoner = bankPeople.some(p => p.id === 'prisioneira');
                const hasPolicial = bankPeople.some(p => p.id === 'policial');
                const hasFamily = bankPeople.some(p => 
                    p.id === 'pai' || p.id === 'mae' || 
                    p.id === 'filho1' || p.id === 'filho2' ||
                    p.id === 'filha1' || p.id === 'filha2'
                );
                
                if (hasPrisoner && hasFamily && !hasPolicial) {
                    showMessage('❌ Prisioneira não pode ficar com família sem policial!');
                    return false;
                }

                return true;
            };

            return checkBank(gameState.leftBank) && checkBank(gameState.rightBank);
        }

        // Atualizar posição visual do barco
        function updateBoatPosition() {
            if (gameState.boatPosition === 'left') {
                boatEl.style.left = '25%';
            } else {
                boatEl.style.left = '75%';
            }
        }

        // Verificar vitória
        function checkWin() {
            if (gameState.rightBank.length === 8 && !gameState.gameOver) {
                gameState.gameOver = true;
                showMessage('🎉 Parabéns! Você completou o desafio em ' + gameState.tripCount + ' viagens!');
                crossBtn.disabled = true;
            }
        }

        // Mostrar mensagem
        function showMessage(text) {
            messageEl.textContent = text;
            messageEl.style.display = 'block';
            setTimeout(() => {
                messageEl.style.display = 'none';
            }, 3000);
        }

        // Reiniciar jogo
        function resetGame() {
            gameState = {
                leftBank: [...people.map(p => p.id)],
                rightBank: [],
                boat: [],
                boatPosition: 'left',
                selectedPeople: [],
                tripCount: 0,
                gameOver: false
            };
            initGame();
            showMessage('🔄 Jogo reiniciado!');
        }

        // Event listeners
        crossBtn.onclick = crossRiver;
        resetBtn.onclick = resetGame;

        // Inicializar
        initGame();
    </script>
</body>
</html>
