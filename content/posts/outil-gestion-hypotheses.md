---
title: "Outil pour nuancer nos opinions"
date: 2024-12-21T20:52:26-05:00
description: 'Un petit outil créé avec ChatGPT'
image:
draft: false
---

# Évaluer nos pistes de réflexion 

Une même question peut avoir plusieurs réponses potentielles. Par exemple, observer un objet que l'on ne reconnaît pas dans le ciel : cela pourrait être un avion, un oiseau ou Superman.

Cependant, chacune de ces hypothèses n’a pas nécessairement le même poids selon notre expérience, nos préjugés ou le contexte.

C’est exactement pour ça que j’ai créé cet **Outil de Gestion des Hypothèses** – avec un coup de main de ChatGPT. Ok... C'est pas mal juste ses mains qui ont servi.

## Pourquoi j’ai fait ça ? (Et pourquoi ça peut vous plaire)

Parfois, on est tellement convaincu d’une idée qu’on ne prend pas le temps de mettre les autres hypothèses à côté pour les comparer. 

Résultat ? Dans notre tête, les différents scénarios se mélangent, et c'est difficile d'évaluer lesquels sont les plus plausibles selon nous.

C'est ce qui donne des opinions particulièrement tranchées, parce qu'il devient difficile de nuancer. 

Avec cet outil, l’idée est simple : **rassembler vos hypothèses, leur donner un niveau d'importance et les faire entrer en relation entre elles**. Ça donne des trucs surprenants !

## Comment ça marche ?

1. **Posez votre question centrale** : Vous vous demandez "Pourquoi les chats dorment autant ?" Écrivez la question dans le champ prévu pour ça.

2. **Ajoutez vos hypothèses** : Pour chaque hypothèse, attribuez un pourcentage (genre "70% pour parce qu’ils sont paresseux", "20% pour économiser de l’énergie", etc.).

3. **Admirez votre tableau** : Chaque hypothèse a sa couleur et sa petite ligne qui la relie à une jolie barre verticale, qui permet de voir l'importance de chaque hypothèse face aux autres.

4. **Ajustez, jouez, testez** : Vous changez un pourcentage, vous enlevez une hypothèse, et hop, tout se réorganise en temps réel. C’est fluide, c’est l'fun.

5. **Je ne garde aucune trace de ce que vous faites** : L'outil fonctionne localement sur la page Web et n'envoie aucune donnée nulle part.

## Alors, c’est quoi la grande question que vous allez explorer aujourd’hui ? 



<div id="hypothesis-tool">
    <h2 id="question-subtitle" class="hidden"></h2> 
    
 <div id="question-form">
        <input type="text" id="question-input" placeholder="Entrez une question">
        <button id="set-question-button">Définir la Question</button>
    </div>
    
 <div id="hypothesis-form">
        <input type="text" id="hypothesis-input" placeholder="Entrez une hypothèse">
        <input type="number" id="percentage-input" placeholder="Pourcentage" min="0" max="100">
        <button id="add-button">Ajouter</button>
    </div>
    
  <div id="hypotheses-container">
    <svg id="connections-svg"></svg>
   <div id="hypotheses-list"></div>
        <div id="percentage-bar-vertical">
            <div id="bar-segments"></div>
        </div>
    </div>
</div>


