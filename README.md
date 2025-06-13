# Flashcard<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Flash Card Study Tool - GitHub Inspired</title>
<style>
  /* GitHub-inspired color palette and fonts */
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600&display=swap');
  @import url('https://fonts.googleapis.com/css2?family=SF+Mono&display=swap');

  :root {
    --color-bg-light: #ffffff;
    --color-bg-muted: #f6f8fa;
    --color-text-primary: #24292e;
    --color-text-secondary: #57606a;
    --color-link: #0366d6;
    --color-green: #28a745;
    --color-red: #d73a49;
    --color-orange: #f66a0a;

    --font-ui: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica, Arial, sans-serif;
    --font-mono: 'SF Mono', Consolas, 'Liberation Mono', Menlo, monospace;

    --spacing-1: 8px;
    --spacing-2: 16px;
    --spacing-3: 24px;
    --spacing-4: 32px;
    --spacing-5: 48px;

    --header-height: 56px;

    --card-width: 320px;
    --card-height: 200px;
  }

  /* Reset and base styles */
  *, *::before, *::after {
    box-sizing: border-box;
  }
  body {
    margin: 0;
    background: var(--color-bg-light);
    color: var(--color-text-primary);
    font-family: var(--font-ui);
    line-height: 1.5;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
  }
  a {
    color: var(--color-link);
    text-decoration: none;
  }
  a:hover,
  a:focus {
    text-decoration: underline;
  }
  button {
    font-family: var(--font-ui);
    font-size: 1rem;
    cursor: pointer;
    background: none;
    border: none;
    color: var(--color-link);
    padding: var(--spacing-1);
    border-radius: 6px;
    transition: background-color 0.2s ease;
  }
  button:hover,
  button:focus {
    background-color: var(--color-bg-muted);
    outline: none;
  }
  input[type="text"] {
    font-family: var(--font-ui);
    font-size: 1rem;
    padding: var(--spacing-1);
    border: 1px solid #d1d5da;
    border-radius: 6px;
    width: 100%;
    max-width: 300px;
  }
  input[type="text"]:focus {
    border-color: var(--color-link);
    outline: none;
    box-shadow: 0 0 5px var(--color-link);
  }
  /* Header styles */
  header {
    height: var(--header-height);
    background-color: var(--color-bg-muted);
    border-bottom: 1px solid #e1e4e8;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 var(--spacing-3);
    position: sticky;
    top: 0;
    z-index: 1000;
  }

  .logo {
    font-weight: 600;
    font-size: 1.25rem;
    color: var(--color-text-primary);
    user-select: none;
    display: flex;
    align-items: center;
    font-family: var(--font-mono);
  }

  .logo svg {
    height: 24px;
    width: 24px;
    margin-right: 8px;
    fill: var(--color-link);
  }

  .hamburger {
    display: none;
    background: none;
    border: none;
    font-size: 1.5rem;
    color: var(--color-text-primary);
  }
  .hamburger:focus {
    outline: 2px solid var(--color-link);
    outline-offset: 2px;
  }

  nav {
    display: flex;
    align-items: center;
    gap: var(--spacing-3);
  }

  nav a {
    font-weight: 600;
    color: var(--color-text-primary);
  }
  nav a[aria-current="page"] {
    color: var(--color-link);
  }

  /* Layout container */
  main {
    flex-grow: 1;
    display: flex;
    min-height: calc(100vh - var(--header-height));
    overflow: hidden;
    background: var(--color-bg-muted);
  }

  /* Sidebar */
  .sidebar {
    background: var(--color-bg-light);
    width: 280px;
    min-width: 280px;
    border-right: 1px solid #e1e4e8;
    display: flex;
    flex-direction: column;
  }
  .sidebar.collapsed {
    width: 0;
    min-width: 0;
  }

  .sidebar-header {
    padding: var(--spacing-3);
    border-bottom: 1px solid #e1e4e8;
    font-weight: 600;
    font-size: 1.1rem;
    color: var(--color-text-primary);
    user-select: none;
  }

  .cards-list {
    flex-grow: 1;
    overflow-y: auto;
    padding: var(--spacing-2);
  }
  .cards-list button {
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
    color: var(--color-text-primary);
    text-align: left;
    font-weight: 500;
    padding: var(--spacing-1);
    border-radius: 6px;
    border: 1px solid transparent;
    transition: background-color 0.2s, border-color 0.2s;
  }
  .cards-list button:hover,
  .cards-list button:focus {
    background-color: var(--color-bg-muted);
    border-color: var(--color-link);
    outline: none;
  }
  .cards-list button.active {
    background-color: var(--color-link);
    color: white;
    font-weight: 600;
  }
  .cards-list button .remove-btn {
    background: none;
    border: none;
    color: var(--color-red);
    font-size: 1rem;
    cursor: pointer;
    margin-left: var(--spacing-2);
  }
  .cards-list button .remove-btn:hover,
  .cards-list button .remove-btn:focus {
    color: #a12024;
    outline: none;
  }

  .add-card-section {
    padding: var(--spacing-3);
    border-top: 1px solid #e1e4e8;
  }
  .add-card-section input[type="text"] {
    margin-bottom: var(--spacing-2);
  }
  .add-card-section button {
    width: 100%;
    padding: var(--spacing-2);
    background-color: var(--color-green);
    color: white;
    font-weight: 600;
    border-radius: 6px;
    border: none;
    transition: background-color 0.3s ease;
  }
  .add-card-section button:hover,
  .add-card-section button:focus {
    background-color: #1e7e34;
    outline: none;
  }

  /* Content Area */
  .content-area {
    flex-grow: 1;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: var(--spacing-4);
    overflow-y: auto;
  }

  /* Flash Card container */
  .flashcard-container {
    perspective: 1500px;
    width: var(--card-width);
    height: var(--card-height);
    max-width: 90vw;
  }

  /* Flash card base */
  .flashcard {
    width: 100%;
    height: 100%;
    border-radius: 12px;
    box-shadow: 0 8px 24px rgba(36,41,46,0.12);
    background: white;
    color: var(--color-text-primary);
    font-family: var(--font-ui);
    font-weight: 600;
    font-size: 1.25rem;
    line-height: 1.4;
    user-select: none;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: var(--spacing-3);
    text-align: center;
    border: 1px solid #d1d5da;
    box-sizing: border-box;

    /* 3D flip */
    transform-style: preserve-3d;
    transition: transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
    position: relative;
  }
  .flashcard.flipped {
    transform: rotateY(180deg);
  }

  /* Front and Back faces */
  .flashcard .front,
  .flashcard .back {
    position: absolute;
    width: 100%;
    height: 100%;
    border-radius: inherit;
    backface-visibility: hidden;
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding: var(--spacing-3);
  }
  .flashcard .back {
    transform: rotateY(180deg);
    color: var(--color-text-secondary);
  }

  /* Controls below the card */
  .card-controls {
    margin-top: var(--spacing-4);
    display: flex;
    justify-content: center;
    gap: var(--spacing-3);
  }
  .card-controls button {
    background-color: var(--color-link);
    color: white;
    border-radius: 6px;
    padding: var(--spacing-2) var(--spacing-3);
    font-weight: 600;
    font-size: 1rem;
    border: none;
    box-shadow: 0 4px 8px rgba(3, 102, 214, 0.4);
    transition: background-color 0.3s ease;
    min-width: 100px;
  }
  .card-controls button:hover,
  .card-controls button:focus {
    background-color: #024ea2;
    outline: none;
  }

  /* Search bar in sidebar */
  .sidebar-search {
    padding: var(--spacing-2) var(--spacing-3);
    border-bottom: 1px solid #e1e4e8;
  }
  .sidebar-search input[type="text"] {
    width: 100%;
    border-radius: 6px;
    border: 1px solid #d1d5da;
    padding: var(--spacing-1);
    font-size: 1rem;
  }
  .sidebar-search input[type="text"]:focus {
    border-color: var(--color-link);
    box-shadow: 0 0 5px var(--color-link);
    outline: none;
  }

  /* Responsive adjustments */

  /* Mobile: 320px - 767px */
  @media (max-width: 767px) {
    header {
      padding: 0 var(--spacing-2);
    }
    .hamburger {
      display: block;
    }
    nav {
      display: none;
    }
    main {
      flex-direction: column;
    }
    .sidebar {
      position: fixed;
      top: var(--header-height);
      left: 0;
      height: calc(100vh - var(--header-height));
      background: var(--color-bg-light);
      border-right: 1px solid #e1e4e8;
      transform: translateX(-100%);
      transition: transform 0.3s ease;
      z-index: 2000;
      width: 280px;
    }
    .sidebar.open {
      transform: translateX(0);
      box-shadow: 4px 0 8px rgba(0,0,0,0.1);
    }
    .content-area {
      padding: var(--spacing-3);
      margin-top: var(--header-height);
      width: 100%;
    }
    /* Flashcard width smaller to fit mobile */
    .flashcard-container {
      width: 90vw;
      height: 180px;
    }
  }

  /* Tablet: 768px - 1023px */
  @media (min-width: 768px) and (max-width: 1023px) {
    .sidebar {
      position: relative;
      width: 280px;
      min-width: 280px;
      transform: none !important;
      box-shadow: none;
    }
    main {
      flex-direction: row;
    }
    .content-area {
      padding: var(--spacing-4);
    }
  }

  /* Desktop: 1024px - 1439px */
  @media (min-width: 1024px) and (max-width: 1439px) {
    .sidebar {
      width: 320px;
      min-width: 320px;
    }
    main {
      flex-direction: row;
    }
    .content-area {
      padding: var(--spacing-5);
      max-width: calc(100% - 320px);
    }
  }

  /* Large desktop 1440px+ */
  @media (min-width: 1440px) {
    main {
      max-width: 1440px;
      margin: 0 auto;
      flex-direction: row;
    }
    .sidebar {
      width: 320px;
      min-width: 320px;
    }
    .content-area {
      padding: var(--spacing-5);
      max-width: calc(1440px - 320px);
    }
  }

  /* Accessibility focus */
  button:focus-visible,
  input:focus-visible {
    outline: 3px solid var(--color-link);
    outline-offset: 2px;
  }
