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
        .meta-data { font-size: 0.85em; color: #555; margin-top: 10px; }
        .meta-data p { margin: 4px 0; }
        .accept-btn { margin-top: 15px; background-color: #2ecc71; }
        .accept-btn:hover { background-color: #27ae60; }
        .end-btn { margin-top: 10px; background-color: #e74c3c; font-size: 0.9em; padding: 8px; }
        .end-btn:hover { background-color: #c0392b; }
        .completed-card { opacity: 0.8; }
        .sub-name { color: #8e44ad; font-weight: bold; }
        .completion-highlight { color: #d35400; font-weight: bold; }
        .liability-box { background: #e8f8f5; border: 2px solid #2ecc71; padding: 15px; text-align: center; border-radius: 8px; margin-top: 20px; display: flex; justify-content: space-between; gap: 10px; }
        .liability-col { flex: 1; }
        .liability-col h4 { margin: 0; color: #27ae60; font-size: 1.1em; }
        .company-col h4 { color: #2980b9; }
        .liability-col p { margin: 5px 0 0 0; font-size: 1.6em; font-weight: bold; color: #2c3e50; }
        .status-dot { height: 10px; width: 10px; background-color: #2ecc71; border-radius: 50%; display: inline-block; margin-right: 5px; animation: pulse 2s infinite; }
        @keyframes pulse { 0% { opacity: 1; } 50% { opacity: 0.4; } 100% { opacity: 1; } }
    </style>
</head>
<body>

<div class="container">
    <div class="dashboard-header">
        <h2 style="margin: 0;">Raytown Construction Dispatch Command</h2>
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
                <option value="service_structures">Service Structures (Arbitrary Logic)</option>
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
            <h3>Active Construction Log</h3>
            <p style="font-size: 0.9em; color: #666; margin-top: 0;">Live tracking and finalized payouts (95/5 Split).</p>
            <div id="completed-board"></div>
            <button class="export-btn" onclick="exportToCSV()">Export to FastTrack Ledger (CSV)</button>
        </div>
    </div>
</div>

<script>
    // Live Clock Logic
    function updateClock() {
        const now = new Date();
        document.getElementById('live-clock').innerText = now.toLocaleString('en-US');
    }
    setInterval(updateClock, 1000);
    updateClock();

    // Generator Rules (Sparklet)
    const sparkletRules = {
        green: { min: 5000, max: 30000 },
        red: { min: 10000, max: 30000 },
        yellow: { min: 15000, max: 30000 }
    };

    // Global Math Variables (New 95/5 math applied across the entire system)
    const sparkHoursToPointRatio = 1000; // 1 Spark = 1000 Spark Hours
    const subcontractorPayoutPercentage = 0.95; // 95% to subcontractor
    const companyCutPercentage = 0.05; // 5% company cut

    // --- REBUILT PROPERTIES DATABASE FROM NEW bulk lists ---
    // (Existing category color-coding maintained for old names. 
    // New arbitrary color assignment logic for bulk items with a tier strategy.)
    const properties = {
        green: [
            { name: "Filigree House", value: 75000 }, { name: "spanish villa", value: 54000 }, 
            { name: "pearl house", value: 72000 }, { name: "Paris Maisonette", value: 39000 },
            { name: "Paris Cottage", value: 36000 }, { name: "Tokyo Keshoyane", value: 39000 },
            { name: "Bermuda Snug Sanctuary", value: 36000 }, { name: "Craftsman House", value: 39000 },
            { name: "Family Home", value: 39000 }, { name: "Parisaisonette", value: 39000 }, 
            { name: "Buenos Aires Ranch House", value: 39000 }, { name: "Ranch House", value: 30000 }, 
            { name: "small town house (1&2)", value: 30000 }, { name: "Small Town House", value: 30000 }
        ],
        red: [
            { name: "Hong Kong Apartment building", value: 48000 }, { name: "Hk Townhouse", value: 48000 },
            { name: " sera design townhouse", value: 48000 }, { name: " singapore townhouse", value: 48000 },
            { name: "Madrid Townhouse", value: 51000 }, { name: "Buenos Aires Town House", value: 51000 },
            { name: "Vista do Douro", value: 45000 }, { name: "Williamsburg Townhouse", value: 45000 },
            { name: "Town House", value: 60000 }, { name: "hong kong town house", value: 48000 },
            { name: "Douglas Townhouse", value: 48000 }, { name: "sera design postmodern", value: 48000 },
            { name: "European Modular Apartments (Various)", value: 45000 }, { name: "Main Street Modular Apartments (Various)", value: 45000 },
            { name: " junior college", value: 54900 }, { name: " police station", value: 51300 },
            { name: "Elementary School", value: 45900 }, { name: "Classic Hotel", value: 44500 }, 
            { name: "Hardware Store", value: 42750 }, { name: "Live Theater", value: 41400 }
        ],
        yellow: [
            { name: "Cypher Cyborgs Tower(1,2,3)", value: 1500000 }, { name: "apex tower", value: 66000 },
            { name: "Krypto Knights Tower(1,2,3)", value: 1500000 }, { name: "Purge Pirates Tower(1,2,3)", value: 1500000 },
            { name: "High-Rise Residential Tower", value: 99000 }, { name: "Apartment Building", value: 90000 },
            { name: "Tokyo Master Builders Tower", value: 90000 }, { name: "Lillicorp Tower", value: 72000 }, 
            { name: "Apex Tower", value: 66000 }, { name: " Historical City Apartment Building", value: 66000 },
            { name: "MAVS PERCH", value: 66000 }, { name: "GRAND ARLINGTON SUITES", value: 60000 },
            { name: "LOS ANGELES RESIDENTIAL TOWER", value: 60000 }, { name: "LUXURY TOWER", value: 57000 },
            { name: "Palacio Da Luz", value: 54000 }, { name: "CMavs KCAD", value: 54000 }, 
            { name: "Bermuda Breeze Apartments", value: 54000 }, { name: "RESIDENTIAL TOWER", value: 51000 }, 
            { name: "GLASS TOWER", value: 51000 }, { name: "PROSPECTOR STEAMPUNK LONDON APARTMENT", value: 51000 }
        ],
        // --- NEW CATEGORY FROM Bulk List images ---
        service_structures: [
            { name: " university", value: 147150 }, { name: " high school", value: 110700 },
            { name: " planitarium", value: 90450 }, { name: " hospital", value: 80100 },
            { name: " natural history museum", value: 75150 }, { name: " post office distribution center", value: 74250 },
            { name: " performing arts", value: 73800 }, { name: " modern hotel", value: 70650 },
            { name: " sports courts", value: 68400 }, { name: " science museum", value: 67500 },
            { name: " supermarket super store", value: 66150 }, { name: " fire station headquarters", value: 62100 },
            { name: " department store", value: 49050 }, { name: " motel", value: 38700 },
            { name: " night club", value: 37800 }, { name: " farmers market", value: 36900 },
            { name: " public pool", value: 35550 }, { name: " courthouse", value: 34650 },
            { name: " clinic", value: 32400 }, { name: " recycling center", value: 32400 },
            { name: " aquarium", value: 34200 }, { name: " movie theater", value: 31950 },
            { name: " bowling alley", value: 31050 }, { name: " community center", value: 31050 },
            { name: " dmv", value: 31050 }, { name: " brewery", value: 30150 },
            { name: " neighborhood police station", value: 27450 }, { name: " library", value: 27000 },
            { name: " comedy club", value: 26550 }, { name: " animal shelter", value: 26100 },
            { name: " art museum", value: 24300 }, { name: " old asylum", value: 23400 },
            { name: " assisted living center", value: 23400 }, { name: " mini golf", value: 22950 },
            { name: " fish market", value: 22500 }, { name: " physical therapy clinic", value: 19800 },
            { name: " ice rink", value: 19350 }, { name: " ramen restaurant", value: 19350 },
            { name: " luxury restaurant", value: 18900 }, { name: " oil change center", value: 18450 },
            { name: " fire station", value: 18000 }, { name: " veterinary clinic", value: 18000 },
            { name: " fine dining restaurant", value: 17550 }, { name: " electronics store", value: 17550 },
            { name: " auto repair shop", value: 17100 }, { name: " supermarket", value: 16650 },
            { name: " dentistry", value: 16200 }, { name: " martial arts dojo", value: 16200 },
            { name: " day care center", value: 15750 }, { name: " pool hall", value: 15750 },
            { name: " gym", value: 15300 }, { name: " laundromat", value: 15300 },
            { name: " auto carwash", value: 13950 }, { name: " funeral home", value: 13950 },
            { name: " jewelry", value: 13050 }, { name: " casual dining restaurant", value: 12600 },
            { name: " coffee stand", value: 12600 }, { name: " bank", value: 12600 },
            { name: " fast food joint", value: 12600 }, { name: " post office", value: 12150 },
            { name: " luxury fashion boutique", value: 12150 }, { name: " barbershop", value: 11700 },
            { name: " coffee shop", value: 11700 }, { name: " sneaker store", value: 11700 },
            { name: " dry cleaner", value: 10800 }, { name: " tire shop", value: 10800 },
            { name: " toy store", value: 10800 }, { name: " pub", value: 10350 },
            { name: " optometrist", value: 10350 }, { name: " arcade", value: 9900 },
            { name: " ice cream parlor", value: 9900 }, { name: " bodega", value: 9900 },
            { name: " butcher shop", value: 9900 }, { name: " pharmacy", value: 9450 },
            { name: " nail salon", value: 9000 }, { name: " massage parlor", value: 8550 },
            { name: " pet shop", value: 8550 }, { name: " pawn shop", value: 8550 },
            { name: " bakery", value: 8100 }, { name: " bookstore", value: 8100 },
            { name: " kiosk - gelato", value: 7200 }, { name: " kiosk - taco", value: 7200 },
            { name: " kiosk - sushi", value: 7200 }, { name: " kiosk - donuts", value: 7200 },
            { name: " kiosk - fried chicken", value: 7200 }, { name: " crepe stand", value: 7200 },
            { name: " cinnamon bun stand", value: 7200 }, { name: " fish ball stand", value: 7200 },
            { name: " kiosk - hot dog", value: 6750 }, { name: " kiosk - pizza", value: 6750 },
            { name: " kiosk - hamburger", value: 6750 }, { name: " empanada stand", value: 6750 },
            { name: " fish deli", value: 6750 }, { name: " sausage stand", value: 6300 },
            { name: " information kiosk", value: 4500 }, { name: " newspaper stand", value: 4050 },
            { name: " bus stop", value: 3150 }
        ]
    };

    // --- ARBITRARY LOGIC FUNCTION ---
    // (Math-based classification strategy for color-coding bulk items. 
    // Logic can be updated at the structure name level easily. Disclaimer added.)
    function getGeneratorColorByValue(value) {
        // Classify value into ranges (low, mid, high tier) for reproducing logic
        if (value < 40000) return 'green'; // Green Tier (Houses size range)
        else if (value < 70000) return 'red'; // Red Tier (Townhouses size range)
        else return 'yellow'; // Yellow Tier (University/hospital/towers range)
    }

    let activeProjects = [];
    let ledgerLog = [];

    // Master Stats Update
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
                // Text reflects HRs for clarity
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
        // Max Payout based on original reward logic, not used in finalized math
        const subcontractorReward = Math.floor(selectedProp.value * 0.15); 
        
        // --- LOGIC ASSIGNMENT ---
        // (Customer chooses 'Service Structures' new choice. Subcontractorboard logic 
        // needs point values. One color assigned to entire group is requested. Arbitrary Red call made for complete solution.)
        let requiredGenColor = 'red'; // Arbitrary color chosen for entire 'Service Structures' new section

        const project = {
            id: Date.now(),
            category: catSelect,
            name: selectedProp.name,
            address: addressInput,
            value: selectedProp.value, // Spark Hrs Needed
            maxGens: maxGenerators,
            payout: subcontractorReward, // Old math, only for liability box display
            timeListed: new Date().toLocaleString(),
            // --- NEW LOGIC FIELD ---
            requiredGenColor: requiredGenColor 
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
                // Determine color tier for bulk list arbitrary assignments
                let colorTier = getGeneratorColorByValue(proj.value);
                // Dispatch logic: if item from new category, show arbitrary color. If not, old category level logic applies.
                let displayColor = proj.category === 'service_structures' ? proj.requiredGenColor : proj.category;
                const rules = sparkletRules[displayColor];
                
                let div = document.createElement('div');
                div.className = `project-card ${displayColor}-gen`;
                
                div.innerHTML = `
                    <strong style="font-size: 1.1em; color: #2c3e50;">${proj.name}</strong>
                    <div class="meta-data">
                        <p><strong>Address:</strong> ${proj.address}</p>
                        <p><strong>Gen Color:</strong> <span style="text-transform: capitalize;">${displayColor}</span></p>
                        <p><strong>Generator Limits:</strong> Max ${proj.maxGens} Gens (${proj.value.toLocaleString()} Spark Hrs)</p>
                        <p><strong>Sparklet Rules:</strong> ${rules.min.toLocaleString()} to ${rules.max.toLocaleString()} Sparklet per Gen</p>
                        <p><strong>Payout Rate:</strong> 95% of build cost to Sub, 5% to Company</p>
                    </div>
                    <button class="accept-btn" onclick="acceptProject(${proj.id})">Accept Project</button>
                `;
                board.appendChild(div);
            }
        });
    }

    function acceptProject(id) {
        const projectIndex = activeProjects.findIndex(p => p.id === id);
        if (projectIndex === -1) return;
        const proj = activeProjects[projectIndex];
        let colorForRules = proj.category === 'service_structures' ? proj.requiredGenColor : proj.category;
        const rules = sparkletRules[colorForRules];

        const username = prompt("Enter your Upland Username to claim this dispatch:");
        if (!username || username.trim() === "") { alert("Username is required."); return; }

        let gensDeployed = prompt(`How many generators are you deploying? (Maximum allowed: ${proj.maxGens})`);
        gensDeployed = parseInt(gensDeployed);
        if (isNaN(gensDeployed) || gensDeployed <= 0 || gensDeployed > proj.maxGens) {
            alert(`Invalid entry. Please enter a number between 1 and ${proj.maxGens}.`);
            return;
        }

        let sparkletPerGen = prompt(`How much Sparklet are you loading PER generator?\n(Required for ${colorForRules}: Min ${rules.min.toLocaleString()}, Max ${rules.max.toLocaleString()})`);
        sparkletPerGen = parseInt(sparkletPerGen.replace(/,/g, ''));

        if (isNaN(sparkletPerGen) || sparkletPerGen < rules.min || sparkletPerGen > rules.max) {
            alert(`Invalid Sparklet amount. A ${colorForRules} generator requires between ${rules.min.toLocaleString()} and ${rules.max.toLocaleString()} Sparklet.`);
            return;
        }

        // --- MATH REBUILT WITH NEW 95/5 SPLIT ---
        const totalSparkletStaked = gensDeployed * sparkletPerGen;
        // 1000 Sparklet = 1 active Spark
        const totalSpark = totalSparkletStaked / sparkHoursToPointRatio; 
        const hoursNeeded = proj.value / totalSpark; // Build cost / active Spark = total hours
        
        const now = Date.now();
        const completionDate = new Date(now + (hoursNeeded * 60 * 60 * 1000));

        let acceptedProject = activeProjects.splice(projectIndex, 1)[0];
        acceptedProject.status = 'active';
        acceptedProject.timestampAccepted = now;
        acceptedProject.timeAccepted = new Date(now).toLocaleString();
        acceptedProject.subcontractor = username.trim(); 
        acceptedProject.gensDeployed = gensDeployed;
        acceptedProject.sparkletPerGen = sparkletPerGen;
        acceptedProject.activeSpark = totalSpark;
        acceptedProject.hoursToComplete = hoursNeeded;
        acceptedProject.estimatedCompletion = completionDate.toLocaleString('en-US', { 
            weekday: 'short', month: 'short', day: 'numeric', hour: '2-digit', minute: '2-digit' 
        });
        acceptedProject.actualPayout = 0; 
        acceptedProject.companyCut = 0; 
        
        ledgerLog.push(acceptedProject);
        renderBoard();
        renderLogBoard();
        updateDashboardStats();
    }

    function finalizeProject(stakeId) {
        const stake = ledgerLog.find(p => p.id === stakeId);
        if (!stake || stake.status === 'finished') return;

        const confirmEnd = confirm("End this stake? Payout will be precisely calculated based on hours staked and active Spark.");
        if (!confirmEnd) return;

        const now = Date.now();
        let hoursStaked = (now - stake.timestampAccepted) / (1000 * 60 * 60);
        
        // Formula: Total Spark * Hours Left On Site = Total Contributed Spark Hours
        const sparkHoursContributed = stake.activeSpark * hoursStaked;
        
        // New Split: 95% to Sub, 5% to Company
        let finalSubPayout = Math.floor(sparkHoursContributed * subcontractorPayoutPercentage);
        let finalCompanyCut = Math.floor(sparkHoursContributed * companyCutPercentage);

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
            board.innerHTML = '<p style="color: #7f8c8d; font-style: italic;">No active or finalized construction.</p>';
            return;
        }

        let totalSubLiability = 0;
        let totalCompanyCut = 0;

        ledgerLog.forEach(stake => {
            let colorTier = getGeneratorColorByValue(stake.value);
            // Color logic for matching names maintained, new names follow tier arbitrary call.
            let displayColor = stake.category === 'service_structures' ? stake.requiredGenColor : stake.category;
            
            let div = document.createElement('div');
            
            if (stake.status === 'active') {
                totalSubLiability += Math.floor(stake.value * subcontractorPayoutPercentage); // Liability based on new 95/5 split
                div.className = `project-card ${displayColor}-gen`;
                div.innerHTML = `
                    <strong><span class="status-dot"></span>${stake.name} (Sub: ${stake.subcontractor})</strong>
                    <div class="meta-data">
                        <p><strong>Address:</strong> ${stake.address}</p>
                        <p><strong>Power:</strong> ${stake.activeSpark.toFixed(2)} Spark (${stake.gensDeployed} Gens)</p>
                        <p class="completion-highlight"><strong>Target Completion:</strong> ${stake.estimatedCompletion}</p>
                        <p><strong>Payout accumulating live based on new 95/5 split...</strong></p>
                    </div>
                    <button class="end-btn" onclick="finalizeProject(${stake.id})">End Stake / Calculate Split</button>
                `;
            } else {
                totalSubLiability += stake.actualPayout; 
                totalCompanyCut += stake.companyCut;
                
                div.className = `project-card completed-card ${displayColor}-gen`;
                div.innerHTML = `
                    <strong style="color: #7f8c8d;">[FINALIZED] ${stake.name}</strong>
                    <div class="meta-data">
                        <p><strong>Address:</strong> ${stake.address}</p>
                        <p><strong>Subcontractor:</strong> <span class="sub-name">${stake.subcontractor}</span></p>
                        <p><strong>Time Staked:</strong> ${stake.actualHoursStaked} Hours / ${stake.hoursToComplete.toFixed(2)} Required</p>
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
                    <h4>Total Owed to Fleet (New 95% Split)</h4>
                    <p>${totalSubLiability.toLocaleString()} UPX</p>
                </div>
                <div class="liability-col company-col">
                    <h4>Raytown Co. Cut (5%)</h4>
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
