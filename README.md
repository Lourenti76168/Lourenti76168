<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Registro de Abastecimento - Carros e Motos</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 20px;
        }
        .container {
            max-width: 600px;
            margin: auto;
            background: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        h1 {
            text-align: center;
            color: #333;
        }
        label {
            display: block;
            margin-top: 10px;
            color: #555;
        }
        input[type="text"],
        input[type="number"],
        input[type="date"],
        select {
            width: 100%;
            padding: 10px;
            margin-top: 5px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        button {
            display: block;
            width: 100%;
            padding: 10px;
            margin-top: 20px;
            background: #5cb85c;
            color: #fff;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
        button:hover {
            background: #4cae4c;
        }
        .records-container {
            display: flex;
            justify-content: space-between;
            margin-top: 20px;
        }
        .records-section {
            width: 48%;
            background: #f9f9f9;
            padding: 10px;
            border-radius: 8px;
        }
        .record {
            background: #e9e9e9;
            padding: 10px;
            border-radius: 4px;
            margin-top: 10px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Registro de Abastecimento - Carros e Motos</h1>
        <form id="abastecimentoForm">
            <label for="vehicleType">Tipo de Veículo:</label>
            <select id="vehicleType" name="vehicleType" required>
                <option value="carro">Carro</option>
                <option value="moto">Moto</option>
            </select>

            <label for="date">Data:</label>
            <input type="date" id="date" name="date" required>

            <label for="stationName">Nome do Posto:</label>
            <input type="text" id="stationName" name="stationName" required>

            <label for="liters">Litros Abastecidos:</label>
            <input type="number" id="liters" name="liters" step="0.01" required>

            <label for="pricePerLiter">Preço por Litro:</label>
            <input type="number" id="pricePerLiter" name="pricePerLiter" step="0.01" required>

            <label for="km">Quilometragem:</label>
            <input type="number" id="km" name="km" required>

            <button type="submit">Registrar</button>
        </form>

        <div class="records-container">
            <div class="records-section">
                <h2>Dados Registrados - Carros:</h2>
                <div id="registrosCarros"></div>
            </div>
            <div class="records-section">
                <h2>Dados Registrados - Motos:</h2>
                <div id="registrosMotos"></div>
            </div>
        </div>
    </div>

    <script>
        document.getElementById('abastecimentoForm').addEventListener('submit', function(event) {
            event.preventDefault();
            const vehicleType = document.getElementById('vehicleType').value;
            const date = document.getElementById('date').value;
            const stationName = document.getElementById('stationName').value;
            const liters = document.getElementById('liters').value;
            const pricePerLiter = document.getElementById('pricePerLiter').value;
            const km = document.getElementById('km').value;

            const abastecimento = {
                date: date,
                stationName: stationName,
                liters: parseFloat(liters),
                pricePerLiter: parseFloat(pricePerLiter),
                km: parseInt(km)
            };

            if (vehicleType === 'carro') {
                let abastecimentosCarros = JSON.parse(localStorage.getItem('abastecimentosCarros')) || [];
                
                if (abastecimentosCarros.length > 0) {
                    const lastAbastecimento = abastecimentosCarros[abastecimentosCarros.length - 1];
                    const kmDriven = abastecimento.km - lastAbastecimento.km;
                    const average = kmDriven / lastAbastecimento.liters;
                    abastecimento.average = average.toFixed(2);
                } else {
                    abastecimento.average = 'N/A';
                }

                abastecimentosCarros.push(abastecimento);
                localStorage.setItem('abastecimentosCarros', JSON.stringify(abastecimentosCarros));
            } else if (vehicleType === 'moto') {
                let abastecimentosMotos = JSON.parse(localStorage.getItem('abastecimentosMotos')) || [];
                
                if (abastecimentosMotos.length > 0) {
                    const lastAbastecimento = abastecimentosMotos[abastecimentosMotos.length - 1];
                    const kmDriven = abastecimento.km - lastAbastecimento.km;
                    const average = kmDriven / lastAbastecimento.liters;
                    abastecimento.average = average.toFixed(2);
                } else {
                    abastecimento.average = 'N/A';
                }

                abastecimentosMotos.push(abastecimento);
                localStorage.setItem('abastecimentosMotos', JSON.stringify(abastecimentosMotos));
            }

            document.getElementById('abastecimentoForm').reset();
            exibirRegistros();
        });

        function exibirRegistros() {
            const registrosCarrosDiv = document.getElementById('registrosCarros');
            const registrosMotosDiv = document.getElementById('registrosMotos');
            registrosCarrosDiv.innerHTML = '';
            registrosMotosDiv.innerHTML = '';

            let abastecimentosCarros = JSON.parse(localStorage.getItem('abastecimentosCarros')) || [];
            abastecimentosCarros.forEach(abastecimento => {
                const registroDiv = document.createElement('div');
                registroDiv.classList.add('record');
                registroDiv.innerHTML = `
                    <p>Data: ${abastecimento.date}</p>
                    <p>Nome do Posto: ${abastecimento.stationName}</p>
                    <p>Litros Abastecidos: ${abastecimento.liters} L</p>
                    <p>Preço por Litro: R$ ${abastecimento.pricePerLiter}</p>
                    <p>Quilometragem: ${abastecimento.km} km</p>
                    <p>Média do Último Abastecimento: ${abastecimento.average} km/L</p>
                `;
                registrosCarrosDiv.appendChild(registroDiv);
            });

            let abastecimentosMotos = JSON.parse(localStorage.getItem('abastecimentosMotos')) || [];
            abastecimentosMotos.forEach(abastecimento => {
                const registroDiv = document.createElement('div');
                registroDiv.classList.add('record');
                registroDiv.innerHTML = `
                    <p>Data: ${abastecimento.date}</p>
                    <p>Nome do Posto: ${abastecimento.stationName}</p>
                    <p>Litros Abastecidos: ${abastecimento.liters} L</p>
                    <p>Preço por Litro: R$ ${abastecimento.pricePerLiter}</p>
                    <p>Quilometragem: ${abastecimento.km} km</p>
                    <p>Média do Último Abastecimento: ${abastecimento.average} km/L</p>
                `;
                registrosMotosDiv.appendChild(registroDiv);
            });
        }

        // Exibir registros ao carregar a página
        document.addEventListener('DOMContentLoaded', exibirRegistros);
    </script>
</body>
</html>
