<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Логи | CRMP</title>
  <style>
    body {
      background-color: #101010;
      color: #fff;
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 20px;
    }
    .container {
      max-width: 1000px;
      margin: auto;
    }
    h1 {
      text-align: center;
    }
    input, select, button {
      padding: 10px;
      margin: 5px 0;
      width: 100%;
      background: #1c1c1c;
      color: white;
      border: 1px solid #333;
    }
    .logs, .admin-panel {
      background: #1a1a1a;
      padding: 20px;
      margin-top: 20px;
      border-radius: 10px;
    }
    .log-entry {
      border-bottom: 1px solid #333;
      padding: 10px 0;
    }
    .hidden {
      display: none;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Система логов CRMP</h1>

    <div>
      <label>Введите ник:</label>
      <input type="text" id="searchNick" placeholder="Например: Mher">
      <button onclick="searchLogs()">Найти логи</button>
    </div>

    <div class="logs" id="logResults">
      <!-- Логи будут тут -->
    </div>

    <!-- Панель администратора -->
    <div class="admin-panel hidden" id="adminPanel">
      <h2>Админ-панель</h2>
      <p>Добро пожаловать, <b id="adminName"></b></p>

      <div>
        <label>Ник игрока:</label>
        <input type="text" id="targetUser" placeholder="Введите ник">

        <label>Выдать уровень админа:</label>
        <select id="adminLevel">
          <option value="1">Уровень 1</option>
          <option value="2">Уровень 2</option>
          <option value="3">Уровень 3</option>
          <option value="4">Уровень 4</option>
        </select>
        <button onclick="giveAdmin()">Выдать админку</button>

        <label>Мут:</label>
        <button onclick="muteUser()">Выдать мут (1-2 лвл)</button>

        <label>Бан:</label>
        <button onclick="banUser()">Выдать бан (только 4 лвл)</button>
      </div>
    </div>
  </div>

  <script>
    // Имитируем логин
    const currentUser = prompt("Введите ваше имя:");
    const currentAdminLevel = currentUser === "Mher" ? 4 : 0;
    document.getElementById("adminName").textContent = currentUser;

    if (currentAdminLevel > 0) {
      document.getElementById("adminPanel").classList.remove("hidden");
    }

    const logsDatabase = {
      "Mher": [
        { type: "money", text: "Пополнение 10000$" },
        { type: "ban", text: "Бан за читы 2024-04-01" }
      ],
      "Armen": [
        { type: "money", text: "Снятие 5000$" },
        { type: "ban", text: "Бан за DM 2024-03-22" }
      ]
    };

    function searchLogs() {
      const nick = document.getElementById("searchNick").value;
      const logs = logsDatabase[nick] || [];

      const logContainer = document.getElementById("logResults");
      logContainer.innerHTML = `<h3>Логи для ${nick}:</h3>`;
      if (logs.length === 0) {
        logContainer.innerHTML += `<p>Нет логов</p>`;
        return;
      }

      logs.forEach(log => {
        logContainer.innerHTML += `<div class="log-entry"><b>${log.type.toUpperCase()}:</b> ${log.text}</div>`;
      });
    }

    function giveAdmin() {
      if (currentAdminLevel === 4) {
        const target = document.getElementById("targetUser").value;
        const level = document.getElementById("adminLevel").value;
        alert(`Выдан уровень администратора ${level} игроку ${target}`);
      } else {
        alert("Недостаточно прав для выдачи админки!");
      }
    }

    function muteUser() {
      if (currentAdminLevel >= 1 && currentAdminLevel <= 2) {
        const target = document.getElementById("targetUser").value;
        alert(`Игрок ${target} получил мут`);
      } else {
        alert("Вы не можете выдать мут. Требуется админ 1-2 уровня.");
      }
    }

    function banUser() {
      if (currentAdminLevel === 4) {
        const target = document.getElementById("targetUser").value;
        alert(`Игрок ${target} забанен`);
      } else {
        alert("Только админ 4 уровня может банить игроков.");
      }
    }
  </script>
</body>
</html>
