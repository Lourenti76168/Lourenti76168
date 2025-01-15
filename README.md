<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Planilha de Abastecimento</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
        }
        table, th, td {
            border: 1px solid #ccc;
        }
        th, td {
            padding: 8px;
            text-align: left;
        }
        th {
            background-color: #f4f4f4;
        }
        .add-row-btn {
            padding: 10px 15px;
            background-color: #28a745;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        .add-row-btn:hover {
            background-color: #218838;
        }
    </style>
</head>
<body>
    <h1>Planilha de Abastecimento</h1>
    <table id="abastecimentoTable">
        <thead>
            <tr>
                <th>Data</th>
                <th>Odômetro (km)</th>
                <th>Litros abastecidos</th>
                <th>Preço por litro (R$)</th>
                <th>Valor total (R$)</th>
                <th>Km rodados</th>
                <th>Consumo (km/L)</th>
                <th>Posto</th>
                <th>Observações</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td><input type="date"></td>
                <td><input type="number" step="0.1" placeholder="0" class="odometro"></td>
                <td><input type="number" step="0.1" placeholder="0" class="litros"></td>
                <td><input type="number" step="0.01" placeholder="0" class="preco"></td>
                <td><input type="number" step="0.01" placeholder="0" class="valor-total" readonly></td>
                <td><input type="number" step="0.1" placeholder="0" class="km-rodados" readonly></td>
                <td><input type="number" step="0.1" placeholder="0" class="consumo" readonly></td>
                <td><input type="text" placeholder="Posto"></td>
                <td><input type="text" placeholder="Observações"></td>
            </tr>
        </tbody>
    </table>
    <button class="add-row-btn" onclick="addRow()">Adicionar Linha</button>

    <script>
        function addRow() {
            const table = document.getElementById('abastecimentoTable').getElementsByTagName('tbody')[0];
            const newRow = table.insertRow();

            const columns = [
                '<input type="date">',
                '<input type="number" step="0.1" placeholder="0" class="odometro">',
                '<input type="number" step="0.1" placeholder="0" class="litros">',
                '<input type="number" step="0.01" placeholder="0" class="preco">',
                '<input type="number" step="0.01" placeholder="0" class="valor-total" readonly>',
                '<input type="number" step="0.1" placeholder="0" class="km-rodados" readonly>',
                '<input type="number" step="0.1" placeholder="0" class="consumo" readonly>',
                '<input type="text" placeholder="Posto">',
                '<input type="text" placeholder="Observações">'
            ];

            columns.forEach(col => {
                const cell = newRow.insertCell();
                cell.innerHTML = col;
            });
        }

        document.addEventListener('input', function (event) {
            const row = event.target.closest('tr');
            if (!row) return;

            const odometro = row.querySelector('.odometro');
            const litros = row.querySelector('.litros');
            const preco = row.querySelector('.preco');
            const valorTotal = row.querySelector('.valor-total');
            const kmRodados = row.querySelector('.km-rodados');
            const consumo = row.querySelector('.consumo');

            // Atualizar Valor Total
            if (litros && preco && valorTotal) {
                const total = parseFloat(litros.value || 0) * parseFloat(preco.value || 0);
                valorTotal.value = total.toFixed(2);
            }

            // Atualizar Km Rodados
            const rows = document.querySelectorAll('#abastecimentoTable tbody tr');
            rows.forEach((currentRow, index) => {
                if (currentRow === row && index > 0) {
                    const previousRow = rows[index - 1];
                    const previousOdometro = previousRow.querySelector('.odometro');
                    if (previousOdometro && odometro) {
                        const km = parseFloat(odometro.value || 0) - parseFloat(previousOdometro.value || 0);
                        kmRodados.value = km > 0 ? km.toFixed(1) : '';
                    }
                }
            });

            // Atualizar Consumo
            if (litros && kmRodados && consumo) {
                const km = parseFloat(kmRodados.value || 0);
                const liters = parseFloat(litros.value || 0);
                if (km > 0 && liters > 0) {
                    consumo.value = (km / liters).toFixed(1);
                } else {
                    consumo.value = '';
                }
            }
        });
    </script>
<button onclick="salvarDados()">Salvar Dados</button>
</body>
</html>
<!---
Lourenti76168/Lourenti76168 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
