<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Gestión Dental</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            padding: 30px;
            text-align: center;
            color: white;
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            font-weight: 300;
        }

        .date-display {
            font-size: 1.2em;
            opacity: 0.9;
        }

        .main-content {
            padding: 30px;
        }

        .dashboard {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 40px;
        }

        .stat-card {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            padding: 25px;
            border-radius: 15px;
            color: white;
            text-align: center;
            transform: translateY(0);
            transition: transform 0.3s ease;
        }

        .stat-card:hover {
            transform: translateY(-5px);
        }

        .stat-card h3 {
            font-size: 0.9em;
            margin-bottom: 10px;
            opacity: 0.9;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .stat-card .number {
            font-size: 2.5em;
            font-weight: bold;
        }

        .sections {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
        }

        .section {
            background: #f8f9fa;
            border-radius: 15px;
            padding: 25px;
        }

        .section h2 {
            color: #333;
            margin-bottom: 20px;
            font-size: 1.5em;
            border-bottom: 3px solid #4facfe;
            padding-bottom: 10px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            color: #555;
            font-weight: 500;
        }

        input, select, textarea {
            width: 100%;
            padding: 12px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            font-size: 16px;
            transition: border-color 0.3s ease;
        }

        input:focus, select:focus, textarea:focus {
            outline: none;
            border-color: #4facfe;
            box-shadow: 0 0 0 3px rgba(79, 172, 254, 0.1);
        }

        button {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 25px;
            font-size: 16px;
            cursor: pointer;
            transition: transform 0.3s ease;
            font-weight: 500;
        }

        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(79, 172, 254, 0.4);
        }

        .patient-list {
            margin-top: 20px;
        }

        .patient-item {
            background: white;
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 10px;
            border-left: 4px solid #4facfe;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }

        .patient-info {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .patient-name {
            font-weight: bold;
            color: #333;
        }

        .patient-treatment {
            color: #666;
            font-size: 0.9em;
        }

        .patient-payment {
            background: #e8f5e8;
            color: #2e7d32;
            padding: 5px 10px;
            border-radius: 20px;
            font-size: 0.9em;
            font-weight: 500;
        }

        .reports-section {
            grid-column: 1 / -1;
            margin-top: 30px;
        }

        .report-summary {
            background: linear-gradient(135deg, #a8edea 0%, #fed6e3 100%);
            padding: 25px;
            border-radius: 15px;
            margin-top: 20px;
        }

        .report-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
        }

        .report-item {
            text-align: center;
        }

        .report-item h4 {
            color: #333;
            margin-bottom: 10px;
        }

        .report-value {
            font-size: 1.8em;
            font-weight: bold;
            color: #4facfe;
        }

        @media (max-width: 768px) {
            .sections {
                grid-template-columns: 1fr;
            }
            
            .dashboard {
                grid-template-columns: 1fr;
            }
            
            .header h1 {
                font-size: 2em;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🦷 Clínica Dental</h1>
            <div class="date-display" id="currentDate"></div>
        </div>

        <div class="main-content">
            <!-- Dashboard de estadísticas -->
            <div class="dashboard">
                <div class="stat-card">
                    <h3>Pacientes Hoy</h3>
                    <div class="number" id="totalPatients">0</div>
                </div>
                <div class="stat-card">
                    <h3>Ingresos del Día</h3>
                    <div class="number" id="totalIncome">$0</div>
                </div>
                <div class="stat-card">
                    <h3>Tratamientos</h3>
                    <div class="number" id="totalTreatments">0</div>
                </div>
            </div>

            <div class="sections">
                <!-- Sección de registro de pacientes -->
                <div class="section">
                    <h2>📝 Registrar Paciente</h2>
                    <form id="patientForm">
                        <div class="form-group">
                            <label for="patientName">Nombre del Paciente:</label>
                            <input type="text" id="patientName" required>
                        </div>
                        
                        <div class="form-group">
                            <label for="patientPhone">Teléfono:</label>
                            <input type="tel" id="patientPhone">
                        </div>
                        
                        <div class="form-group">
                            <label for="treatment">Tratamiento Realizado:</label>
                            <select id="treatment" required>
                                <option value="">Seleccionar tratamiento</option>
                                <option value="Limpieza dental">Limpieza dental</option>
                                <option value="Extracción">Extracción</option>
                                <option value="Empaste">Empaste</option>
                                <option value="Endodoncia">Endodoncia</option>
                                <option value="Corona">Corona</option>
                                <option value="Ortodoncia">Ortodoncia</option>
                                <option value="Blanqueamiento">Blanqueamiento</option>
                                <option value="Consulta">Consulta</option>
                                <option value="Otro">Otro</option>
                            </select>
                        </div>
                        
                        <div class="form-group">
                            <label for="cost">Costo del Tratamiento:</label>
                            <input type="number" id="cost" min="0" step="0.01" required>
                        </div>
                        
                        <div class="form-group">
                            <label for="paymentMethod">Método de Pago:</label>
                            <select id="paymentMethod" required>
                                <option value="">Seleccionar método</option>
                                <option value="Efectivo">Efectivo</option>
                                <option value="Tarjeta">Tarjeta</option>
                                <option value="Transferencia">Transferencia</option>
                                <option value="Pendiente">Pendiente</option>
                            </select>
                        </div>
                        
                        <div class="form-group">
                            <label for="notes">Notas (opcional):</label>
                            <textarea id="notes" rows="3"></textarea>
                        </div>
                        
                        <button type="submit">✅ Registrar Paciente</button>
                    </form>
                </div>

                <!-- Lista de pacientes del día -->
                <div class="section">
                    <h2>👥 Pacientes de Hoy</h2>
                    <div class="patient-list" id="patientList">
                        <p style="text-align: center; color: #666;">No hay pacientes registrados hoy</p>
                    </div>
                </div>
            </div>

            <!-- Sección de reportes -->
            <div class="section reports-section">
                <h2>📊 Resumen del Día</h2>
                <div class="report-summary" id="dailySummary">
                    <div class="report-grid">
                        <div class="report-item">
                            <h4>Total de Pacientes</h4>
                            <div class="report-value" id="summaryPatients">0</div>
                        </div>
                        <div class="report-item">
                            <h4>Ingresos Totales</h4>
                            <div class="report-value" id="summaryIncome">$0</div>
                        </div>
                        <div class="report-item">
                            <h4>Pagos Pendientes</h4>
                            <div class="report-value" id="summaryPending">$0</div>
                        </div>
                        <div class="report-item">
                            <h4>Tratamiento Más Común</h4>
                            <div class="report-value" id="summaryCommonTreatment">-</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Variables globales para almacenar datos
        let patientsData = [];
        
        // Inicializar la aplicación
        document.addEventListener('DOMContentLoaded', function() {
            updateCurrentDate();
            loadTodayData();
        });

        // Mostrar fecha actual
        function updateCurrentDate() {
            const now = new Date();
            const options = { 
                weekday: 'long', 
                year: 'numeric', 
                month: 'long', 
                day: 'numeric' 
            };
            document.getElementById('currentDate').textContent = now.toLocaleDateString('es-ES', options);
        }

        // Cargar datos del día actual
        function loadTodayData() {
            const today = new Date().toDateString();
            const savedData = localStorage.getItem(`dental_data_${today}`);
            
            if (savedData) {
                patientsData = JSON.parse(savedData);
                updateDisplay();
            }
        }

        // Guardar datos del día
        function saveTodayData() {
            const today = new Date().toDateString();
            localStorage.setItem(`dental_data_${today}`, JSON.stringify(patientsData));
        }

        // Manejar el formulario de pacientes
        document.getElementById('patientForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const patientData = {
                id: Date.now(),
                name: document.getElementById('patientName').value,
                phone: document.getElementById('patientPhone').value,
                treatment: document.getElementById('treatment').value,
                cost: parseFloat(document.getElementById('cost').value),
                paymentMethod: document.getElementById('paymentMethod').value,
                notes: document.getElementById('notes').value,
                timestamp: new Date().toLocaleTimeString('es-ES')
            };
            
            patientsData.push(patientData);
            saveTodayData();
            updateDisplay();
            
            // Limpiar formulario
            this.reset();
            
            // Mensaje de confirmación
            alert('✅ Paciente registrado correctamente');
        });

        // Actualizar toda la visualización
        function updateDisplay() {
            updateStats();
            updatePatientList();
            updateSummary();
        }

        // Actualizar estadísticas del dashboard
        function updateStats() {
            const totalPatients = patientsData.length;
            const totalIncome = patientsData.reduce((sum, patient) => {
                return patient.paymentMethod !== 'Pendiente' ? sum + patient.cost : sum;
            }, 0);
            const totalTreatments = patientsData.length;
            
            document.getElementById('totalPatients').textContent = totalPatients;
            document.getElementById('totalIncome').textContent = `$${totalIncome.toLocaleString()}`;
            document.getElementById('totalTreatments').textContent = totalTreatments;
        }

        // Actualizar lista de pacientes
        function updatePatientList() {
            const listContainer = document.getElementById('patientList');
            
            if (patientsData.length === 0) {
                listContainer.innerHTML = '<p style="text-align: center; color: #666;">No hay pacientes registrados hoy</p>';
                return;
            }
            
            listContainer.innerHTML = patientsData.map(patient => `
                <div class="patient-item">
                    <div class="patient-info">
                        <div>
                            <div class="patient-name">${patient.name}</div>
                            <div class="patient-treatment">${patient.treatment} - ${patient.timestamp}</div>
                            ${patient.notes ? `<div style="font-size: 0.8em; color: #888; margin-top: 5px;">📝 ${patient.notes}</div>` : ''}
                        </div>
                        <div class="patient-payment">
                            ${patient.paymentMethod} - $${patient.cost.toLocaleString()}
                        </div>
                    </div>
                </div>
            `).join('');
        }

        // Actualizar resumen del día
        function updateSummary() {
            const totalPatients = patientsData.length;
            const totalIncome = patientsData.reduce((sum, patient) => {
                return patient.paymentMethod !== 'Pendiente' ? sum + patient.cost : sum;
            }, 0);
            const pendingPayments = patientsData.reduce((sum, patient) => {
                return patient.paymentMethod === 'Pendiente' ? sum + patient.cost : sum;
            }, 0);
            
            // Tratamiento más común
            const treatmentCount = {};
            patientsData.forEach(patient => {
                treatmentCount[patient.treatment] = (treatmentCount[patient.treatment] || 0) + 1;
            });
            
            const mostCommonTreatment = Object.keys(treatmentCount).reduce((a, b) => 
                treatmentCount[a] > treatmentCount[b] ? a : b, '-'
            );
            
            document.getElementById('summaryPatients').textContent = totalPatients;
            document.getElementById('summaryIncome').textContent = `$${totalIncome.toLocaleString()}`;
            document.getElementById('summaryPending').textContent = `$${pendingPayments.toLocaleString()}`;
            document.getElementById('summaryCommonTreatment').textContent = totalPatients > 0 ? mostCommonTreatment : '-';
        }
    </script>
</body>
</html>