<style>
    #hypothesis-tool {
        max-width: 800px;
        margin: 0 auto; /* Centrer l'outil sur la page */
        padding: 20px;
        background-color: #ffffff;
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
        position: relative; /* Pour positionner le SVG correctement */
    }

    #question-form,
    #hypothesis-form {
        display: flex;
        gap: 10px;
        margin-bottom: 20px;
        justify-content: center;
        flex-wrap: wrap; /* Permettre le retour à la ligne sur petits écrans */
    }

    #question-form input[type="text"],
    #hypothesis-form input[type="text"],
    #hypothesis-form input[type="number"] {
        padding: 8px;
        border: 1px solid #ccc;
        border-radius: 4px;
    }

    #question-form input[type="text"],
    #hypothesis-form input[type="text"] {
        flex: 1 1 300px; /* Flex-grow, Flex-shrink, Flex-basis */
        min-width: 200px;
    }

    #hypothesis-form input[type="number"] {
        width: 100px;
    }

    #set-question-button,
    #add-button {
        padding: 8px 16px;
        background-color: #3498db;
        color: white;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        transition: background-color 0.3s;
        flex: 0 0 auto;
    }

    #set-question-button:hover,
    #add-button:hover {
        background-color: #2980b9;
    }

    #hypotheses-container {
        display: flex;
        gap: 20px;
        justify-content: space-between;
        align-items: flex-end;
        flex-wrap: wrap; /* Permettre le retour à la ligne sur petits écrans */
        position: relative; /* Pour positionner le SVG absolument */
    }

    #connections-svg {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        pointer-events: none; /* Permettre les interactions avec les éléments sous le SVG */
        z-index: 1; /* Positionner derrière les éléments */
    }

    #hypotheses-list {
        flex: 1 1 400px;
        max-width: 600px;
        display: flex;
        flex-direction: column-reverse; /* Inverser l'ordre pour aligner avec la barre */
        position: relative;
        z-index: 2; /* Positionner au-dessus du SVG */
    }

    .hypothesis-item {
        display: flex;
        align-items: center;
        margin-bottom: 10px;
        padding: 10px;
        border: 1px solid #ddd;
        border-left: 10px solid #000;
        border-radius: 5px;
        background-color: #fff;
        transition: background-color 0.3s;
        animation: fadeIn 0.5s ease-in-out;
        position: relative;
        z-index: 2; /* Positionner au-dessus du SVG */
    }

    .hypothesis-item:hover {
        background-color: #f9f9f9;
    }

    .hypothesis-color {
        width: 15px;
        height: 15px;
        border-radius: 50%;
        margin-right: 10px;
    }

    .hypothesis-details {
        flex: 1;
        display: flex;
        align-items: center;
    }

    .hypothesis-details span {
        margin-right: 10px;
        flex: 1;
    }

    .hypothesis-details input[type="number"] {
        width: 80px;
        padding: 5px;
        border: 1px solid #ccc;
        border-radius: 4px;
        margin-right: 10px;
    }

    .delete-button {
        background-color: #e74c3c;
        color: white;
        border: none;
        padding: 5px 10px;
        border-radius: 3px;
        cursor: pointer;
        transition: background-color 0.3s;
    }

    .delete-button:hover {
        background-color: #c0392b;
    }

    #percentage-bar-vertical {
        width: 60px;
        height: 300px;
        border: 1px solid #000;
        display: flex;
        flex-direction: column-reverse; /* Pour que le plus grand soit en bas */
        overflow: hidden;
        position: relative;
        background-color: #f0f0f0;
        border-radius: 5px;
        transition: all 0.5s ease-in-out;
        z-index: 2; /* Positionner au-dessus du SVG */
    }

    #bar-segments {
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: column-reverse; /* Assurer que le premier segment est en bas */
        transition: all 0.5s ease-in-out;
    }

    .segment {
        width: 100%;
        transition: height 0.5s ease-in-out, background-color 0.5s ease-in-out;
        animation: fadeIn 0.5s ease-in-out;
        position: relative;
    }

    .segment::after {
        content: attr(title);
        position: absolute;
        left: 50%;
        bottom: 0;
        transform: translateX(-50%);
        background: rgba(0, 0, 0, 0.7);
        color: #fff;
        padding: 2px 5px;
        border-radius: 3px;
        font-size: 10px;
        opacity: 0;
        transition: opacity 0.3s;
    }

    .segment:hover::after {
        opacity: 1;
    }

    @keyframes fadeIn {
        from { opacity: 0; transform: translateY(-10px); }
        to { opacity: 1; transform: translateY(0); }
    }

    /* Media Queries pour Responsivité */
    @media (max-width: 768px) {
        #hypotheses-container {
            flex-direction: column;
            align-items: center;
        }

        #percentage-bar-vertical {
            width: 80%;
            height: 200px;
        }

        #hypotheses-list {
            max-width: 100%;
        }
    }
</style>