</style>
<link
  href="https://fonts.googleapis.com/icon?family=Material+Icons"
  rel="stylesheet"
/>
</head>
<body>
<header role="banner">
  <div class="logo" aria-label="Flash Card Study Tool">
    <svg aria-hidden="true" viewBox="0 0 24 24"><path d="M19 3H5a2 2 0 0 0-2 2v14l4-4h12a2 2 0 0 0 2-2V5a2 2 0 0 0-2-2zM5 15v-2h14v2H5z"></path></svg>
    FlashCards
  </div>
  <button
    class="hamburger"
    aria-label="Toggle menu"
    aria-controls="sidebar"
    aria-expanded="false"
  >
    <span class="material-icons">menu</span>
  </button>
  <nav role="navigation" aria-label="Primary navigation">
    <a href="#" aria-current="page">Flash Cards</a>
    <a href="https://github.com/" target="_blank" rel="noopener noreferrer" aria-label="GitHub Repository Link">GitHub Repo</a>
  </nav>
</header>
<main>
  <aside
    class="sidebar"
    id="sidebar"
    aria-label="Flash cards list"
    tabindex="-1"
  >
    <div class="sidebar-header">Meus Flash Cards</div>
    <div class="sidebar-search">
      <input
        id="searchInput"
        type="text"
        placeholder="Buscar flash cards..."
        aria-label="Buscar flash cards"
      />
    </div>
    <div class="cards-list" role="list" aria-live="polite" aria-relevant="additions removals"></div>
    <div class="add-card-section" aria-label="Adicionar novo flash card">
      <input
        id="newFrontInput"
        type="text"
        placeholder="Frente da carta (pergunta)"
        aria-label="Frente da carta (pergunta)"
      />
      <input
        id="newBackInput"
        type="text"
        placeholder="Verso da carta (resposta)"
        aria-label="Verso da carta (resposta)"
      />
      <button id="addCardBtn" aria-label="Adicionar nova carta">Adicionar</button>
    </div>
  </aside>
  <section class="content-area" aria-live="polite" aria-atomic="true">
    <div
      class="flashcard-container"
      role="region"
      aria-label="Flash card atual"
      tabindex="0"
    >
      <article class="flashcard" id="flashcard" tabindex="0" aria-pressed="false" aria-live="polite">
        <div class="front" role="heading" aria-level="2" id="cardFront">
          <!-- Question front -->
        </div>
        <div class="back" role="region" aria-hidden="true" id="cardBack">
          <!-- Answer back -->
        </div>
      </article>
    </div>
    <div class="card-controls" role="toolbar" aria-label="Controles do flash card">
      <button id="prevCard" aria-label="Cartão anterior" title="Cartão anterior">
        <span class="material-icons" aria-hidden="true">chevron_left</span>
        Anterior
      </button>
      <button id="flipCard" aria-label="Virar carta" title="Virar carta">
        <span class="material-icons" aria-hidden="true">flip</span>
        Virar
      </button>
      <button id="nextCard" aria-label="Próximo cartão" title="Próximo cartão">
        Próximo
        <span class="material-icons" aria-hidden="true">chevron_right</span>
      </button>
    </div>
  </section>
