<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <title>军衔评分计算器</title>
  <style>
    body {
      font-family: sans-serif;
      background: #f4f4f4;
      padding: 2rem;
    }
    h2 {
      color: #333;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 1rem;
      background: #fff;
    }
    th, td {
      border: 1px solid #ccc;
      padding: 0.5rem;
      text-align: center;
    }
    input {
      padding: 0.3rem;
      margin-right: 0.5rem;
    }
    button {
      padding: 0.4rem 1rem;
      margin-top: 1rem;
      font-size: 15px;
    }
    .input-row {
      margin-bottom: 1rem;
    }
  </style>
</head>
<body>
  <h2>军衔评分计算器（支持多人）</h2>

  <div class="input-row">
    姓名：<input type="text" id="name" placeholder="请输入姓名" />
    段位（1~25）：<input type="number" id="rank" min="1" max="25" />
    活跃度（1~25）：<input type="number" id="activity" min="1" max="25" />
    <button onclick="addPerson()">添加</button>
  </div>

  <table id="resultTable">
    <thead>
      <tr>
        <th>姓名</th>
        <th>段位</th>
        <th>活跃度</th>
        <th>平均分</th>
        <th>军衔</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <script>
    const data = [];

    function getRankName(score) {
      if (score <= 3) return "元帅";
      if (score <= 6) return "中将";
      if (score <= 10) return "上校";
      if (score <= 15) return "总军士长";
      if (score <= 20) return "士官长";
      return "上等兵";
    }

    function addPerson() {
      const name = document.getElementById("name").value.trim();
      const rank = parseInt(document.getElementById("rank").value);
      const activity = parseInt(document.getElementById("activity").value);

      if (!name || isNaN(rank) || isNaN(activity) || rank < 1 || rank > 25 || activity < 1 || activity > 25) {
        alert("请填写正确的信息！");
        return;
      }

      const avg = (rank + activity) / 2;
      const level = getRankName(avg);

      data.push({ name, rank, activity, avg, level });

      // 清空输入框
      document.getElementById("name").value = "";
      document.getElementById("rank").value = "";
      document.getElementById("activity").value = "";

      updateTable();
    }

    function updateTable() {
      // 按平均分升序排序（越小越强）
      data.sort((a, b) => a.avg - b.avg);

      const tbody = document.querySelector("#resultTable tbody");
      tbody.innerHTML = "";

      data.forEach(item => {
        const row = document.createElement("tr");
        row.innerHTML = `
          <td>${item.name}</td>
          <td>${item.rank}</td>
          <td>${item.activity}</td>
          <td>${item.avg.toFixed(2)}</td>
          <td>${item.level}</td>
        `;
        tbody.appendChild(row);
      });
    }
  </script>
</body>
</html>
