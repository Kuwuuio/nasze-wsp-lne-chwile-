# nasze-wsp-lne-chwile-
<html lang="pl">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Nasze Chwile</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: #fff0f5;
      margin: 0;
      padding: 0;
      display: flex;
      flex-direction: column;
      align-items: center;
    }
    header {
      background-color: #ff80ab;
      color: white;
      padding: 1rem;
      font-size: 2rem;
      width: 100%;
      text-align: center;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    .calendar {
      display: grid;
      grid-template-columns: repeat(7, 1fr);
      gap: 5px;
      max-width: 400px;
      width: 100%;
      padding: 1rem;
    }
    .day {
      background-color: #ffe4ec;
      border-radius: 10px;
      padding: 1rem;
      text-align: center;
      cursor: pointer;
      box-shadow: 0 1px 3px rgba(0,0,0,0.1);
      position: relative;
    }
    .day:hover {
      background-color: #ffc1d8;
    }
    .modal {
      position: fixed;
      top: 0; left: 0; right: 0; bottom: 0;
      background-color: rgba(0,0,0,0.5);
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 10;
    }
    .modal-content {
      background-color: white;
      padding: 1rem;
      border-radius: 10px;
      max-width: 300px;
      width: 90%;
      box-shadow: 0 2px 10px rgba(0,0,0,0.2);
    }
    input, textarea {
      width: 100%;
      margin-top: 0.5rem;
      padding: 0.5rem;
      font-size: 1rem;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
    button {
      margin-top: 1rem;
      background-color: #ff80ab;
      color: white;
      border: none;
      padding: 0.5rem 1rem;
      font-size: 1rem;
      border-radius: 5px;
      cursor: pointer;
    }
    .memory-icon {
      position: absolute;
      top: 5px;
      right: 5px;
      font-size: 1.2rem;
      color: #d81b60;
    }
  </style>
</head>
<body>
  <header>Nasze Chwile</header>
  <div class="calendar" id="calendar"></div>

  <div class="modal" id="modal" style="display: none;">
    <div class="modal-content">
      <h3 id="modal-date">Dodaj wspomnienie</h3>
      <input type="text" id="memoryTitle" placeholder="Tytuł...">
      <textarea id="memoryText" rows="4" placeholder="Opis wspomnienia..."></textarea>
      <button onclick="saveMemory()">Zapisz</button>
    </div>
  </div>

  <script>
    const calendar = document.getElementById('calendar');
    const modal = document.getElementById('modal');
    const modalDate = document.getElementById('modal-date');
    const memoryTitle = document.getElementById('memoryTitle');
    const memoryText = document.getElementById('memoryText');

    let selectedDate = '';

    function generateCalendar() {
      const now = new Date();
      const year = now.getFullYear();
      const month = now.getMonth();
      const firstDay = new Date(year, month, 1).getDay();
      const lastDate = new Date(year, month + 1, 0).getDate();

      calendar.innerHTML = '';

      for (let i = 0; i < firstDay; i++) {
        const empty = document.createElement('div');
        calendar.appendChild(empty);
      }

      for (let day = 1; day <= lastDate; day++) {
        const div = document.createElement('div');
        div.className = 'day';
        div.textContent = day;
        const key = `${year}-${month + 1}-${day}`;
        if (localStorage.getItem(key)) {
          div.innerHTML += '<span class="memory-icon">❤️</span>';
        }
        div.onclick = () => openModal(key);
        calendar.appendChild(div);
      }
    }

    function openModal(dateKey) {
      selectedDate = dateKey;
      modal.style.display = 'flex';
      modalDate.textContent = `Dodaj wspomnienie: ${dateKey}`;
      const data = JSON.parse(localStorage.getItem(dateKey) || '{}');
      memoryTitle.value = data.title || '';
      memoryText.value = data.text || '';
    }

    function saveMemory() {
      const data = {
        title: memoryTitle.value,
        text: memoryText.value
      };
      localStorage.setItem(selectedDate, JSON.stringify(data));
      modal.style.display = 'none';
      generateCalendar();
    }

    window.onclick = function(e) {
      if (e.target === modal) {
        modal.style.display = 'none';
      }
    }

    generateCalendar();
  </script>
</body>
</html>