</main>
<script>
  (() => {
    'use strict';

    // DOM elements
    const hamburgerBtn = document.querySelector('.hamburger');
    const sidebar = document.getElementById('sidebar');
    const cardsListContainer = document.querySelector('.cards-list');
    const addCardBtn = document.getElementById('addCardBtn');
    const newFrontInput = document.getElementById('newFrontInput');
    const newBackInput = document.getElementById('newBackInput');
    const searchInput = document.getElementById('searchInput');

    const flashcardEl = document.getElementById('flashcard');
    const cardFrontEl = document.getElementById('cardFront');
    const cardBackEl = document.getElementById('cardBack');

    const prevCardBtn = document.getElementById('prevCard');
    const flipCardBtn = document.getElementById('flipCard');
    const nextCardBtn = document.getElementById('nextCard');

    // Data keys for localStorage
    const STORAGE_KEY = 'flashcards-data';

    // State
    let flashcards = [];
    let filteredFlashcards = [];
    let currentIndex = 0;
    let isFlipped = false;

    // Helper - load flashcards from localStorage
    function loadFlashcards() {
      try {
        const saved = localStorage.getItem(STORAGE_KEY);
        if (saved) {
          const parsed = JSON.parse(saved);
          if (Array.isArray(parsed)) {
            flashcards = parsed;
          } else {
            flashcards = [];
          }
        } else {
          flashcards = [];
        }
      } catch (e) {
        flashcards = [];
      }
      filteredFlashcards = flashcards;
    }

    // Helper - save flashcards to localStorage
    function saveFlashcards() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(flashcards));
    }

    // Render list of cards in sidebar
    function renderCardsList() {
      // Clear existing
      cardsListContainer.innerHTML = '';
      if (filteredFlashcards.length === 0) {
        const emptyMessage = document.createElement('p');
        emptyMessage.textContent = 'Nenhum flash card encontrado.';
        emptyMessage.style.color = '#57606a';
        emptyMessage.style.fontStyle = 'italic';
        cardsListContainer.appendChild(emptyMessage);
        return;
      }
      filteredFlashcards.forEach((card, idx) => {
        const btn = document.createElement('button');
        btn.type = 'button';
        btn.setAttribute('role', 'listitem');
        btn.textContent = card.front.length > 30 ? card.front.slice(0, 27) + '...' : card.front;
        btn.className = (currentIndex === idx) ? 'active' : '';
        btn.setAttribute('aria-current', currentIndex === idx ? 'true' : 'false');
        btn.title = card.front;
        btn.addEventListener('click', () => {
          currentIndex = idx;
          isFlipped = false;
          renderFlashcard();
          renderCardsList();
          flashcardEl.focus();
          if (sidebar.classList.contains('open')) {
            toggleSidebar();
          }
        });
        // Remove button inside
        const removeBtn = document.createElement('button');
        removeBtn.type = 'button';
        removeBtn.className = 'remove-btn';
        removeBtn.setAttribute('aria-label', 'Remover flash card');
        removeBtn.innerHTML = '<span class="material-icons" aria-hidden="true">delete</span>';
        removeBtn.addEventListener('click', e => {
          e.stopPropagation();
          removeFlashcard(idx);
        });

        btn.appendChild(removeBtn);
        cardsListContainer.appendChild(btn);
      });
    }

    // Render currently selected flashcard
    function renderFlashcard() {
      if (filteredFlashcards.length === 0) {
        cardFrontEl.textContent = 'Nenhum flash card disponível.';
        cardBackEl.textContent = '';
        flashcardEl.classList.remove('flipped');
        flashcardEl.setAttribute('aria-pressed', 'false');
        return;
      }
      const card = filteredFlashcards[currentIndex];
      cardFrontEl.textContent = card.front;
      cardBackEl.textContent = card.back;
      if (isFlipped) {
        flashcardEl.classList.add('flipped');
        flashcardEl.setAttribute('aria-pressed', 'true');
        cardBackEl.setAttribute('aria-hidden', 'false');
        cardFrontEl.setAttribute('aria-hidden', 'true');
      } else {
        flashcardEl.classList.remove('flipped');
        flashcardEl.setAttribute('aria-pressed', 'false');
        cardBackEl.setAttribute('aria-hidden', 'true');
        cardFrontEl.setAttribute('aria-hidden', 'false');
      }
    }

    // Add new flashcard
    function addNewFlashcard() {
      const front = newFrontInput.value.trim();
      const back = newBackInput.value.trim();
      if (!front || !back) {
        alert('Por favor, preencha frente e verso da carta.');
        return;
      }
      flashcards.push({ front, back });
      saveFlashcards();
      filteredFlashcards = flashcards;
      currentIndex = filteredFlashcards.length - 1;
      newFrontInput.value = '';
      newBackInput.value = '';
      renderCardsList();
      renderFlashcard();
      flashcardEl.focus();
    }

    // Remove flashcard by index
    function removeFlashcard(idx) {
      if (idx < 0 || idx >= filteredFlashcards.length) return;
      const cardToRemove = filteredFlashcards[idx];
      const originalIndex = flashcards.findIndex(c => c.front === cardToRemove.front && c.back === cardToRemove.back);
      if (originalIndex !== -1) {
        flashcards.splice(originalIndex, 1);
      }
      saveFlashcards();
      filterFlashcards(searchInput.value);
      if (currentIndex >= filteredFlashcards.length) {
        currentIndex = filteredFlashcards.length - 1;
      }
      renderCardsList();
      renderFlashcard();
    }

    // Flip card
    function flipCard() {
      if (filteredFlashcards.length === 0) return;
      isFlipped = !isFlipped;
      renderFlashcard();
    }

    // Show next card
    function showNextCard() {
      if (filteredFlashcards.length === 0) return;
      currentIndex = (currentIndex + 1) % filteredFlashcards.length;
      isFlipped = false;
      renderFlashcard();
      renderCardsList();
      flashcardEl.focus();
    }

    // Show previous card
    function showPrevCard() {
      if (filteredFlashcards.length === 0) return;
      currentIndex = (currentIndex - 1 + filteredFlashcards.length) % filteredFlashcards.length;
      isFlipped = false;
      renderFlashcard();
      renderCardsList();
      flashcardEl.focus();
    }

    // Filter flashcards by search query
    function filterFlashcards(query) {
      query = query.trim().toLowerCase();
      if (!query) {
        filteredFlashcards = flashcards;
      } else {
        filteredFlashcards = flashcards.filter(card =>
          card.
