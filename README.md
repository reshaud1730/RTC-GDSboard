# RTC-GDSboard
Dispatxh app for generators in the Upland Metaverse!
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Raytown Construction Dispatch Command</title>
    <style>
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background-color: #f4f4f9; color: #333; margin: 0; padding: 20px; }
        h1, h2, h3 { color: #2c3e50; margin-top: 0; }
        .container { max-width: 1200px; margin: auto; }
        
        .dashboard-header { display: flex; justify-content: space-between; align-items: center; background: #2c3e50; color: white; padding: 15px 20px; border-radius: 8px; margin-bottom: 20px; }
        .header-title-group { display: flex; align-items: center; }
        .logo-placeholder { height: 50px; width: auto; margin-right: 15px; border-radius: 4px; background-color: white; padding: 2px; }
        
        .stats-bar { background: #34495e; color: white; padding: 15px 20px; border-radius: 8px; margin-bottom: 20px; display: flex; justify-content: space-between; font-size: 1.2em; font-weight: bold; box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
        #total-spark-stat { color: #f1c40f; font-size: 1.4em; } 

        .panels { display: flex; gap: 20px; flex-wrap: wrap; align-items: flex-start; }
        .panel { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1); flex: 1; min-width: 300px; }
        label { font-weight: bold; font-size: 0.9em; color: #555; }
        select, button, input { width: 100%; padding: 10px; margin-top: 5px; margin-bottom: 15px; border-radius: 5px; border: 1px solid #ccc; font-size: 16px; box-sizing: border-box; }
        button { background-color: #3498db; color: white; border: none; cursor: pointer; font-weight: bold; margin-bottom: 0; }
        button:hover { background-color: #2980b9; }
        .export-btn { background-color: #9b59b6; margin-top: 20px; }
        .export-btn:hover { background-color: #8e44ad; }
        
        .project-card { border-left: 5px solid gray; background: #f9f9f9; margin-bottom: 15px; padding: 15px; border-radius: 4px; position: relative; }
        .green-gen { border-left-color: #2ecc71; }
        .red-gen { border-left-color: #e74c3c; }
        .yellow-gen { border-left-color: #f1c40f; }
        
        .progress-bar { background: #e0e0e0; border-radius: 4px; height: 10px; width: 10px; margin-top: 5px; overflow: hidden; width: 100%; }
        .progress-fill { background: #3498db; height: 100%; }
        
        .meta-data { font-size: 0.85em; color: #555; margin-top: 10px; }
        .meta-data p { margin: 4px 0; }
        .accept-btn { margin-top: 15px; background-color: #2ecc71; }
        .accept-btn:hover { background-color: #27ae60; }
        .end-btn { margin-top: 10px; background-color: #e74c3c; font-size: 0.9em; padding: 8px; }
        .end-btn:hover { background-color: #c0392b; }
        .completed-card { opacity: 0.9; background: #ecf0f1; border-left-color: #95a5a6 !important; }
        .sub-name { color: #8e44ad; font-weight: bold; }
        .completion-highlight { color: #d35400; font-weight: bold; }
        
        .liability-box { background: #e8f8f5; border: 2px solid #2ecc71; padding: 15px; text-align: center; border-radius: 8px; margin-top: 20px; display: flex; justify-content: space-between; gap: 10px; }
        .liability-col { flex: 1; }
        .liability-col h4 { margin: 0; color: #27ae60; font-size: 1em; }
        .company-col h4 { color: #2980b9; }
        .liability-col p { margin: 5px 0 0 0; font-size: 1.4em; font-weight: bold; color: #2c3e50; }
        
        .status-dot { height: 10px; width: 10px; background-color: #2ecc71; border-radius: 50%; display: inline-block; margin-right: 5px; animation: pulse 2s infinite; }
        @keyframes pulse { 0% { opacity: 1; } 50% { opacity: 0.4; } 100% { opacity: 1; } }
    </style>
</head>
<body>

<div class="container">
    <div class="dashboard-header">
        <div class="header-title-group">
            <img src="https://via.placeholder.com/150x50/34495e/ffffff?text=Raytown+Construction" alt="Raytown Construction Logo" class="logo-placeholder" id="company-logo">
            <h2 style="margin: 0;">Dispatch Command</h2>
        </div>
        <div id="live-clock" style="font-weight: bold; font-size: 1.2em;"></div>
    </div>

    <div class="stats-bar">
        <span>Total Active Spark on the Field:</span>
        <span id="total-spark-stat">0.00 Spark</span>
    </div>

    <div class="panels">
        <div class="panel">
            <h3>Customer: List a Project</h3>
            
            <label for="category-select">Select Project Category:</label>
            <select id="category-select" onchange="populateProperties()">
                <option value="">-- Choose Category --</option>
                <option value="green">Green (Houses & Estates)</option>
                <option value="red">Red (Townhouses & Mid-Sized)</option>
                <option value="yellow">Yellow (Towers & Large Apts)</option>
            </select>

            <label for="property-select">Select Specific Property:</label>
            <select id="property-select" disabled>
                <option value="">-- Await Category --</option>
            </select>

            <label for="project-address">Upland Property Address:</label>
            <input type="text" id="project-address" placeholder="e.g., 123 Upland Ave">

            <button onclick="listProject()" style="margin-top: 10px;">Deploy to Dispatch Board</button>
        </div>

        <div class="panel">
            <h3>Subcontractor: Dispatch Board</h3>
            <label for="filter-select">Filter Available Projects:</label>
            <select id="filter-select" onchange="renderBoard()">
                <option value="all">Show All</option>
                <option value="green">Green Generators Only</option>
                <option value="red">Red Generators Only</option>
                <option value="yellow">Yellow Generators Only</option>
            </select>

            <div id="dispatch-board"></div>
        </div>

        <div class="panel">
            <h3>Active Subcontractor Log</h3>
            <p style="font-size: 0.9em; color: #666; margin-top: 0;">Live tracking and finalized payouts (95/5 Split).</p>
            <div id="completed-board"></div>
            <button class="export-btn" onclick="exportToCSV()">Export to FastTrack Ledger (CSV)</button>
        </div>
    </div>
</div>

<script>
    function updateClock() {
        const now = new Date();
        document.getElementById('live-clock').innerText = now.toLocaleString('en-US');
    }
    setInterval(updateClock, 1000);
    updateClock();

    const sparkletRules = {
        green: { min: 5000, max: 30000 },
        red: { min: 10000, max: 30000 },
        yellow: { min: 15000, max: 30000 }
    };

    const properties = {
        green: [
            { name: "Luxury Modern House", value: 75000 }, { name: "Luxury Ranch House", value: 75000 },
            { name: "The Filigree House", value: 75000 }, { name: "Cosy Cabin", value: 72000 },
            { name: "Ivory Rising Pearl", value: 72000 }, { name: "Micro House", value: 72000 },
            { name: "Spanish Luxus Villa", value: 54000 }, { name: "Mansão Brisa do Oceano", value: 48000 },
            { name: "Paris Estate", value: 48000 }, { name: "Super Luxury Ranch House", value: 48000 },
            { name: "Bermuda Luxe Ranch Estate", value: 45000 }, { name: "Contemporary House", value: 45000 },
            { name: "Genesis Modern house ’25", value: 45000 }, { name: "London Manor", value: 45000 },
            { name: "Manty Cottage / Mansion", value: 45000 }, { name: "Prospector Berlin Small House", value: 45000 },
            { name: "Rome Ranch House", value: 45000 }, { name: "Urban Residence", value: 45000 },
            { name: "Bermuda Luxe Coastal House", value: 42000 }, { name: "Bermuda Mystwood Manor", value: 42000 },
            { name: "BigNick Midcentury Ranch House", value: 42000 }, { name: "Modern Chalet", value: 42000 },
            { name: "Two Story Loft", value: 42000 }, { name: "Paris Duplex", value: 42000 },
            { name: "Bermuda Nestled Townhouse", value: 39000 }, { name: "Buenos Aires Ranch House", value: 39000 },
            { name: "Craftsman House", value: 39000 }, { name: "Family Home", value: 39000 },
            { name: "Paris Maisonette", value: 39000 }, { name: "Tokyo Keshoyane", value: 39000 },
            { name: "Bermuda Snug Sanctuary", value: 36000 }, { name: "Paris Cottage", value: 36000 },
            { name: "Ranch House", value: 30000 }, { name: "Small Town House (1 & 2)", value: 30000 }
        ],
        red: [
            { name: "Buenos Aires Town House", value: 51000 }, { name: "Madrid Townhouse", value: 51000 },
            { name: "Town House", value: 60000 }, { name: "Hong Kong Apartment building", value: 48000 },
            { name: "Paris Condos", value: 48000 }, { name: "Assassins Courtyard", value: 48000 },
            { name: "Queen Anne", value: 48000 }, { name: "Echo of Berlin", value: 48000 },
            { name: "Hong Kong Town House", value: 48000 }, { name: "Paris Town House", value: 48000 },
            { name: "Rome Town House", value: 48000 }, { name: "SERA Design Postmodern", value: 48000 },
            { name: "Singapore Townhouse", value: 48000 }, { name: "Victorian House", value: 48000 },
            { name: "European Modular Apartments", value: 45000 }, { name: "Main Street Modular Apartments", value: 45000 },
            { name: "Vista do Douro", value: 45000 }, { name: "Williamsburg Townhouse", value: 45000 },
            { name: "Brownstone Queens", value: 45000 }
        ],
        yellow: [
            { name: "Cypher Cyborgs Tower", value: 1500000 }, { name: "Krypto Knights Tower", value: 1500000 },
            { name: "Purge Pirates Tower", value: 1500000 }, { name: "High-Rise Residential Tower", value: 99000 },
            { name: "Apartment Building", value: 90000 }, { name: "Tokyo Master Builders Tower", value: 90000 },
            { name: "Lillicorp Tower", value: 72000 }, { name: "Apex Tower", value: 66000 },
            { name: "Historical City Apartment Building", value: 66000 }, { name: "Mavs Perch", value: 66000 },
            { name: "Grand Arlington Suites", value: 60000 }, { name: "Los Angeles Residential Tower", value: 60000 },
            { name: "Luxury Tower", value: 57000 }, { name: "Palácio Da Luz", value: 54000 },
            { name: "CMavs KCAD", value: 54000 }, { name: "Bermuda Breeze Apartments", value: 54000 },
            { name: "Residential Tower", value: 51000 }, { name: "Glass Tower", value: 51000 },
            { name: "Prospector Steampunk London Apartment", value: 51000 }
        ]
    };

    let activeProjects = [];
    let ledgerLog = [];

    function updateDashboardStats() {
        let totalActiveSpark = 0;
        ledgerLog.forEach(proj => {
            if (proj.status === 'active') totalActiveSpark += proj.activeSpark;
        });
        document.getElementById('total-spark-stat').innerText = totalActiveSpark.toFixed(2) + ' Spark';
    }

    function populateProperties() {
        const catSelect = document.getElementById('category-select').value;
        const propSelect = document.getElementById('property-select');
        propSelect.innerHTML = '<option value="">-- Select Property --</option>';
        if(catSelect) {
            propSelect.disabled = false;
            properties[catSelect].forEach((prop, index) => {
                let opt = document.createElement('option');
                opt.value = index;
                opt.text = `${prop.name} (${prop.value.toLocaleString()} Spark Hrs)`;
                propSelect.appendChild(opt);
            });
        } else {
            propSelect.disabled = true;
        }
    }

    function listProject() {
        const catSelect = document.getElementById('category-select').value;
        const propIndex = document.getElementById('property-select').value;
        const addressInput = document.getElementById('project-address').value.trim();
        
        if(!catSelect || propIndex === "" || addressInput === "") {
            alert("Please select a category, a property, and enter the project address.");
            return;
        }

        const selectedProp = properties[catSelect][propIndex];
        const maxGenerators = Math.ceil(selectedProp.value / 10000); 
        
        const project = {
            id: Date.now(),
            category: catSelect,
            name: selectedProp.name,
            address: addressInput,
            value: selectedProp.value,
            maxGens: maxGenerators,
            currentGens: 0, // Track how many slots are filled
            timeListed: new Date().toLocaleString()
        };

        activeProjects.push(project);
        renderBoard();
        
        document.getElementById('category-select').value = '';
        document.getElementById('project-address').value = '';
        populateProperties();
    }

    function renderBoard() {
        const filter = document.getElementById('filter-select').value;
        const board = document.getElementById('dispatch-board');
        board.innerHTML = '';

        if (activeProjects.length === 0) {
            board.innerHTML = '<p style="color: #7f8c8d; font-style: italic;">No active projects awaiting dispatch.</p>';
            return;
        }

        activeProjects.forEach(proj => {
            if(filter === 'all' || filter === proj.category) {
                const rules = sparkletRules[proj.category];
                const availableSlots = proj.maxGens - proj.currentGens;
                const fillPercentage = (proj.currentGens / proj.maxGens) * 100;
                
                let div = document.createElement('div');
                div.className = `project-card ${proj.category}-gen`;
                
                div.innerHTML = `
                    <strong style="font-size: 1.1em; color: #2c3e50;">${proj.name}</strong>
                    <div class="meta-data">
                        <p><strong>Address:</strong> ${proj.address}</p>
                        <p><strong>Gen Color:</strong> <span style="text-transform: capitalize;">${proj.category}</span></p>
                        <p><strong>Available Slots:</strong> <span style="color: #d35400; font-weight: bold;">${availableSlots} Open</span> (Max ${proj.maxGens})</p>
                        <div class="progress-bar"><div class="progress-fill" style="width: ${fillPercentage}%"></div></div>
                        <p><strong>Payout Rate:</strong> 95% of Spark Hours to Sub, 5% to Company</p>
                        <p><strong>Sparklet Rules:</strong> ${rules.min.toLocaleString()} to ${rules.max.toLocaleString()} per Gen</p>
                    </div>
                    <button class="accept-btn" onclick="acceptProject(${proj.id})">Accept Job & Stake Gens</button>
                `;
                board.appendChild(div);
            }
        });
    }

    function acceptProject(id) {
        const projectIndex = activeProjects.findIndex(p => p.id === id);
        if (projectIndex === -1) return;
        const proj = activeProjects[projectIndex];
        const availableSlots = proj.maxGens - proj.currentGens;
        const rules = sparkletRules[proj.category];

        const username = prompt("Enter your Upland Username to claim generators on this dispatch:");
        if (!username || username.trim() === "") { alert("Username is required."); return; }

        let gensDeployed = prompt(`How many generators are you deploying? \n(Available Slots Remaining: ${availableSlots})`);
        gensDeployed = parseInt(gensDeployed);
        if (isNaN(gensDeployed) || gensDeployed <= 0 || gensDeployed > availableSlots) {
            alert(`Invalid entry. You must deploy between 1 and ${availableSlots} generators.`);
            return;
        }

        let sparkletPerGen = prompt(`How much Sparklet are you loading PER generator?\n(Required for ${proj.category}: Min ${rules.min.toLocaleString()}, Max ${rules.max.toLocaleString()})`);
        sparkletPerGen = parseInt(sparkletPerGen.replace(/,/g, ''));

        if (isNaN(sparkletPerGen) || sparkletPerGen < rules.min || sparkletPerGen > rules.max) {
            alert(`Invalid Sparklet amount. A ${proj.category} generator requires between ${rules.min.toLocaleString()} and ${rules.max.toLocaleString()} Sparklet.`);
            return;
        }

        // --- MATH ---
        const totalSparkletStaked = gensDeployed * sparkletPerGen;
        const totalSpark = totalSparkletStaked / 1000; 
        const now = Date.now();
        
        // Create an individual STAKE record for the Ledger
        const stakeRecord = {
            id: Date.now() + Math.random(), // Unique ID for this specific stake
            parentId: proj.id,
            category: proj.category,
            name: proj.name,
            address: proj.address,
            subcontractor: username.trim(),
            gensDeployed: gensDeployed,
            sparkletPerGen: sparkletPerGen,
            activeSpark: totalSpark,
            status: 'active',
            timestampAccepted: now,
            timeAccepted: new Date(now).toLocaleString(),
            actualPayout: 0,
            companyCut: 0,
            actualHoursStaked: 0
        };
        
        ledgerLog.push(stakeRecord);
        
        // Update parent project
        proj.currentGens += gensDeployed;
        
        // Remove from board ONLY if fully staked
        if (proj.currentGens >= proj.maxGens) {
            activeProjects.splice(projectIndex, 1);
        }

        renderBoard();
        renderLogBoard();
        updateDashboardStats();
    }

    function finalizeProject(stakeId) {
        const stake = ledgerLog.find(p => p.id === stakeId);
        if (!stake || stake.status === 'finished') return;

        const confirmEnd = confirm("End this stake? Payout will be calculated precisely based on hours staked and active Spark.");
        if (!confirmEnd) return;

        const now = Date.now();
        let hoursStaked = (now - stake.timestampAccepted) / (1000 * 60 * 60);
        
        // Formula: Total Spark * Hours Staked = Total Contributed Spark Hours
        const sparkHoursContributed = stake.activeSpark * hoursStaked;
        
        // New Split: 95% to Sub, 5% to Company
        let finalSubPayout = Math.floor(sparkHoursContributed * 0.95);
        let finalCompanyCut = Math.floor(sparkHoursContributed * 0.05);

        stake.status = 'finished';
        stake.actualHoursStaked = hoursStaked.toFixed(2);
        stake.actualPayout = finalSubPayout;
        stake.companyCut = finalCompanyCut;
        stake.timeFinished = new Date(now).toLocaleString();

        renderLogBoard();
        updateDashboardStats();
    }

    function renderLogBoard() {
        const board = document.getElementById('completed-board');
        board.innerHTML = '';

        if (ledgerLog.length === 0) {
            board.innerHTML = '<p style="color: #7f8c8d; font-style: italic;">No active subcontractors.</p>';
            return;
        }

        let totalSubLiability = 0;
        let totalCompanyCut = 0;

        ledgerLog.forEach(stake => {
            let div = document.createElement('div');
            
            if (stake.status === 'active') {
                div.className = `project-card ${stake.category}-gen`;
                div.innerHTML = `
                    <strong><span class="status-dot"></span>${stake.name} (Sub: ${stake.subcontractor})</strong>
                    <div class="meta-data">
                        <p><strong>Address:</strong> ${stake.address}</p>
                        <p><strong>Power Staked:</strong> ${stake.activeSpark.toFixed(2)} Spark (${stake.gensDeployed} Gens)</p>
                        <p><strong>Started:</strong> ${stake.timeAccepted}</p>
                        <p><em>Payout accumulating live based on 95/5 split...</em></p>
                    </div>
                    <button class="end-btn" onclick="finalizeProject(${stake.id})">End Stake / Calculate Split</button>
                `;
            } else {
                totalSubLiability += stake.actualPayout; 
                totalCompanyCut += stake.companyCut;
                
                div.className = `project-card completed-card ${stake.category}-gen`;
                div.innerHTML = `
                    <strong style="color: #7f8c8d;">[FINALIZED] ${stake.name}</strong>
                    <div class="meta-data">
                        <p><strong>Address:</strong> ${stake.address}</p>
                        <p><strong>Subcontractor:</strong> <span class="sub-name">${stake.subcontractor}</span></p>
                        <p><strong>Time Staked:</strong> ${stake.actualHoursStaked} Hours</p>
                        <p><strong>Sub Payout (95%):</strong> <span style="color: #27ae60; font-weight: bold;">${stake.actualPayout.toLocaleString()} UPX</span></p>
                        <p><strong>Company Cut (5%):</strong> <span style="color: #2980b9; font-weight: bold;">${stake.companyCut.toLocaleString()} UPX</span></p>
                    </div>
                `;
            }
            board.appendChild(div);
        });

        board.innerHTML += `
            <div class="liability-box">
                <div class="liability-col">
                    <h4>Total Owed to Fleet</h4>
                    <p>${totalSubLiability.toLocaleString()} UPX</p>
                </div>
                <div class="liability-col company-col">
                    <h4>Raytown Co. Cut</h4>
                    <p>${totalCompanyCut.toLocaleString()} UPX</p>
                </div>
            </div>
        `;
    }

    function exportToCSV() {
        if (ledgerLog.length === 0) {
            alert("No projects to export.");
            return;
        }

        let csvContent = "data:text/csv;charset=utf-8,";
        csvContent += "Stake ID,Status,Subcontractor,Address,Property Name,Active Spark,Hours Staked,Sub Payout (95%),Company Cut (5%)\n";

        ledgerLog.forEach(row => {
            let actualHours = row.status === 'finished' ? row.actualHoursStaked : 'IN PROGRESS';
            let subPayout = row.status === 'finished' ? row.actualPayout : 'PENDING';
            let coCut = row.status === 'finished' ? row.companyCut : 'PENDING';

            let rowData = [
                row.id,
                row.status.toUpperCase(),
                `"${row.subcontractor}"`, 
                `"${row.address}"`,
                `"${row.name}"`, 
                row.activeSpark.toFixed(2),
                actualHours,
                subPayout,
                coCut
            ];
            csvContent += rowData.join(",") + "\n";
        });

        const encodedUri = encodeURI(csvContent);
        const link = document.createElement("a");
        link.setAttribute("href", encodedUri);
        link.setAttribute("download", "FastTrack_Ledger_Export.csv");
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
    }
</script>

</body>
</html>
