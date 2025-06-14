<!-- fichier: index.html -->
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Gestion des étudiants</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
    }
    h1 {
      text-align: center;
    }
    form, table {
      margin: auto;
      width: 80%;
      max-width: 700px;
    }
    table {
      border-collapse: collapse;
      margin-top: 30px;
    }
    table, th, td {
      border: 1px solid #aaa;
    }
    th, td {
      padding: 8px;
      text-align: center;
    }
    input[type="text"], input[type="email"] {
      width: 100%;
      padding: 6px;
      margin: 5px 0;
    }
    .actions button {
      margin: 2px;
    }
  </style>
</head>
<body>
  <h1>CRUD Étudiant</h1>
  <form id="studentForm">
    <input type="hidden" id="studentId">
    <label>Nom : <input type="text" id="nom" required></label><br>
    <label>Prénom : <input type="text" id="prenom" required></label><br>
    <label>Email : <input type="email" id="email" required></label><br>
    <button type="submit">Enregistrer</button>
  </form>

  <table>
    <thead>
      <tr>
        <th>ID</th>
        <th>Nom</th>
        <th>Prénom</th>
        <th>Email</th>
        <th>Actions</th>
      </tr>
    </thead>
    <tbody id="studentTableBody">
      <!-- Les étudiants s'afficheront ici -->
    </tbody>
  </table>

  <script>
    let students = [];
    let editing = false;

    const form = document.getElementById("studentForm");
    const tableBody = document.getElementById("studentTableBody");

    form.addEventListener("submit", function (e) {
      e.preventDefault();
      const nom = document.getElementById("nom").value;
      const prenom = document.getElementById("prenom").value;
      const email = document.getElementById("email").value;
      const id = document.getElementById("studentId").value;

      if (editing) {
        const index = students.findIndex(s => s.id == id);
        students[index] = { id: Number(id), nom, prenom, email };
        editing = false;
      } else {
        students.push({ id: Date.now(), nom, prenom, email });
      }

      form.reset();
      renderTable();
    });

    function renderTable() {
      tableBody.innerHTML = "";
      students.forEach(s => {
        const row = `<tr>
          <td>${s.id}</td>
          <td>${s.nom}</td>
          <td>${s.prenom}</td>
          <td>${s.email}</td>
          <td class="actions">
            <button onclick="editStudent(${s.id})">Modifier</button>
            <button onclick="deleteStudent(${s.id})">Supprimer</button>
          </td>
        </tr>`;
        tableBody.innerHTML += row;
      });
    }

    function editStudent(id) {
      const student = students.find(s => s.id === id);
      document.getElementById("studentId").value = student.id;
      document.getElementById("nom").value = student.nom;
      document.getElementById("prenom").value = student.prenom;
      document.getElementById("email").value = student.email;
      editing = true;
    }

    function deleteStudent(id) {
      students = students.filter(s => s.id !== id);
      renderTable();
    }
  </script>
</body>
</html>
