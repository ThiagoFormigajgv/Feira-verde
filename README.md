<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Feira Verde - Cronograma Interativo</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
      background: linear-gradient(135deg, #f0fdf4 0%, #f0f9ff 100%);
      color: #1f2937;
      line-height: 1.6;
    }

    .container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 20px;
    }

    header {
      background: linear-gradient(135deg, #008000 0%, #228B22 100%);
      color: white;
      padding: 30px 20px;
      border-radius: 12px;
      margin-bottom: 30px;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
      text-align: center;
    }

    header h1 {
      font-size: 2.5em;
      margin-bottom: 10px;
      text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
    }

    header p {
      font-size: 1.1em;
      opacity: 0.95;
    }

    .header-actions {
      margin-top: 15px;
      display: flex;
      gap: 10px;
      justify-content: center;
      flex-wrap: wrap;
    }

    .header-btn {
      background: rgba(255, 255, 255, 0.2);
      color: white;
      border: 2px solid white;
      padding: 10px 18px;
      border-radius: 20px;
      cursor: pointer;
      font-size: 0.95em;
      font-weight: 600;
      transition: all 0.3s ease;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .header-btn:hover {
      background: rgba(255, 255, 255, 0.3);
      transform: translateY(-2px);
    }

    .header-btn i {
      font-size: 1.1em;
    }

    .favorites-badge {
      background: rgba(255, 255, 255, 0.3);
      padding: 2px 8px;
      border-radius: 12px;
      font-size: 0.85em;
      margin-left: 5px;
    }

    .weeks-selector {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 15px;
      margin-bottom: 40px;
    }

    .week-btn {
      padding: 20px;
      border: none;
      border-radius: 12px;
      font-size: 1.1em;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.3s ease;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
      color: white;
      position: relative;
      overflow: hidden;
    }

    .week-btn::before {
      content: '';
      position: absolute;
      top: 0;
      left: -100%;
      width: 100%;
      height: 100%;
      background: rgba(255, 255, 255, 0.2);
      transition: left 0.3s ease;
    }

    .week-btn:hover::before {
      left: 100%;
    }

    .week-btn.azul {
      background: linear-gradient(135deg, #0369a1 0%, #0284c7 100%);
    }

    .week-btn.amarela {
      background: linear-gradient(135deg, #ca8a04 0%, #eab308 100%);
      color: #1f2937;
    }

    .week-btn.verde {
      background: linear-gradient(135deg, #16a34a 0%, #22c55e 100%);
    }

    .week-btn.vermelha {
      background: linear-gradient(135deg, #dc2626 0%, #ef4444 100%);
    }

    .week-btn.active {
      transform: scale(1.05);
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
    }

    .week-btn:hover {
      transform: translateY(-2px);
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
    }

    .week-label {
      display: block;
      font-size: 0.9em;
      opacity: 0.9;
      margin-bottom: 5px;
    }

    .week-period {
      display: block;
      font-size: 0.85em;
      opacity: 0.8;
      margin-top: 5px;
    }

    .days-container {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 20px;
      margin-bottom: 40px;
    }

    .day-card {
      background: white;
      border-radius: 12px;
      padding: 20px;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
      transition: all 0.3s ease;
      border-left: 5px solid #16a34a;
    }

    .day-card:hover {
      transform: translateY(-4px);
      box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
    }

    .day-card.azul { border-left-color: #0369a1; }
    .day-card.amarela { border-left-color: #ca8a04; }
    .day-card.verde { border-left-color: #16a34a; }
    .day-card.vermelha { border-left-color: #dc2626; }

    .day-title {
      font-size: 1.3em;
      font-weight: 700;
      margin-bottom: 15px;
      color: #1f2937;
    }

    .events-list {
      display: flex;
      flex-direction: column;
      gap: 12px;
    }

    .event-item {
      background: #f9fafb;
      padding: 15px;
      border-radius: 8px;
      border-left: 3px solid #16a34a;
      cursor: pointer;
      transition: all 0.3s ease;
      position: relative;
    }

    .event-item:hover {
      background: #f3f4f6;
      transform: translateX(4px);
    }

    .event-item.azul { border-left-color: #0369a1; }
    .event-item.amarela { border-left-color: #ca8a04; }
    .event-item.verde { border-left-color: #16a34a; }
    .event-item.vermelha { border-left-color: #dc2626; }

    .event-item.favorited {
      background: #fef3c7;
      border-left-color: #f59e0b;
    }

    .event-header {
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      margin-bottom: 8px;
    }

    .event-neighborhood {
      font-weight: 700;
      color: #1f2937;
      font-size: 1.05em;
      flex: 1;
    }

    .event-actions {
      display: flex;
      gap: 8px;
      margin-left: 10px;
    }

    .event-btn {
      background: none;
      border: none;
      cursor: pointer;
      transition: all 0.2s ease;
      padding: 6px 8px;
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 4px;
      color: #6b7280;
      width: 32px;
      height: 32px;
    }

    .event-btn:hover {
      background: rgba(0, 0, 0, 0.08);
      color: #1f2937;
    }

    .event-btn i {
      font-size: 1.1em;
    }

    .event-btn.favorite.active {
      color: #ef4444;
      animation: heartBeat 0.3s ease;
    }

    @keyframes heartBeat {
      0%, 100% { transform: scale(1); }
      25% { transform: scale(1.3); }
      50% { transform: scale(1.1); }
    }

    .event-time {
      color: #6b7280;
      font-size: 0.9em;
      margin-bottom: 3px;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .event-time i {
      color: #9ca3af;
      width: 16px;
    }

    .event-location {
      color: #6b7280;
      font-size: 0.9em;
      margin-bottom: 3px;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .event-location i {
      color: #9ca3af;
      width: 16px;
    }

    .event-reference {
      color: #9ca3af;
      font-size: 0.85em;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .event-reference i {
      color: #d1d5db;
      width: 16px;
    }

    .modal {
      display: none;
      position: fixed;
      z-index: 1000;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.5);
      animation: fadeIn 0.3s ease;
    }

    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }

    .modal-content {
      background-color: white;
      margin: 10% auto;
      padding: 30px;
      border-radius: 12px;
      width: 90%;
      max-width: 500px;
      box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
      animation: slideIn 0.3s ease;
    }

    @keyframes slideIn {
      from {
        transform: translateY(-50px);
        opacity: 0;
      }
      to {
        transform: translateY(0);
        opacity: 1;
      }
    }

    .modal-header {
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      margin-bottom: 20px;
    }

    .modal-title {
      font-size: 1.5em;
      font-weight: 700;
      color: #1f2937;
      flex: 1;
    }

    .modal-actions {
      display: flex;
      gap: 8px;
    }

    .modal-btn {
      background: none;
      border: none;
      cursor: pointer;
      transition: all 0.2s ease;
      padding: 6px 10px;
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 6px;
      color: #6b7280;
      width: 36px;
      height: 36px;
    }

    .modal-btn:hover {
      background: rgba(0, 0, 0, 0.08);
      color: #1f2937;
    }

    .modal-btn i {
      font-size: 1.2em;
    }

    .modal-btn.favorite.active {
      color: #ef4444;
    }

    .modal-body {
      margin-bottom: 20px;
    }

    .modal-field {
      margin-bottom: 15px;
    }

    .modal-field-label {
      font-weight: 600;
      color: #6b7280;
      font-size: 0.9em;
      margin-bottom: 5px;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .modal-field-label i {
      color: #9ca3af;
      width: 16px;
    }

    .modal-field-value {
      color: #1f2937;
      font-size: 1.05em;
      padding-left: 22px;
    }

    .modal-footer {
      display: flex;
      gap: 10px;
      justify-content: flex-end;
      flex-wrap: wrap;
    }

    .btn {
      padding: 10px 20px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-size: 1em;
      font-weight: 600;
      transition: all 0.3s ease;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .btn-primary {
      background: linear-gradient(135deg, #008000 0%, #228B22 100%);
      color: white;
    }

    .btn-primary:hover {
      transform: translateY(-2px);
      box-shadow: 0 4px 12px rgba(0, 128, 0, 0.3);
    }

    .btn-secondary {
      background: #e5e7eb;
      color: #1f2937;
    }

    .btn-secondary:hover {
      background: #d1d5db;
    }

    .btn-share {
      background: #3b82f6;
      color: white;
      padding: 8px 12px;
      font-size: 0.9em;
    }

    .btn-share:hover {
      background: #2563eb;
    }

    .close {
      background: none;
      border: none;
      cursor: pointer;
      padding: 0;
      width: 36px;
      height: 36px;
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 6px;
      color: #6b7280;
      transition: all 0.2s ease;
    }

    .close:hover {
      background: rgba(0, 0, 0, 0.08);
      color: #1f2937;
    }

    .close i {
      font-size: 1.3em;
    }

    .share-menu {
      display: none;
      position: absolute;
      background: white;
      border: 1px solid #e5e7eb;
      border-radius: 8px;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
      z-index: 100;
      min-width: 180px;
    }

    .share-menu.active {
      display: block;
    }

    .share-option {
      padding: 12px 16px;
      cursor: pointer;
      transition: background 0.2s ease;
      border: none;
      width: 100%;
      text-align: left;
      background: none;
      font-size: 0.95em;
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .share-option:hover {
      background: #f3f4f6;
    }

    .share-option i {
      width: 18px;
      color: #6b7280;
    }

    .share-option:first-child {
      border-radius: 8px 8px 0 0;
    }

    .share-option:last-child {
      border-radius: 0 0 8px 8px;
    }

    .empty-state {
      text-align: center;
      padding: 40px 20px;
      color: #9ca3af;
    }

    .empty-state-icon {
      font-size: 2.5em;
      margin-bottom: 10px;
    }

    .toast {
      position: fixed;
      bottom: 20px;
      right: 20px;
      background: #10b981;
      color: white;
      padding: 16px 24px;
      border-radius: 8px;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
      animation: slideUp 0.3s ease;
      z-index: 2000;
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .toast i {
      font-size: 1.2em;
    }

    @keyframes slideUp {
      from {
        transform: translateY(100px);
        opacity: 0;
      }
      to {
        transform: translateY(0);
        opacity: 1;
      }
    }

    @media (max-width: 768px) {
      header h1 {
        font-size: 1.8em;
      }

      .weeks-selector {
        grid-template-columns: repeat(2, 1fr);
      }

      .days-container {
        grid-template-columns: 1fr;
      }

      .modal-content {
        margin: 20% auto;
        width: 95%;
      }

      .header-actions {
        flex-direction: column;
      }

      .header-btn {
        width: 100%;
        justify-content: center;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <h1><i class="fas fa-leaf"></i> Feira Verde</h1>
      <p>Cronograma Interativo de Feiras - Novembro 2024</p>
      <div class="header-actions">
        <button class="header-btn" onclick="showFavorites()" title="Ver favoritos">
          <i class="fas fa-heart"></i> Favoritos <span class="favorites-badge" id="favoritesBadge">0</span>
        </button>
        <button class="header-btn" onclick="clearAllFavorites()" title="Limpar todos os favoritos">
          <i class="fas fa-trash-alt"></i> Limpar
        </button>
      </div>
    </header>

    <div class="weeks-selector" id="weeksSelector"></div>

    <div class="days-container" id="daysContainer"></div>

    <div id="eventModal" class="modal">
      <div class="modal-content">
        <div class="modal-header">
          <div class="modal-title" id="modalTitle"></div>
          <div class="modal-actions">
            <button class="modal-btn favorite" id="modalFavorite" onclick="toggleFavoriteFromModal()" title="Adicionar aos favoritos">
              <i class="far fa-heart"></i>
            </button>
            <button class="modal-btn" id="modalShare" onclick="toggleShareMenu()" title="Compartilhar">
              <i class="fas fa-share-alt"></i>
            </button>
            <button class="close" onclick="closeModal()" title="Fechar">
              <i class="fas fa-times"></i>
            </button>
          </div>
        </div>
        <div class="modal-body">
          <div class="modal-field">
            <span class="modal-field-label">
              <i class="fas fa-clock"></i> Horário
            </span>
            <div class="modal-field-value" id="modalTime"></div>
          </div>
          <div class="modal-field">
            <span class="modal-field-label">
              <i class="fas fa-map-marker-alt"></i> Localização
            </span>
            <div class="modal-field-value" id="modalLocation"></div>
          </div>
          <div class="modal-field">
            <span class="modal-field-label">
              <i class="fas fa-compass"></i> Próximo à
            </span>
            <div class="modal-field-value" id="modalReference"></div>
          </div>
        </div>
        <div class="modal-footer">
          <div class="share-menu" id="shareMenu">
            <button class="share-option" onclick="shareVia('whatsapp')">
              <i class="fab fa-whatsapp"></i> WhatsApp
            </button>
            <button class="share-option" onclick="shareVia('email')">
              <i class="fas fa-envelope"></i> Email
            </button>
            <button class="share-option" onclick="shareVia('copy')">
              <i class="fas fa-copy"></i> Copiar
            </button>
          </div>
          <button class="btn btn-secondary" onclick="closeModal()">
            <i class="fas fa-times"></i> Fechar
          </button>
        </div>
      </div>
    </div>
  </div>

  <script>
    const cronograma = {
      "semanas": [
        {
          "id": 1,
          "nome": "Semana Azul",
          "cor": "azul",
          "periodo": "03 a 07/11",
          "dias": [
            {"id": "seg1", "nome": "Segunda-Feira", "eventos": [{"bairro": "Pedrinhas 1", "horario": "09H às 10H", "local": "R. Maria Roseli de Miranda", "proximo": "Praça da Saudade"}, {"bairro": "Pedrinhas 2", "horario": "10H às 11H", "local": "Av. Bertina Faustino Xavier", "proximo": "Ponte do Adão"}]},
            {"id": "ter1", "nome": "Terça-Feira", "eventos": [{"bairro": "Jardim Samambaia", "horario": "09H às 10H", "local": "R. Jasmim", "proximo": "Quadrinha do Mercado Alexandrino"}, {"bairro": "Sub 50", "horario": "10H às 11H", "local": "R. Gerônimo Porfírio de Matos", "proximo": "Esquina do Cemitério"}]},
            {"id": "qua1", "nome": "Quarta-Feira", "eventos": [{"bairro": "Remonta", "horario": "09H às 11H", "local": "R. Sete de Setembro", "proximo": "Esquina do Ginásio Antônio Alves Filho"}]},
            {"id": "qui1", "nome": "Quinta-Feira", "eventos": [{"bairro": "Jardim São Roque", "horario": "09H às 10H", "local": "R. Levy Macedo Taques", "proximo": "Bifurcação - Ao lado do Bar do Sérgio"}, {"bairro": "Bosque da Saúde", "horario": "10H às 11H", "local": "R. Caviúna", "proximo": "Em frente à Pracinha"}]},
            {"id": "sex1", "nome": "Sexta-Feira", "eventos": [{"bairro": "Limpeza e Manutenção dos Equipamentos", "horario": "Dia Todo", "local": "N/A", "proximo": "N/A"}]}
          ]
        },
        {
          "id": 2,
          "nome": "Semana Amarela",
          "cor": "amarela",
          "periodo": "10 a 14/11",
          "dias": [
            {"id": "seg2", "nome": "Segunda-Feira", "eventos": [{"bairro": "Vila Edith", "horario": "09H às 11H", "local": "R. Sebastiana Pereira da Silva", "proximo": "Ao lado da Quadra"}, {"bairro": "Vila Fonseca", "horario": "14H às 15H", "local": "R. Santa Ana", "proximo": "Auto Elétrica Eguert"}, {"bairro": "Lagoão", "horario": "15H às 16H", "local": "Rua Olga Kojo Turek", "proximo": "Bar do Gira (Alto Lagoão)"}]},
            {"id": "ter2", "nome": "Terça-Feira", "eventos": [{"bairro": "Vila São Luiz", "horario": "09H às 10H", "local": "R. Jacy Rodrigues Brotas", "proximo": "1ª Rua subindo a esquina"}, {"bairro": "Vila Fluviópolis", "horario": "10H às 11H", "local": "Av. Fluviópolis", "proximo": "Caps"}]},
            {"id": "qua2", "nome": "Quarta-Feira", "eventos": [{"bairro": "Jd N. Sra de Fátima", "horario": "09H às 10H", "local": "R. Porto Alegre", "proximo": "Igreja Quadrangular"}, {"bairro": "Jardim Matarazzo", "horario": "10H às 11H", "local": "R. Walfrido Sandrine", "proximo": "Academia ao ar livre ao lado"}]},
            {"id": "qui2", "nome": "Quinta-Feira", "eventos": [{"bairro": "Vila Kennedy 1", "horario": "09H às 11H", "local": "Av. Coronel Calazans", "proximo": "Esquina com BNH"}]},
            {"id": "sex2", "nome": "Sexta-Feira", "eventos": [{"bairro": "Limpeza e Manutenção dos Equipamentos", "horario": "Dia Todo", "local": "N/A", "proximo": "N/A"}]}
          ]
        },
        {
          "id": 3,
          "nome": "Semana Verde",
          "cor": "verde",
          "periodo": "17 a 21/11",
          "dias": [
            {"id": "seg3", "nome": "Segunda-Feira", "eventos": [{"bairro": "Vila André", "horario": "09H às 10H", "local": "R. Luiza Maria Cristina Delgado", "proximo": "2ª Esquina após a Rotatória"}, {"bairro": "Vila Pinheiro", "horario": "10H às 11H", "local": "R. João Cava", "proximo": "Ginásio de Esportes"}]},
            {"id": "ter3", "nome": "Terça-Feira", "eventos": [{"bairro": "Santa Cecília", "horario": "09H às 11H", "local": "R. Miguel Xavier da Silva", "proximo": "Esquina da Congregação Cristã"}]},
            {"id": "qua3", "nome": "Quarta-Feira", "eventos": [{"bairro": "Jardim Primavera 3", "horario": "09H às 11H", "local": "R. Pato Branco", "proximo": "Frente ao Brechó da Vó Lu"}]},
            {"id": "qui3", "nome": "Quinta-Feira", "eventos": [{"bairro": "Jardim Primavera 3", "horario": "09H às 11H", "local": "Avenida Sertaneja", "proximo": "Padaria Delícias do Trigo"}]},
            {"id": "sex3", "nome": "Sexta-Feira", "eventos": [{"bairro": "Limpeza e Manutenção dos Equipamentos", "horario": "Dia Todo", "local": "N/A", "proximo": "N/A"}]}
          ]
        },
        {
          "id": 4,
          "nome": "Semana Vermelha",
          "cor": "vermelha",
          "periodo": "24 a 28/11",
          "dias": [
            {"id": "seg4", "nome": "Segunda-Feira", "eventos": [{"bairro": "Jardim Primavera 2", "horario": "9H às 11H", "local": "Rua Maringá", "proximo": "Congregação Cristã"}]},
            {"id": "ter4", "nome": "Terça-Feira", "eventos": [{"bairro": "Bela Vista", "horario": "9H às 11H", "local": "Rua Rosa Ginaqui Pomim", "proximo": "Igreja Wesleyana"}]},
            {"id": "qua4", "nome": "Quarta-Feira", "eventos": [{"bairro": "Vila Kennedy 2", "horario": "9H às 11H", "local": "R. Ivani Pinheiro Zanão, nº 810", "proximo": "Igreja Presbiteriana Pentecostal"}]},
            {"id": "qui4", "nome": "Quinta-Feira", "eventos": [{"bairro": "Portal do Sertão", "horario": "9H às 11H", "local": "Rua Otélio Renato Baroni", "proximo": "Centro da Rua"}]},
            {"id": "sex4", "nome": "Sexta-Feira", "eventos": [{"bairro": "Limpeza e Manutenção dos Equipamentos", "horario": "Dia Todo", "local": "N/A", "proximo": "N/A"}]}
          ]
        }
      ]
    };

    let currentWeek = 1;
    let currentEventData = null;
    const modal = document.getElementById('eventModal');

    // Favorites Management
    function getFavorites() {
      const fav = localStorage.getItem('feirVerdesFavoritos');
      return fav ? JSON.parse(fav) : [];
    }

    function saveFavorites(favorites) {
      localStorage.setItem('feirVerdesFavoritos', JSON.stringify(favorites));
      updateFavoritesBadge();
    }

    function toggleFavorite(bairro, horario, local, proximo) {
      const favorites = getFavorites();
      const key = `${bairro}|${horario}|${local}`;
      const index = favorites.findIndex(f => f.key === key);

      if (index > -1) {
        favorites.splice(index, 1);
        showToast('Removido dos favoritos', 'heart');
      } else {
        favorites.push({ key, bairro, horario, local, proximo });
        showToast('Adicionado aos favoritos', 'heart');
      }

      saveFavorites(favorites);
      renderDays();
    }

    function toggleFavoriteFromModal() {
      if (currentEventData) {
        toggleFavorite(currentEventData.bairro, currentEventData.horario, currentEventData.local, currentEventData.proximo);
        updateModalFavoriteButton();
      }
    }

    function isFavorited(bairro, horario, local) {
      const favorites = getFavorites();
      const key = `${bairro}|${horario}|${local}`;
      return favorites.some(f => f.key === key);
    }

    function updateFavoritesBadge() {
      document.getElementById('favoritesBadge').textContent = getFavorites().length;
    }

    function updateModalFavoriteButton() {
      const btn = document.getElementById('modalFavorite');
      if (currentEventData && isFavorited(currentEventData.bairro, currentEventData.horario, currentEventData.local)) {
        btn.classList.add('active');
        btn.innerHTML = '<i class="fas fa-heart"></i>';
      } else {
        btn.classList.remove('active');
        btn.innerHTML = '<i class="far fa-heart"></i>';
      }
    }

    function clearAllFavorites() {
      if (confirm('Tem certeza que deseja limpar todos os favoritos?')) {
        localStorage.removeItem('feirVerdesFavoritos');
        updateFavoritesBadge();
        renderDays();
        showToast('Todos os favoritos foram removidos', 'trash-alt');
      }
    }

    function showFavorites() {
      const favorites = getFavorites();
      if (favorites.length === 0) {
        alert('Você ainda não tem favoritos!\n\nClique no ícone de coração em um evento para adicioná-lo aos favoritos.');
        return;
      }

      let message = 'SEUS FAVORITOS:\n\n';
      favorites.forEach((fav, index) => {
        message += `${index + 1}. ${fav.bairro}\n   ${fav.horario} - ${fav.local}\n\n`;
      });
      alert(message);
    }

    // Share functionality
    function toggleShareMenu() {
      const menu = document.getElementById('shareMenu');
      menu.classList.toggle('active');
    }

    function shareVia(method) {
      if (!currentEventData) return;

      const text = `Feira Verde\n\n${currentEventData.bairro}\n⏰ ${currentEventData.horario}\n📍 ${currentEventData.local}\n🧭 Próximo à: ${currentEventData.proximo}`;

      if (method === 'whatsapp') {
        const encoded = encodeURIComponent(text);
        window.open(`https://wa.me/?text=${encoded}`, '_blank');
      } else if (method === 'email') {
        const subject = encodeURIComponent(`Feira Verde - ${currentEventData.bairro}`);
        const body = encodeURIComponent(text);
        window.open(`mailto:?subject=${subject}&body=${body}`, '_blank');
      } else if (method === 'copy') {
        navigator.clipboard.writeText(text).then(() => {
          showToast('Copiado para a área de transferência!', 'copy');
          document.getElementById('shareMenu').classList.remove('active');
        });
      }
    }

    // Toast notification
    function showToast(message, icon = 'check') {
      const toast = document.createElement('div');
      toast.className = 'toast';
      toast.innerHTML = `<i class="fas fa-${icon}"></i> ${message}`;
      document.body.appendChild(toast);
      setTimeout(() => toast.remove(), 3000);
    }

    function renderWeeks() {
      const selector = document.getElementById('weeksSelector');
      selector.innerHTML = '';
      cronograma.semanas.forEach(semana => {
        const btn = document.createElement('button');
        btn.className = `week-btn ${semana.cor} ${semana.id === currentWeek ? 'active' : ''}`;
        btn.innerHTML = `<span class="week-label">${semana.nome}</span><span class="week-period">${semana.periodo}</span>`;
        btn.onclick = () => selectWeek(semana.id);
        selector.appendChild(btn);
      });
    }

    function selectWeek(weekId) {
      currentWeek = weekId;
      renderWeeks();
      renderDays();
    }

    function renderDays() {
      const week = cronograma.semanas.find(s => s.id === currentWeek);
      const container = document.getElementById('daysContainer');
      container.innerHTML = '';

      week.dias.forEach(dia => {
        const dayCard = document.createElement('div');
        dayCard.className = `day-card ${week.cor}`;
        
        let eventsHTML = '';
        if (dia.eventos.length === 0) {
          eventsHTML = '<div class="empty-state"><div class="empty-state-icon"><i class="fas fa-inbox"></i></div>Nenhum evento</div>';
        } else {
          dia.eventos.forEach(evento => {
            const isFav = isFavorited(evento.bairro, evento.horario, evento.local);
            eventsHTML += `
              <div class="event-item ${week.cor} ${isFav ? 'favorited' : ''}">
                <div class="event-header">
                  <div class="event-neighborhood">${evento.bairro}</div>
                  <div class="event-actions">
                    <button class="event-btn favorite ${isFav ? 'active' : ''}" onclick="toggleFavorite('${evento.bairro}', '${evento.horario}', '${evento.local}', '${evento.proximo}')" title="Adicionar aos favoritos">
                      <i class="fa${isFav ? 's' : 'r'} fa-heart"></i>
                    </button>
                    <button class="event-btn" onclick="showModal('${evento.bairro}', '${evento.horario}', '${evento.local}', '${evento.proximo}')" title="Ver detalhes">
                      <i class="fas fa-eye"></i>
                    </button>
                  </div>
                </div>
                <div class="event-time"><i class="fas fa-clock"></i> ${evento.horario}</div>
                <div class="event-location"><i class="fas fa-map-marker-alt"></i> ${evento.local}</div>
                <div class="event-reference"><i class="fas fa-compass"></i> ${evento.proximo}</div>
              </div>
            `;
          });
        }

        dayCard.innerHTML = `
          <div class="day-title">${dia.nome}</div>
          <div class="events-list">${eventsHTML}</div>
        `;
        container.appendChild(dayCard);
      });
    }

    function showModal(bairro, horario, local, proximo) {
      currentEventData = { bairro, horario, local, proximo };
      document.getElementById('modalTitle').textContent = bairro;
      document.getElementById('modalTime').textContent = horario;
      document.getElementById('modalLocation').textContent = local;
      document.getElementById('modalReference').textContent = proximo;
      updateModalFavoriteButton();
      document.getElementById('shareMenu').classList.remove('active');
      modal.style.display = 'block';
    }

    function closeModal() {
      modal.style.display = 'none';
      currentEventData = null;
    }

    window.onclick = (event) => {
      if (event.target === modal) {
        closeModal();
      }
    };

    // Initialize
    updateFavoritesBadge();
    renderWeeks();
    renderDays();
  </script>
</body>
</html>
