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

2. **Ajoutez vos hypothèses** : Pour chaque hypothèse, attribuez un pourcentage (genre 70% pour "parce qu’ils sont paresseux", 20% à l'hypothèse que c'est "pour économiser de l’énergie", etc.).

3. **Admirez votre tableau** : Chaque hypothèse a sa couleur et sa petite ligne qui la relie à une jolie barre verticale, qui permet de voir l'importance de chaque hypothèse face aux autres.

4. **Ajustez, jouez, testez** : Parfois, on se rend compte qu'une hypothèse a trop d'importance quand on la voit à côté d'autres plus crédibles. C'est là que vous pouvez modifier un pourcentage ou enlever une hypothèse, et hop, tout se réorganise en temps réel.

5. **Verouiller un pourcentage** : Vous avez une ou des hypothèses que vous voulez les voir conserver un certain pourcentage, vous pouvez verouiller celui-ci. Les autres hypothèses s'ajusteront, mais celles verrouillées ne bougeront pas. Vous pouvez verrouiller plusieurs hypothèses, mais le total ne doit pas dépasser 100% (exemple: A-80%, B-45%... Vous verrez un message d'erreur demandant de modifier ces pointages). 

5. **Je ne garde aucune trace de ce que vous faites** : L'outil fonctionne localement sur la page Web et n'envoie aucune donnée nulle part.

On s'entend, c'est littéralement construit avec de la colle en bâton et du *duck tape*. Donc, soyez indulgents. Surtout, amusez-vous!


## Alors, c’est quoi la grande question que vous allez explorer aujourd’hui ? 

<style>
  /**********************************************/
  /*                STYLE GÉNÉRAL               */
  /**********************************************/

  #hypothesis-tool {
    max-width: 900px;
    margin: 2rem auto;
    padding: 1rem;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    position: relative;
  }

  #hypothesis-tool h2,
  #hypothesis-tool h3 {
    text-align: center;
  }

  .row {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
    justify-content: center;
    margin-bottom: 1rem;
  }

  .row input[type="text"],
  .row input[type="number"] {
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  .btn {
    padding: 8px 16px;
    background-color: #3498db;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s;
  }

  .btn:hover {
    background-color: #2980b9;
  }

  #question-subtitle {
    margin-bottom: 2rem;
    font-size: 1.2rem;
  }

  /**********************************************/
  /*              LISTE D’HYPOTHÈSES            */
  /**********************************************/

  #hypotheses-container {
    display: flex;
    gap: 20px;
    justify-content: space-between;
    align-items: flex-start;
    flex-wrap: wrap;
    position: relative;
    margin-top: 1rem;
  }

  #hypotheses-list {
    flex: 1 1 400px;
    max-width: 600px;
    display: flex;
    flex-direction: column; /* Ordre décroissant du haut vers le bas */
    position: relative;
    z-index: 2;
  }

  .hypothesis-item {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
    padding: 10px;
    border: 1px solid #ddd;
    border-left: 10px solid #000; /* Couleur dynamique */
    border-radius: 5px;
    background-color: #fff;
    transition: background-color 0.3s;
    animation: fadeIn 0.5s ease-in-out;
    position: relative;
  }

  .hypothesis-item:hover {
    background-color: #f9f9f9;
  }

  .hypothesis-details {
    flex: 1;
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    align-items: center;
    margin-left: 10px;
  }

  .hypothesis-label {
    flex: 1 1 auto;
    font-weight: bold;
  }

  /* Suppression du style pour le slider */
  /* .slider-percentage {
    width: 100px; 
  } */

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

  .lock-checkbox {
    margin-left: 10px;
  }

  /**********************************************/
  /*         BARRE DE POURCENTAGE VERTICALE     */
  /**********************************************/

  #percentage-bar-vertical {
    width: 60px;
    height: 300px;
    border: 1px solid #000;
    display: flex;
    flex-direction: column; /* Ordre décroissant */
    overflow: hidden;
    position: relative;
    background-color: #f0f0f0;
    border-radius: 5px;
    transition: all 0.5s ease-in-out;
    z-index: 2;
  }

  #bar-segments {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column; /* Ordre décroissant */
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

  /**********************************************/
  /*                  SVG LINES                 */
  /**********************************************/

  #connections-svg {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 1;
  }

  /**********************************************/
  /*                  RESPONSIVE                */
  /**********************************************/

  @media (max-width: 768px) {
    #hypotheses-container {
      flex-direction: column;
      align-items: center;
    }
    #percentage-bar-vertical {
      width: 80%;
      height: 200px;
      margin-top: 1rem;
    }
    #hypotheses-list {
      max-width: 100%;
    }
  }
