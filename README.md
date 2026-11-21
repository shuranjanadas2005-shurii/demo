# demo
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Simple To-Do App</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }
    body {
      background-color: #f4f6f8;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      padding: 20px;
    }
    .container {
      background: #ffffff;
      width: 100%;
      max-width: 400px;
      padding: 25px;
      border-radius: 12px;
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
    }
    h2 {
      text-align: center;
      color: #333;
      margin-bottom: 20px;
    }
    .input-group {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
    }
    input[type="text"] {
      flex: 1;
      padding: 10px 14px;
      border: 1px solid #ccc;
      border-radius: 6px;
      font-size: 16px;
      outline: none;
    }
    input[type="text"]:focus {
      border-color: #007bff;
    }
    button.add-btn {
      padding: 10px 18px;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 6px;
      font-size: 16px;
      cursor: pointer;
    }
    button.add-btn:hover {
      background-color: #0056b3;
    }
    ul {
      list-style: none;
    }
    li {
      background: #f9fafb;
      padding: 12px;
      border-radius: 6px;
      margin-bottom: 10px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      border: 1px solid #eee;
    }
    li.completed span {
      text-decoration: line-through;
      color: #888;
    }
    li span {
      cursor: pointer;
      flex: 1;
    }
    button.delete-btn {
      background: #ff4d4d;
      color: white;
      border: none;
      padding: 6px 10px;
      border-radius: 4px;
      cursor: pointer;
      margin-left: 10px;
    }
    button.delete-btn:hover {
      background: #cc0000;
    }
  </style>
</head>
<body>

  <div class="container">
    <h2>My To-Do List</h2>
    <div class="input-group">
      <input type="text" id="taskInput" placeholder="Add a new task...">
      <button class="add-btn" onclick="addTask()">Add</button>
    </div>
    <ul id="taskList"></ul>
  </div>

  <script>
    const taskInput = document.getElementById('taskInput');
    const taskList = document.getElementById('taskList');

    // Load saved tasks on startup
    document.addEventListener('DOMContentLoaded', loadTasks);

    // Allow pressing "Enter" to add a task
    taskInput.addEventListener('keypress', function (e) {
      if (e.key === 'Enter') addTask();
    });

    function addTask() {
      const text = taskInput.value.trim();
      if (text === '') return;

      createTaskElement(text, false);
      saveTasks();
      taskInput.value = '';
    }

    function createTaskElement(text, isCompleted) {
      const li = document.createElement('li');
      if (isCompleted) li.classList.add('completed');

      const span = document.createElement('span');
      span.textContent = text;
      span.onclick = () => {
        li.classList.toggle('completed');
        saveTasks();
      };

      const delBtn = document.createElement('button');
      delBtn.textContent = 'Delete';
      delBtn.className = 'delete-btn';
      delBtn.onclick = () => {
        li.remove();
        saveTasks();
      };

      li.appendChild(span);
      li.appendChild(delBtn);
      taskList.appendChild(li);
    }

    function saveTasks() {
      const tasks = [];
      document.querySelectorAll('#taskList li').forEach(li => {
        tasks.push({
          text: li.querySelector('span').textContent,
          completed: li.classList.contains('completed')
        });
      });
      localStorage.setItem('myTasks', JSON.stringify(tasks));
    }

    function loadTasks() {
      const saved = JSON.parse(localStorage.getItem('myTasks')) || [];
      saved.forEach(task => createTaskElement(task.text, task.completed));
    }
  </script>

</body>
</html>
