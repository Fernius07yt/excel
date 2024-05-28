### README.md

# Excel to Sortable Table Web Application

This web application allows you to upload an Excel file and display its data in a sortable HTML table. The application uses SheetJS to read the Excel file and DataTables to provide sorting and other table functionalities.

## Features

- Upload an Excel file (`.xlsx` or `.xls`).
- Display the contents of the Excel file in an HTML table.
- Sort the table by clicking on the column headers.

## Usage

You can use the application directly on the website without downloading anything.

**Visit the application here: [Excel to Sortable Table](https://fernius07yt.github.io/excel/)**

### Steps:

1. Open the [web application](https://fernius07yt.github.io/excel/).
2. Click on the file input to select and upload an Excel file (`.xlsx` or `.xls`).
3. The table will be generated automatically, and you can sort the data by clicking on the column headers.

### Code Overview

#### HTML (`index.html`)

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Excel a Tabla</title>
    <link rel="stylesheet" href="https://cdn.datatables.net/1.11.5/css/jquery.dataTables.min.css">
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        #fileInput {
            margin-bottom: 20px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
        }
        th, td {
            padding: 8px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
    </style>
</head>
<body>
    <h1>Subir y Ordenar Tabla de Excel</h1>
    <input type="file" id="fileInput" accept=".xlsx, .xls">
    <table id="excelTable" class="display">
        <thead>
            <tr id="tableHeader"></tr>
        </thead>
        <tbody id="tableBody"></tbody>
    </table>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <script src="https://cdn.datatables.net/1.11.5/js/jquery.dataTables.min.js"></script>
    <script src="script.js"></script>
</body>
</html>
```

#### JavaScript (`script.js`)

```javascript
document.getElementById('fileInput').addEventListener('change', handleFile, false);

function handleFile(event) {
    const file = event.target.files[0];
    const reader = new FileReader();

    reader.onload = function(event) {
        const data = new Uint8Array(event.target.result);
        const workbook = XLSX.read(data, { type: 'array' });
        const firstSheetName = workbook.SheetNames[0];
        const worksheet = workbook.Sheets[firstSheetName];
        const json = XLSX.utils.sheet_to_json(worksheet, { header: 1 });
        generateTable(json);
    };

    reader.readAsArrayBuffer(file);
}

function generateTable(data) {
    const tableHeader = document.getElementById('tableHeader');
    const tableBody = document.getElementById('tableBody');

    tableHeader.innerHTML = '';
    tableBody.innerHTML = '';

    if (data.length === 0) return;

    // Generar encabezado de la tabla
    const headerRow = data[0];
    headerRow.forEach(header => {
        const th = document.createElement('th');
        th.textContent = header;
        tableHeader.appendChild(th);
    });

    // Generar cuerpo de la tabla
    data.slice(1).forEach(row => {
        const tr = document.createElement('tr');
        row.forEach(cell => {
            const td = document.createElement('td');
            td.textContent = cell;
            tr.appendChild(td);
        });
        tableBody.appendChild(tr);
    });

    // Inicializar DataTables
    $('#excelTable').DataTable();
}
```

### Dependencies

The project uses the following external libraries:

- [SheetJS (xlsx.js)](https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js)
- [DataTables](https://cdn.datatables.net/1.11.5/js/jquery.dataTables.min.js)
- [jQuery](https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js)

### Custom Styles

If you want to add custom styles, you can create a `styles.css` file and include it in the `index.html` file. Modify the CSS according to your design preferences.

### License

This project is licensed under the MIT License. Feel free to use and modify the code as needed.

### Acknowledgments

- [SheetJS](https://sheetjs.com/)
- [DataTables](https://datatables.net/)
- [jQuery](https://jquery.com/)

Enjoy using the application! If you encounter any issues or have suggestions for improvements, feel free to open an issue or submit a pull request.