</style>

<div id="hypothesis-tool">
  <h2>Outil de Gestion des Hypothèses</h2>

  <!-- Titre de la question (changé dynamiquement) -->
  <h3 id="question-subtitle" class="hidden"></h3>

  <!-- FORMULAIRE POUR LA QUESTION -->
  <div class="row">
    <input type="text" id="question-input" placeholder="Entrez une question" style="flex:1 1 300px;">
    <button id="set-question-button" class="btn">Définir la Question</button>
  </div>

  <!-- FORMULAIRE POUR AJOUTER UNE HYPOTHÈSE -->
  <div class="row">
    <input type="text" id="hypothesis-input" placeholder="Entrez une hypothèse" style="flex:2 1 300px;">
    <input type="number" id="percentage-input" placeholder="Pourcentage (optionnel)" min="0" max="100" step="1" style="width:130px;">

<!-- Sélection d'une couleur + bouton "Aléatoire" -->
<label for="color-input">Couleur :</label>
<input type="color" id="color-input" value="#FF5733" title="Choisir une couleur" />
<button id="random-color-button" class="btn" title="Couleur aléatoire">🎨</button>

<button id="add-button" class="btn">Ajouter</button>
  </div>

  <!-- Conteneur principal : liste + barre + SVG -->
  <div id="hypotheses-container">
    <svg id="connections-svg"></svg>
    <div id="hypotheses-list"></div>
    <div id="percentage-bar-vertical">
      <div id="bar-segments"></div>
    </div>
  </div>
</div>

