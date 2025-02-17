
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fayadhowr Cleaning Company</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <style>
        /* Header Style */
        header {
            background-color: #0A2342; /* Dark blue */
            color: white;
            display: flex;
            align-items: center;
            padding: 15px;
        }

        header img {
            width: 50px;
            margin-right: 15px;
        }

        /* Form Section */
        .form-section {
            background: #f4f4f4;
            padding: 20px;
            border-radius: 8px;
            width: 50%;
            margin: 20px auto;
            transition: transform 0.3s ease-in-out;
            border: 2px solid #0A2342;
        }

        /* Form Hover Effect */
        .form-section:hover {
            transform: scale(1.05);
            box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.2);
        }

        .form-group {
            margin-bottom: 15px;
        }

        .form-group label {
            display: block;
            font-weight: bold;
        }

        .form-group input, .form-group select {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }

        /* Button Style */
        .rotate-btn {
            background-color: #0A2342;
            color: white;
            border: none;
            padding: 10px 20px;
            cursor: pointer;
            border-radius: 5px;
            transition: transform 0.3s ease-in-out;
        }

        .rotate-btn:hover {
            transform: rotate(5deg);
            background-color: #082036;
        }

        /* Table Section */
        .table-section {
            width: 80%;
            margin: 20px auto;
            border-collapse: collapse;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            background: white;
            box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
        }

        table, th, td {
            border: 1px solid #ddd;
        }

        th, td {
            padding: 10px;
            text-align: left;
        }

        th {
            background-color: #0A2342;
            color: white;
        }

        /* Search Section */
        .search-section {
            width: 50%;
            margin: 20px auto;
            text-align: center;
        }

        .search-section input {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }

        /* Footer */
        footer {
            text-align: center;
            margin-top: 20px;
        }

        footer a img {
            width: 30px;
            margin: 5px;
        }

        /* Hide search section when printing */
        @media print {
            .search-section {
                display: none;
            }
            .actions {
                display: none; /* Hide buttons during printing */
            }
            table td:last-child, table td:nth-last-child(2) {
                display: none; /* Hide Delete and Undo buttons during printing */
            }
        }
    </style>