<!-- Scripts JavaScript spécifiques à l'outil -->
<script>
    // ---------------------------------------------
    // Gestion des hypothèses
    // ---------------------------------------------
    let hypotheses = [];
    const colors = ['#FF5733', '#33FF57', '#3357FF', '#F333FF', '#FF33A8', '#33FFF5'];

    // Éléments du DOM
    const addButton = document.getElementById('add-button');
    const setQuestionButton = document.getElementById('set-question-button');
    const questionInput = document.getElementById('question-input');
    const questionSubtitle = document.getElementById('question-subtitle');
    const connectionsSvg = document.getElementById('connections-svg');

    addButton.addEventListener('click', addHypothesis);
    setQuestionButton.addEventListener('click', setQuestion);

    // Charger les hypothèses sauvegardées et la question au démarrage
    window.onload = loadHypotheses;

    // ---------------------------------------------
    // Fonctions principales
    // ---------------------------------------------

    function addHypothesis() {
        const hypothesisInput = document.getElementById('hypothesis-input');
        const percentageInput = document.getElementById('percentage-input');
        const hypothesis = hypothesisInput.value.trim();
        let percentage = parseFloat(percentageInput.value);

        if (!hypothesis) {
            alert("Veuillez entrer une hypothèse valide.");
            return;
        }

        if (hypotheses.length === 0) {
            // Première hypothèse, elle occupe automatiquement 100%
            percentage = 100;
            hypotheses.push({ hypothesis, percentage, color: colors[hypotheses.length % colors.length] });
        } else {
            if (isNaN(percentage) || percentage < 0 || percentage > 100) {
                alert("Veuillez entrer un pourcentage valide entre 0 et 100.");
                return;
            }

            // Vérifier si l'ajout dépasse 100%
            const desiredTotal = getTotalPercentage() + percentage;
            if (desiredTotal > 100) {
                const excess = desiredTotal - 100;
                // Réduire toutes les hypothèses existantes proportionnellement
                const adjustableHypotheses = hypotheses.filter(h => h.percentage > 0);
                const totalAdjustable = adjustableHypotheses.reduce((sum, h) => sum + h.percentage, 0);
                if (totalAdjustable === 0) {
                    alert("Impossible d'ajouter cette hypothèse car il n'y a pas d'hypothèses ajustables.");
                    return;
                }
                adjustableHypotheses.forEach(h => {
                    h.percentage -= (h.percentage / totalAdjustable) * excess;
                    h.percentage = Math.max(0, h.percentage);
                });
            }

            // Ajouter la nouvelle hypothèse
            const color = colors[hypotheses.length % colors.length];
            hypotheses.push({ hypothesis, percentage, color });
        }

        adjustPercentages();
        sortHypotheses();
        renderHypotheses();
        renderBar();
        drawConnections();
        saveHypotheses();

        hypothesisInput.value = '';
        percentageInput.value = '';
    }

    function setQuestion() {
        // Si on appuie sur le bouton alors qu'on a déjà une question, on demande de confirmer
        if (setQuestionButton.textContent === "Définir une nouvelle question") {
            const confirmReset = confirm("Voulez-vous réinitialiser toutes les hypothèses et définir une nouvelle question ?");
            if (confirmReset) {
                clearAllHypotheses();
            } else {
                return; // Annuler l'action si l'utilisateur ne souhaite pas réinitialiser
            }
        }
        defineNewQuestion();
    }

    function defineNewQuestion() {
        const question = questionInput.value.trim();
        if (!question) {
            alert("Veuillez entrer une question valide.");
            return;
        }
        setQuestionDisplay(question);
        saveQuestion(question);
        setQuestionButton.textContent = "Définir une nouvelle question"; 
        questionInput.value = '';
    }

    // Vider toutes les hypothèses (et localStorage) 
    function clearAllHypotheses() {
        hypotheses = [];
        localStorage.removeItem('hypotheses');
        renderHypotheses();
        renderBar();
        drawConnections();
    }

    // ---------------------------------------------
    // Fonctions utilitaires
    // ---------------------------------------------
    function adjustPercentages() {
        // Assurer que le total des pourcentages est toujours à 100%
        let total = getTotalPercentage();
        if (total !== 100 && hypotheses.length > 0) {
            const difference = 100 - total;
            // Répartir la différence proportionnellement parmi toutes les hypothèses
            const adjustableHypotheses = hypotheses.filter(h => h.percentage > 0);
            const totalAdjustable = adjustableHypotheses.reduce((sum, h) => sum + h.percentage, 0);
            if (totalAdjustable > 0) {
                adjustableHypotheses.forEach(h => {
                    h.percentage += (h.percentage / totalAdjustable) * difference;
                    h.percentage = Math.max(0, h.percentage);
                });
            }
        }
    }

    function sortHypotheses() {
        // Trier les hypothèses en ordre décroissant de pourcentage
        hypotheses.sort((a, b) => b.percentage - a.percentage);
    }

    function getTotalPercentage() {
        return hypotheses.reduce((sum, h) => sum + h.percentage, 0);
    }

    function renderHypotheses() {
        const list = document.getElementById('hypotheses-list');
        list.innerHTML = '';

        // Les hypothèses sont déjà triées en ordre décroissant
        hypotheses.forEach((h, index) => {
            const item = document.createElement('div');
            item.className = 'hypothesis-item';
            item.style.borderLeftColor = h.color;

            const colorIndicator = document.createElement('div');
            colorIndicator.className = 'hypothesis-color';
            colorIndicator.style.backgroundColor = h.color;

            const details = document.createElement('div');
            details.className = 'hypothesis-details';

            const label = document.createElement('span');
            label.textContent = h.hypothesis;

            const input = document.createElement('input');
            input.type = 'number';
            input.value = h.percentage.toFixed(2);
            input.min = 0;
            input.max = 100;
            input.step = 0.01;
            // Désactiver la modification si c'est la seule hypothèse
            input.disabled = (hypotheses.length === 1); 
            input.title = (hypotheses.length === 1) 
                ? "La première hypothèse est toujours à 100%" 
                : "Modifier le pourcentage";

            input.addEventListener('change', (e) => {
                if (hypotheses.length === 1) return; // Ne pas modifier si c'est la seule
                updatePercentage(index, parseFloat(e.target.value));
            });

            const deleteBtn = document.createElement('button');
            deleteBtn.className = 'delete-button';
            deleteBtn.textContent = 'Supprimer';
            deleteBtn.addEventListener('click', () => deleteHypothesis(index));

            details.appendChild(label);
            details.appendChild(input);
            details.appendChild(deleteBtn);

            item.appendChild(colorIndicator);
            item.appendChild(details);
            list.appendChild(item);
        });
    }

    function updatePercentage(index, newPercentage) {
        if (isNaN(newPercentage) || newPercentage < 0 || newPercentage > 100) {
            alert("Veuillez entrer un pourcentage valide entre 0 et 100.");
            renderHypotheses();
            return;
        }

        hypotheses[index].percentage = newPercentage;

        adjustPercentages();
        sortHypotheses();
        renderHypotheses();
        renderBar();
        drawConnections();
        saveHypotheses();
    }

    function deleteHypothesis(index) {
        // Permettre la suppression de la première hypothèse seulement si c'est la seule
        if (index === 0 && hypotheses.length > 1) {
            alert("La première hypothèse ne peut pas être supprimée tant qu'il y a d'autres hypothèses.");
            return;
        }

        hypotheses.splice(index, 1);
        adjustPercentages();
        sortHypotheses();
        renderHypotheses();
        renderBar();
        drawConnections();
        saveHypotheses();
    }

    function renderBar() {
        const bar = document.getElementById('bar-segments');
        bar.innerHTML = '';

        // Les hypothèses sont déjà triées en ordre décroissant
        hypotheses.forEach(h => {
            const segment = document.createElement('div');
            segment.className = 'segment';
            segment.style.height = `${h.percentage}%`;
            segment.style.backgroundColor = h.color;
            segment.title = `${h.hypothesis}: ${h.percentage.toFixed(2)}%`;
            bar.appendChild(segment);
        });
    }

    function drawConnections() {
        // Effacer les connexions existantes
        connectionsSvg.innerHTML = '';

        const container = document.getElementById('hypotheses-container');
        const containerRect = container.getBoundingClientRect();

        const list = document.getElementById('hypotheses-list');
        const listItems = list.getElementsByClassName('hypothesis-item');

        const bar = document.getElementById('percentage-bar-vertical');
        const segments = bar.getElementsByClassName('segment');

        hypotheses.forEach((h, index) => {
            const item = listItems[index];
            const segment = segments[index];
            if (item && segment) {
                const itemRect = item.getBoundingClientRect();
                const segmentRect = segment.getBoundingClientRect();

                // Calculer la position relative au conteneur
                const x1 = itemRect.right - containerRect.left;
                const y1 = itemRect.top - containerRect.top + (itemRect.height / 2);
                const x2 = segmentRect.left - containerRect.left + (segmentRect.width / 2);
                const y2 = segmentRect.top - containerRect.top + (segmentRect.height / 2);

                // Créer une ligne SVG avec la couleur de l'hypothèse
                const line = document.createElementNS("http://www.w3.org/2000/svg", "line");
                line.setAttribute('x1', x1);
                line.setAttribute('y1', y1);
                line.setAttribute('x2', x2);
                line.setAttribute('y2', y2);
                line.setAttribute('stroke', h.color); // Couleur de l'hypothèse
                line.setAttribute('stroke-width', '1');

                connectionsSvg.appendChild(line);
            }
        });
    }

    // ---------------------------------------------
    // Gestion du LocalStorage
    // ---------------------------------------------

    function saveHypotheses() {
        localStorage.setItem('hypotheses', JSON.stringify(hypotheses));
    }

    function loadHypotheses() {
        const stored = localStorage.getItem('hypotheses');
        if (stored) {
            hypotheses = JSON.parse(stored);
            adjustPercentages();
            sortHypotheses();
            renderHypotheses();
            renderBar();
            drawConnections();
        }

        // Charger la question si elle existe
        const storedQuestion = localStorage.getItem('question');
        if (storedQuestion) {
            setQuestionDisplay(storedQuestion);
            setQuestionButton.textContent = "Définir une nouvelle question";
        }
    }

    function setQuestionDisplay(question) {
        questionSubtitle.textContent = question;
        questionSubtitle.classList.remove('hidden');
    }

    function saveQuestion(question) {
        localStorage.setItem('question', question);
    }
</script>