<script>
  /**************************************************/
  /*               ÉTAT GLOBAL & CONFIG             */
  /**************************************************/

  let hypotheses = [];  // Tableau d'objets : { hypothesis, percentage, color, locked }
  const defaultColors = [
    '#FF5733','#33FF57','#3357FF','#F333FF','#FF33A8',
    '#33FFF5','#FFC300','#DAF7A6'
  ];

  // Récupération des éléments du DOM
  const questionInput = document.getElementById('question-input');
  const questionSubtitle = document.getElementById('question-subtitle');
  const setQuestionButton = document.getElementById('set-question-button');

  const hypothesisInput = document.getElementById('hypothesis-input');
  const percentageInput = document.getElementById('percentage-input');
  const colorInput = document.getElementById('color-input');
  const randomColorButton = document.getElementById('random-color-button');
  const addButton = document.getElementById('add-button');

  const connectionsSvg = document.getElementById('connections-svg');

  /**************************************************/
  /*                   INIT                         */
  /**************************************************/
  window.addEventListener('DOMContentLoaded', () => {
    loadStateFromLocalStorage();
    renderAll();
  });

  /**************************************************/
  /*                  ÉVÉNEMENTS                    */
  /**************************************************/
  setQuestionButton.addEventListener('click', handleQuestion);
  addButton.addEventListener('click', handleAddHypothesis);
  randomColorButton.addEventListener('click', handleRandomColor);

  /**************************************************/
  /*               FONCTIONS HANDLERS              */
  /**************************************************/

  function handleQuestion() {
    if (setQuestionButton.textContent === "Définir une nouvelle question") {
      const confirmReset = confirm(
        "Voulez-vous réinitialiser toutes les hypothèses et définir une nouvelle question ?"
      );
      if (confirmReset) {
        clearAllHypotheses();
      } else {
        return;
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
    questionSubtitle.textContent = question;
    questionSubtitle.classList.remove('hidden');
    localStorage.setItem('question', question);

    setQuestionButton.textContent = "Définir une nouvelle question";
    questionInput.value = '';
  }

  function handleRandomColor() {
    const randomIndex = Math.floor(Math.random() * defaultColors.length);
    colorInput.value = defaultColors[randomIndex];
  }

  /**
   * Ajoute une nouvelle hypothèse
   * - Si l'utilisateur fournit un pourcentage p > 0, 
   *   on réserve p% pour la nouvelle hypothèse, et on rééchelonne les existantes pour totaliser 100%.
   * - Si l'utilisateur ne fournit aucun pourcentage (ou 0),
   *   on répartit équitablement 100% sur toutes les hypothèses (anciennes + nouvelle).
   */
  function handleAddHypothesis() {
    const hypothesisText = hypothesisInput.value.trim();
    let rawPct = percentageInput.value.trim();
    let chosenColor = colorInput.value.trim();

    if (!hypothesisText) {
      alert("Veuillez entrer une hypothèse valide.");
      return;
    }

    // Couleur : si vide, on prend une couleur aléatoire
    if (!chosenColor) {
      const randomIndex = Math.floor(Math.random() * defaultColors.length);
      chosenColor = defaultColors[randomIndex];
    }

    // Crée l'objet avec 'locked' initialisé à false
    const newHypothesis = {
      hypothesis: hypothesisText,
      percentage: 0,
      color: chosenColor,
      locked: false
    };

    // Ajoute au tableau
    hypotheses.push(newHypothesis);

    // Gère la répartition
    if (rawPct === "") {
      // Pas de pourcentage fourni => distribution équitable
      distributeEqually();
    } else {
      // L'utilisateur veut un certain pourcentage
      let p = parseInt(rawPct, 10);
      if (isNaN(p) || p < 0 || p > 100) {
        alert("Veuillez entrer un pourcentage valide entre 0 et 100 (ou laissez vide).");
        // On retire l'hypothèse qu'on vient d'ajouter
        hypotheses.pop();
        return;
      }
      if (p === 0) {
        // Si l'utilisateur a explicitement saisi 0
        distributeEqually();
      } else {
        // L'utilisateur a un pourcentage > 0
        // 1. On réserve p% pour la nouvelle hypothèse
        newHypothesis.percentage = p;

        // 2. Vérifier que les hypothèses verrouillées ne dépassent pas 100%
        const totalLocked = getTotalLockedPercentage();
        if (totalLocked + p > 100) {
          alert("Le total des pourcentages verrouillés dépasse 100%. Veuillez ajuster les pourcentages.");
          hypotheses.pop();
          return;
        }

        // 3. On rééchelonne les hypothèses non verrouillées pour qu'elles se partagent (100 - p)
        redistributeAfterAddition(p);
      }
    }

    saveStateToLocalStorage();
    renderAll();

    // Reset champs
    hypothesisInput.value = '';
    percentageInput.value = '';
  }

  /**************************************************/
  /*        FONCTIONS : RÉPARTITION / AJUSTEMENT    */
  /**************************************************/

  /**
   * Distribution équitable de 100% sur toutes les hypothèses non verrouillées
   */
  function distributeEqually() {
    const unlocked = hypotheses.filter(h => !h.locked);
    const n = unlocked.length;
    if (n === 0) {
      // Si aucune hypothèse non verrouillée, répartir tout sur les verrouillées
      return;
    }
    const share = Math.floor(100 / n);
    let sum = share * n;
    let remainder = 100 - sum;

    // Assigne
    unlocked.forEach(h => h.percentage = share);

    // Distribue le remainder
    for (let i = 0; i < unlocked.length && remainder > 0; i++) {
      unlocked[i].percentage++;
      remainder--;
    }
  }

  /**
   * Rééchelonne les hypothèses déjà présentes quand on a réservé p% pour la nouvelle hypothèse
   * => Les autres se partagent 100 - p proportionnellement à leur poids actuel
   */
  function redistributeAfterAddition(newPct) {
    const leftover = 100 - newPct;      // ce qu'il reste pour les autres
    if (leftover < 0) return;          // on a déjà alerté, ne devrait pas arriver

    const unlocked = hypotheses.filter(h => !h.locked);
    const sumOldHypotheses = unlocked.reduce((sum, h) => sum + h.percentage, 0);

    if (sumOldHypotheses === 0) {
      // Si aucune hypothèse non verrouillée, distribuer équitablement le leftover
      distributeEqually();
      return;
    }

    // Redistribuer proportionnellement
    unlocked.forEach(h => {
      h.percentage = Math.floor((h.percentage / sumOldHypotheses) * leftover);
    });

    // Corriger les éventuels écarts dus aux arrondis
    let sumAfter = unlocked.reduce((sum, h) => sum + h.percentage, 0);
    let diff = leftover - sumAfter;
    for (let i = 0; i < unlocked.length && diff > 0; i++) {
      unlocked[i].percentage++;
      diff--;
    }
  }

  /**
   * Quand on modifie un pourcentage existant via l'input number, 
   * on force à 100% en ajustant les hypothèses non verrouillées.
   */
  function handlePercentageChange(index, newValue) {
    if (isNaN(newValue) || newValue < 0 || newValue > 100) {
      alert("Veuillez entrer un pourcentage valide entre 0 et 100.");
      renderAll();
      return;
    }

    const oldValue = hypotheses[index].percentage;
    const delta = newValue - oldValue;
    hypotheses[index].percentage = newValue;

    let totalLocked = getTotalLockedPercentage();
    if (totalLocked > 100) {
      alert("Le total des pourcentages verrouillés dépasse 100%. Veuillez ajuster les pourcentages.");
      hypotheses[index].percentage = oldValue;
      return;
    }

    let total = getTotalPercentage();
    if (total > 100) {
      const diff = total - 100;
      adjustUnlockedPercentages(-diff, index);
    } else if (total < 100) {
      const diff = 100 - total;
      adjustUnlockedPercentages(diff, index);
    }
    saveStateToLocalStorage();
    renderAll();
  }

  /**
   * Ajuste les pourcentages des hypothèses non verrouillées
   * @param {number} diff - La différence à ajuster (positive ou négative)
   * @param {number} excludeIndex - L'index à exclure de l'ajustement
   */
  function adjustUnlockedPercentages(diff, excludeIndex) {
    const unlocked = hypotheses.filter((h, idx) => !h.locked && idx !== excludeIndex);
    const n = unlocked.length;
    if (n === 0) return;

    if (diff < 0) {
      // Réduire les pourcentages
      const reduction = Math.abs(diff);
      const share = Math.floor(reduction / n);
      let remainder = reduction - (share * n);
      unlocked.forEach(h => h.percentage = Math.max(0, h.percentage - share));
      for (let i = 0; i < unlocked.length && remainder > 0; i++) {
        if (unlocked[i].percentage > 0) {
          unlocked[i].percentage--;
          remainder--;
        }
      }
    } else {
      // Augmenter les pourcentages
      const addition = diff;
      const share = Math.floor(addition / n);
      let remainder = addition - (share * n);
      unlocked.forEach(h => h.percentage += share);
      for (let i = 0; i < unlocked.length && remainder > 0; i++) {
        unlocked[i].percentage++;
        remainder--;
      }
    }
  }

  /**************************************************/
  /*        FONCTIONS : SAUVEGARDE / RESTAURATION   */
  /**************************************************/

  function saveStateToLocalStorage() {
    localStorage.setItem('hypotheses', JSON.stringify(hypotheses));
  }

  function loadStateFromLocalStorage() {
    // Hypothèses
    const stored = localStorage.getItem('hypotheses');
    if (stored) {
      hypotheses = JSON.parse(stored);
    }

    // Question
    const storedQuestion = localStorage.getItem('question');
    if (storedQuestion) {
      questionSubtitle.textContent = storedQuestion;
      questionSubtitle.classList.remove('hidden');
      setQuestionButton.textContent = "Définir une nouvelle question";
    }
  }

  function clearAllHypotheses() {
    hypotheses = [];
    localStorage.removeItem('hypotheses');
    renderAll();
  }

  /**************************************************/
  /*         FONCTIONS : RENDER / AFFICHAGE         */
  /**************************************************/

  function renderAll() {
    sortHypotheses();
    renderHypothesesList();
    renderBar();
    drawConnections();
  }

  function sortHypotheses() {
    // Tri décroissant par pourcentage
    hypotheses.sort((a, b) => b.percentage - a.percentage);
  }

  function renderHypothesesList() {
    const list = document.getElementById('hypotheses-list');
    list.innerHTML = '';

    hypotheses.forEach((h, index) => {
      const item = document.createElement('div');
      item.className = 'hypothesis-item';
      item.style.borderLeftColor = h.color;

      const details = document.createElement('div');
      details.className = 'hypothesis-details';

      // Label
      const label = document.createElement('span');
      label.className = 'hypothesis-label';
      label.textContent = h.hypothesis;

      // Input number
      const inputNumber = document.createElement('input');
      inputNumber.type = 'number';
      inputNumber.min = 0;
      inputNumber.max = 100;
      inputNumber.step = 1;
      inputNumber.value = h.percentage;
      inputNumber.title = "Modifier le pourcentage";

      // Couleur
      const colorPicker = document.createElement('input');
      colorPicker.type = 'color';
      colorPicker.value = h.color;
      colorPicker.title = "Changer la couleur";

      // Case à cocher pour verrouiller
      const lockCheckbox = document.createElement('input');
      lockCheckbox.type = 'checkbox';
      lockCheckbox.checked = h.locked;
      lockCheckbox.className = 'lock-checkbox';
      lockCheckbox.title = "Verrouiller le pourcentage";

      const lockLabel = document.createElement('label');
      lockLabel.textContent = "Verrouiller";
      lockLabel.style.marginLeft = '5px';

      // Bouton supprimer
      const deleteBtn = document.createElement('button');
      deleteBtn.className = 'delete-button';
      deleteBtn.textContent = 'Supprimer';

      // Écouteurs
      inputNumber.addEventListener('change', (e) => {
        handlePercentageChange(index, parseInt(e.target.value));
      });
      colorPicker.addEventListener('input', (e) => {
        h.color = e.target.value;
        saveStateToLocalStorage();
        renderAll();
      });
      deleteBtn.addEventListener('click', () => handleDeleteHypothesis(index));
      lockCheckbox.addEventListener('change', (e) => {
        handleLockChange(index, e.target.checked);
      });

      // Construction
      details.appendChild(label);
      details.appendChild(inputNumber);
      details.appendChild(colorPicker);
      details.appendChild(lockCheckbox);
      details.appendChild(lockLabel);
      details.appendChild(deleteBtn);

      item.appendChild(details);
      list.appendChild(item);
    });
  }

  function handleDeleteHypothesis(index) {
    const removedPct = hypotheses[index].percentage;
    hypotheses.splice(index, 1);

    if (hypotheses.length === 0) {
      // Plus rien
      saveStateToLocalStorage();
      renderAll();
      return;
    }

    // On réintroduit ce pourcentage dans les hypothèses non verrouillées
    adjustUnlockedPercentages(removedPct, null);
    forceTotalTo100();

    saveStateToLocalStorage();
    renderAll();
  }

  /**
   * Gère le changement de verrouillage d'une hypothèse
   * @param {number} index - Index de l'hypothèse
   * @param {boolean} isLocked - État du verrouillage
   */
  function handleLockChange(index, isLocked) {
    if (isLocked) {
      const totalLocked = getTotalLockedPercentage() + hypotheses[index].percentage;
      if (totalLocked > 100) {
        alert("Le total des pourcentages verrouillés dépasse 100%. Veuillez ajuster les pourcentages.");
        // Revenir à l'état précédent
        hypotheses[index].locked = false;
        renderAll();
        return;
      }
      hypotheses[index].locked = true;
    } else {
      hypotheses[index].locked = false;
    }
    redistributeAfterLockChange();
    saveStateToLocalStorage();
    renderAll();
  }

  /**
   * Redistribue les pourcentages après un changement de verrouillage
   */
  function redistributeAfterLockChange() {
    const totalLocked = getTotalLockedPercentage();
    if (totalLocked > 100) {
      alert("Le total des pourcentages verrouillés dépasse 100%. Veuillez ajuster les pourcentages.");
      return;
    }

    const leftover = 100 - totalLocked;
    const unlocked = hypotheses.filter(h => !h.locked);
    const n = unlocked.length;

    if (n > 0) {
      const share = Math.floor(leftover / n);
      let sum = share * n;
      let remainder = leftover - sum;

      unlocked.forEach(h => h.percentage = share);

      for (let i = 0; i < n && remainder > 0; i++) {
        unlocked[i].percentage++;
        remainder--;
      }
    }
  }

  /**
   * Force le total des pourcentages à 100%
   */
  function forceTotalTo100() {
    let total = getTotalPercentage();
    if (total === 0 && hypotheses.length > 0) {
      // Tout à 0 => répartir équitablement
      distributeEqually();
      return;
    }
    if (total === 100) return;

    if (total > 100) {
      const diff = total - 100;
      adjustUnlockedPercentages(-diff, null);
    } else if (total < 100) {
      const diff = 100 - total;
      adjustUnlockedPercentages(diff, null);
    }
  }

  /**
   * Répartit la différence parmi les hypothèses non verrouillées
   * @param {number} diff - Différence à ajuster (positive ou négative)
   * @param {number|null} excludeIndex - Index à exclure de l'ajustement
   */
  function adjustUnlockedPercentages(diff, excludeIndex) {
    const unlocked = hypotheses.filter((h, idx) => !h.locked && idx !== excludeIndex);
    const n = unlocked.length;
    if (n === 0) return;

    if (diff < 0) {
      // Réduire les pourcentages
      const reduction = Math.abs(diff);
      const share = Math.floor(reduction / n);
      let remainder = reduction - (share * n);
      unlocked.forEach(h => h.percentage = Math.max(0, h.percentage - share));
      for (let i = 0; i < unlocked.length && remainder > 0; i++) {
        if (unlocked[i].percentage > 0) {
          unlocked[i].percentage--;
          remainder--;
        }
      }
    } else {
      // Augmenter les pourcentages
      const addition = diff;
      const share = Math.floor(addition / n);
      let remainder = addition - (share * n);
      unlocked.forEach(h => h.percentage += share);
      for (let i = 0; i < unlocked.length && remainder > 0; i++) {
        unlocked[i].percentage++;
        remainder--;
      }
    }
  }

  /**
   * Retourne le total des pourcentages verrouillés
   */
  function getTotalLockedPercentage() {
    return hypotheses.reduce((sum, h) => sum + (h.locked ? h.percentage : 0), 0);
  }

  function getTotalPercentage() {
    return hypotheses.reduce((sum, h) => sum + h.percentage, 0);
  }

  function renderBar() {
    const bar = document.getElementById('bar-segments');
    bar.innerHTML = '';

    hypotheses.forEach(h => {
      const segment = document.createElement('div');
      segment.className = 'segment';
      segment.style.height = h.percentage + '%';
      segment.style.backgroundColor = h.color;
      segment.title = `${h.hypothesis}: ${h.percentage}%`;
      bar.appendChild(segment);
    });
  }

  function drawConnections() {
    const container = document.getElementById('hypotheses-container');
    const containerRect = container.getBoundingClientRect();
    const list = document.getElementById('hypotheses-list');
    const listItems = list.getElementsByClassName('hypothesis-item');
    const bar = document.getElementById('percentage-bar-vertical');
    const segments = bar.getElementsByClassName('segment');
    const svg = document.getElementById('connections-svg');

    svg.innerHTML = '';

    hypotheses.forEach((h, index) => {
      // L’item DOM correspondant
      const item = listItems[index];
      const segment = segments[index];

      if (item && segment) {
        const itemRect = item.getBoundingClientRect();
        const segmentRect = segment.getBoundingClientRect();

        const x1 = itemRect.right - containerRect.left;
        const y1 = itemRect.top - containerRect.top + (itemRect.height / 2);

        const x2 = segmentRect.left - containerRect.left + (segmentRect.width / 2);
        const y2 = segmentRect.top - containerRect.top + (segmentRect.height / 2);

        const line = document.createElementNS("http://www.w3.org/2000/svg", "line");
        line.setAttribute('x1', x1);
        line.setAttribute('y1', y1);
        line.setAttribute('x2', x2);
        line.setAttribute('y2', y2);
        line.setAttribute('stroke', h.color);
        line.setAttribute('stroke-width', '1');

        svg.appendChild(line);
      }
    });
  }
</script>