</head>
<body>
    <header>
        <img src="faydhowr.png" alt="Logo">
        <h1>Fayadhowr Cleaning Company</h1>
    </header>

    <section class="form-section">
        <form id="task-form">
            <div class="form-group">
                <label for="name">Magaca:</label>
                <input type="text" id="name" placeholder="Magaca" required>
            </div>
            <div class="form-group">
                <label for="date">Taariikhda:</label>
                <input type="date" id="date" required>
            </div>
            <div class="form-group">
                <label for="task">Shaqada:</label>
                <select id="task">
                    <option value="Jornati">Jornati</option>
                    <option value="Baraariko">Baraariko</option>
                    <option value="Mohan">Mohan</option>
                </select>
            </div>
            <div class="form-group">
                <label for="location">W. laqabanyo:</label>
                <select id="location">
                    <option value="Xarun">Xarun</option>
                    <option value="Project">Project</option>
                    <option value="Nalfar">Nalfar</option>
                </select>
            </div>
            <button type="submit" class="rotate-btn">Save</button>
        </form>
    </section>

    <section class="search-section">
        <label for="search">Raadi Magaca:</label>
        <input type="text" id="search" placeholder="Raadi magaca..." onkeyup="searchTable()">
    </section>

    <section class="table-section">
        <table>
            <thead>
                <tr>
                    <th>No.</th>
                    <th>Magaca</th>
                    <th>Taariikhda</th>
                    <th>Shaqada</th>
                    <th>W. laqabanyo</th>
                    <th>Delete</th>
                    <th>Undo</th>
                </tr>
            </thead>
            <tbody id="task-table"></tbody>
        </table>
    </section>

    <section class="actions">
        <button onclick="printTable()" class="rotate-btn">Print</button>
        <button onclick="downloadPDF()" class="rotate-btn">Download PDF</button>
    </section>

    <footer>
        <a href="#"><img src="email.webp" alt="Gmail"></a>
        <a href="#"><img src="whatsapp (1).png" alt="WhatsApp"></a>
        <a href="#"><img src="facebook.png" alt="Facebook"></a>
    </footer>

    <script>
        let deletedTask = null; // Store deleted task for undo

        // Handle task form submission
        document.getElementById("task-form").addEventListener("submit", function(event) {
            event.preventDefault();

            const name = document.getElementById("name").value;
            const date = document.getElementById("date").value;
            const task = document.getElementById("task").value;
            const location = document.getElementById("location").value;

            const taskData = { name, date, task, location };

            // Get existing tasks from localStorage or initialize an empty array
            let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

            // Add the new task
            tasks.push(taskData);

            // Save the updated tasks array to localStorage
            localStorage.setItem("tasks", JSON.stringify(tasks));

            // Update the table
            updateTable();

            // Reset the form
            document.getElementById("task-form").reset();
        });

        function updateTable() {
            const table = document.getElementById("task-table");
            table.innerHTML = ""; // Clear existing table rows

            // Get tasks from localStorage
            const tasks = JSON.parse(localStorage.getItem("tasks")) || [];

            // Populate the table with saved tasks
            tasks.forEach((task, index) => {
                const row = table.insertRow();
                row.innerHTML = `
                    <td>${index + 1}</td>
                    <td>${task.name}</td>
                    <td>${task.date}</td>
                    <td>${task.task}</td>
                    <td>${task.location}</td>
                    <td><button onclick="deleteRow(${index})">Delete</button></td>
                    <td><button onclick="undoDelete()">Undo</button></td>
                `;
            });
        }

        function deleteRow(index) {
            // Get tasks from localStorage
            let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

            // Store the deleted task for undo
            deletedTask = tasks.splice(index, 1)[0];

            // Save the updated tasks array back to localStorage
            localStorage.setItem("tasks", JSON.stringify(tasks));

            // Update the table
            updateTable();
        }

        function undoDelete() {
            if (deletedTask) {
                // Get tasks from localStorage
                let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

                // Restore the deleted task
                tasks.push(deletedTask);

                // Save the updated tasks array back to localStorage
                localStorage.setItem("tasks", JSON.stringify(tasks));

                // Reset the deleted task
                deletedTask = null;

                // Update the table
                updateTable();
            }
        }

        // Initial table population when page loads
        window.onload = function() {
            updateTable();
        };

        // Search functionality
        function searchTable() {
            const searchInput = document.getElementById("search").value.toLowerCase();
            const tableRows = document.getElementById("task-table").rows;

            for (let i = 0; i < tableRows.length; i++) {
                const nameCell = tableRows[i].cells[1]; // Name column
                const nameText = nameCell.textContent || nameCell.innerText;

                if (nameText.toLowerCase().indexOf(searchInput) > -1) {
                    tableRows[i].style.display = "";
                } else {
                    tableRows[i].style.display = "none";
                }
            }
        }

        // Exclude form and actions from printing
        function printTable() {
            // Hide the form and other elements that shouldn't be printed
            document.querySelector(".form-section").style.display = "none";
            document.querySelector(".actions").style.display = "none";
            document.querySelector(".search-section").style.display = "none"; // Hide the search bar

            // Print the page content
            window.print();

            // After printing, restore the hidden elements
            document.querySelector(".form-section").style.display = "block";
            document.querySelector(".actions").style.display = "block";
            document.querySelector(".search-section").style.display = "block";
        }

        // Download PDF without form data
        function downloadPDF() {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();
            const table = document.getElementById("task-table");
            let data = [];

            // Loop through the rows and gather the data
            for (let i = 0; i < table.rows.length; i++) {
                let rowData = [];
                for (let j = 0; j < table.rows[i].cells.length - 2; j++) { // Skip the "Delete" and "Undo" columns
                    rowData.push(table.rows[i].cells[j].textContent);
                }
                data.push(rowData);
            }

            // Create the PDF with the data
            doc.autoTable({
                head: [['No.', 'Magaca', 'Taariikhda', 'Shaqada', 'W. laqabanyo']],
                body: data,
            });

            // Add the footer (icons) if needed
            doc.text('Contact us: Gmail, WhatsApp, Facebook', 10, doc.internal.pageSize.height - 10);

            // Save the PDF
            doc.save('task-table.pdf');
        }
    </script>

