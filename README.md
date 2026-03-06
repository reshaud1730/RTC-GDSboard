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
        
        /* Company Logo */
        .company-logo { height: 70px; width: auto; margin-right: 18px; border-radius: 4px; }
        
        .stats-bar { background: #34495e; color: white; padding: 15px 20px; border-radius: 8px; margin-bottom: 20px; display: flex; justify-content: space-between; font-size: 1.2em; font-weight: bold; box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
        #total-spark-stat { color: #f1c40f; font-size: 1.4em; } 

        .panels { display: flex; gap: 20px; flex-wrap: wrap; align-items: flex-start; }
        .panel { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1); flex: 1; min-width: 300px; }
        label { font-weight: bold; font-size: 0.9em; color: #555; }
        select, button, input { width: 100%; padding: 10px; margin-top: 5px; margin-bottom: 15px; border-radius: 5px; border: 1px solid #ccc; font-size: 16px; box-sizing: border-box; }
        
        /* Ensures disabled selects look visually locked */
        select:disabled { background-color: #e9ecef; cursor: not-allowed; color: #999; }
        
        button { background-color: #3498db; color: white; border: none; cursor: pointer; font-weight: bold; margin-bottom: 0; }
        button:hover { background-color: #2980b9; }
        .export-btn { background-color: #9b59b6; margin-top: 20px; }
        .export-btn:hover { background-color: #8e44ad; }
        
        .project-card { border-left: 5px solid gray; background: #f9f9f9; margin-bottom: 15px; padding: 15px; border-radius: 4px; position: relative; }
        .green-gen { border-left-color: #2ecc71; }
        .red-gen { border-left-color: #e74c3c; }
        .yellow-gen { border-left-color: #f1c40f; }
        
        .progress-bar { background: #e0e0e0; border-radius: 4px; height: 10px; width: 100%; margin-top: 5px; overflow: hidden; }
        .progress-fill { background: #3498db; height: 100%; transition: width 0.3s ease; }
        
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
            <img src="GDSlogo2.png" alt="Raytown Construction Logo" class="company-logo" id="company-logo">
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
                <option value="">-- Choose Category First --</option>
                <option value="green">Green (Houses & Estates)</option>
                <option value="red">Red (Townhouses & Mid-Sized)</option>
                <option value="yellow">Yellow (Towers & Large Apts)</option>
                <option value="service_structures">Service Structures</option>
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
    // Live Clock Logic
    function updateClock() {
        const now = new Date();
        document.getElementById('live-clock').innerText = now.toLocaleString('en-US');
    }
    setInterval(updateClock, 1000);
    updateClock();

    // Generator Rules
    const sparkletRules = {
        green: { min: 5000, max: 30000 },
        red: { min: 10000, max: 30000 },
        yellow: { min: 15000, max: 30000 }
    };

    const subcontractorPayoutPercentage = 0.95; 
    const companyCutPercentage = 0.05; 

    // Database
    const properties = {
        green: [
            { name: "Luxury Modern House", value: 75000 }, { name: "Luxury Ranch House", value: 75000 },
            { name: "The Filigree House", value: 75000 }, { name: "Cosy Cabin", value: 72000 },
            { name: "Ivory Rising Pearl", value: 72000 }, { name: "Micro House", value: 72000 },
            { name: "Spanish Luxus Villa", value: 54000 }, { name: "Mansao Brisa do Oceano", value: 48000 },
            { name: "Paris Estate", value: 48000 }, { name: "Super Luxury Ranch House", value: 48000 },
            { name: "Bermuda Luxe Ranch Estate", value: 45000 }, { name: "Contemporary House", value: 45000 },
            { name: "Genesis Modern house 25", value: 45000 }, { name: "London Manor", value: 45000 },
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
            { name: "Luxury Tower", value: 57000 }, { name: "Palacio Da Luz", value: 54000 },
            { name: "CMavs KCAD", value: 54000 }, { name: "Bermuda Breeze Apartments", value: 54000 },
            { name: "Residential Tower", value: 51000 }, { name: "Glass Tower", value: 51000 },
            { name: "Prospector Steampunk London Apartment", value: 51000 }
        ],
        service_structures: [
            { name: "University", value: 147150 }, { name: "High School", value: 110700 },
            { name: "Planitarium", value: 90450 }, { name: "Hospital", value: 80100 },
            { name: "Natural History Museum", value: 75150 }, { name: "Post Office Dist. Center", value: 74250 },
            { name: "Performing Arts", value: 73800 }, { name: "Modern Hotel", value: 70650 },
            { name: "Sports Courts", value: 68400 }, { name: "Science Museum", value: 67500 },
            { name: "Supermarket Super Store", value: 66150 }, { name: "Fire Station HQ", value: 62100 },
            { name: "Department Store", value: 49050 }, { name: "Motel", value: 38700 },
            { name: "Night Club", value: 37800 }, { name: "Farmers Market", value: 36900 },
            { name: "Public Pool", value: 35550 }, { name: "Courthouse", value: 34650 },
            { name: "Clinic", value: 32400 }, { name: "Recycling Center", value: 32400 },
            { name: "Aquarium", value: 34200 }, { name: "Movie Theater", value: 31950 },
            { name: "Bowling Alley", value: 31050 }, { name: "Community Center", value: 31050 },
            { name: "DMV", value: 31050 }, { name: "Brewery", value: 30150 },
            { name: "Neighborhood Police Station", value: 27450 }, { name: "Library", value: 27000 },
            { name: "Comedy Club", value: 26550 }, { name: "Animal Shelter", value: 26100 },
            { name: "Art Museum", value: 24300 }, { name: "Old Asylum", value: 23400 },
            { name: "Assisted Living Center", value: 23400 }, { name: "Mini Golf", value: 22950 },
            { name: "Fish Market", value: 22500 }, { name: "Physical Therapy Clinic", value: 19800 },
            { name: "Ice Rink", value: 19350 }, { name: "Ramen Restaurant", value: 19350 },
            { name: "Luxury Restaurant", value: 18900 }, { name: "Oil Change Center", value: 18450 },
            { name: "Fire Station", value: 18000 }, { name: "Veterinary Clinic", value: 18000 },
            { name: "Fine Dining Restaurant", value: 17550 }, { name: "Electronics Store", value: 17550 },
            { name: "Auto Repair Shop", value: 17100 }, { name: "Supermarket", value: 16650 },
            { name: "Dentistry", value: 16200 }, { name: "Martial Arts Dojo", value: 16200 },
            { name: "Day Care Center", value: 15750 }, { name: "Pool Hall", value: 15750 },
            { name: "Gym", value: 15300 }, { name: "Laundromat", value: 15300 },
            { name: "Auto Carwash", value: 13950 }, { name: "Funeral Home", value: 13950 },
            { name: "Jewelry", value: 13050 }, { name: "Casual Dining Restaurant", value: 12600 },
            { name: "Coffee Stand", value: 12600 }, { name: "Bank", value: 12600 },
            { name: "Fast Food Joint", value: 12600 }, { name: "Post Office", value: 12150 },
            { name: "Luxury Fashion Boutique", value: 12150 }, { name: "Barbershop", value: 11700 },
            { name: "Coffee Shop", value: 11700 }, { name: "Sneaker Store", value: 11700 },
            { name: "Dry Cleaner", value: 10800 }, { name: "Tire Shop", value: 10800 },
            { name: "Toy Store", value: 10800 }, { name: "Pub", value: 10350 },
            { name: "Optometrist", value: 10350 }, { name: "Arcade", value: 9900 },
            { name: "Ice Cream Parlor", value: 9900 }, { name: "Bodega", value: 9900 },
            { name: "Butcher Shop", value: 9900 }, { name: "Pharmacy", value: 9450 },
            { name: "Nail Salon", value: 9000 }, { name: "Massage Parlor", value: 8550 },
            { name: "Pet Shop", value: 8550 }, { name: "Pawn Shop", value: 8550 },
            { name: "Bakery", value: 8100 }, { name: "Bookstore", value: 8100 },
            { name: "Kiosk - Gelato", value: 7200 }, { name: "Kiosk - Taco", value: 7200 },
            { name: "Kiosk - Sushi", value: 7200 }, { name: "Kiosk - Donuts", value: 7200 },
            { name: "Kiosk - Fried Chicken", value: 7200 }, { name: "Crepe Stand", value: 7200 },
            { name: "Cinnamon Bun Stand", value: 7200 }, { name: "Fish Ball Stand", value: 7200 },
            { name: "Kiosk - Hot Dog", value: 6750 }, { name: "Kiosk - Pizza", value: 6750 },
            { name: "Kiosk - Hamburger", value: 6750 }, { name: "Empanada Stand", value: 6750 },
            { name: "Fish Deli", value: 6750 }, { name: "Sausage Stand", value: 6300 },
            { name: "Information Kiosk", value: 4500 }, { name: "Newspaper Stand", value: 4050 },
            { name: "Bus Stop", value: 3150 }
        ]
    };

    function getGeneratorColorByValue(value) {
        if (value < 40000) return 'green';
        else if (value < 70000) return 'red';
        else return 'yellow';
    }

    let activeProjects = [];
    let ledgerLog = [];

    function updateDashboardStats() {
        let totalActiveSpark = 0;
        ledgerLog.forEach(proj => {
            if (proj.status === 'active') totalActiveSpark += proj.activeSpark;
        });
        document.getElementById('total-spark-stat').innerText = totalActiveSpark.toFixed(2) + ' Spark';
    }

    // THIS FUNCTION POPULATES THE DROPDOWN
    function populateProperties() {
        const catSelect = document.getElementById('category-select').value;
        const propSelect = document.getElementById('property-select');
        
        propSelect.innerHTML = '<option value="">-- Select Property --</option>';
        if(catSelect && catSelect !== "") {
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
        
        let requiredGenColor = catSelect === 'service_structures' ? getGeneratorColorByValue(selectedProp.value) : catSelect;

        const project = {
            id: Date.now(),
            category: catSelect,
            name: selectedProp.name,
            address: addressInput,
            value: selectedProp.value,
            maxGens: maxGenerators,
            currentGens: 0, 
            timeListed: new Date().toLocaleString(),
            requiredGenColor: requiredGenColor 
        };

        activeProjects.push(project);
        renderBoard();
        
        document.getElementById('category-select').value = '';
        document.getElementById('project-address').value = '';
        populateProperties(); // Reset dropdown
    }

    function renderBoard() {
        const filter = document.getElementById('filter-select').value;
        const board = document.getElementById('dispatch-board');
        board.innerHTML = '';

        if (activeProjects.length === 0) {
            board.innerHTML = '<p style="color: #7f8c8d; font-style: italic;">No active projects awaiting dispatch Board.</p>';
            return;
        }

        activeProjects.forEach(proj => {
            if(filter === 'all' || filter === proj.category) {
                let displayColor = proj.requiredGenColor;
                const rules = sparkletRules[displayColor];
                
                const availableSlots = proj.maxGens - proj.currentGens;
                const fillPercentage = (proj.currentGens / proj.maxGens) * 100;
                
                let div = document.createElement('div');
                div.className = `project-card ${displayColor}-gen`;
                
                div.innerHTML = `
                    <strong style="font-size: 1.1em; color: #2c3e50;">${proj.name}</strong>
                    <div class="meta-data">
                        <p><strong>Address:</strong> ${proj.address}</p>
                        <p><strong>Required Gen Color:</strong> <span style="text-transform: capitalize; font-weight:bold;">${displayColor}</span></p>
                        <p><strong>Available Slots:</strong> <span style="color: #d35400; font-weight: bold;">${availableSlots} Open</span> (Max ${proj.maxGens})</p>
                        <div class="progress-bar"><div class="progress-fill" style="width: ${fillPercentage}%"></div></div>
                        <p><strong>Sparklet Rules:</strong> ${rules.min.toLocaleString()} to ${rules.max.toLocaleString()} Sparklet per Gen</p>
                        <p><strong>Payout Rate:</strong> 95% of build cost to Sub, 5% to Company</p>
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
        
        let colorForRules = proj.requiredGenColor;
        const rules = sparkletRules[colorForRules];
        
        const availableSlots = proj.maxGens - proj.currentGens;

        const username = prompt("Enter your Upland Username to claim generators on this dispatch:");
        if (!username || username.trim() === "") { alert("Username is required."); return; }

        let gensDeployed = prompt(`How many generators are you deploying? \n(Available Slots Remaining: ${availableSlots})`);
        gensDeployed = parseInt(gensDeployed);
        if (isNaN(gensDeployed) || gensDeployed <= 0 || gensDeployed > availableSlots) {
            alert(`Invalid entry. You must deploy between 1 and ${availableSlots} generators.`);
            return;
        }

        let sparkletPerGen = prompt(`How much Sparklet are you loading PER generator?\n(Required for ${colorForRules}: Min ${rules.min.toLocaleString()}, Max ${rules.max.toLocaleString()})`);
        sparkletPerGen = parseInt(sparkletPerGen.replace(/,/g, ''));

        if (isNaN(sparkletPerGen) || sparkletPerGen < rules.min || sparkletPerGen > rules.max) {
            alert(`Invalid Sparklet amount. A ${colorForRules} generator requires between ${rules.min.toLocaleString()} and ${rules.max.toLocaleString()} Sparklet.`);
            return;
        }

        const totalSparkletStaked = gensDeployed * sparkletPerGen;
        const totalSpark = totalSparkletStaked / 1000; 
        const hoursNeeded = proj.value / totalSpark; 
        
        const now = Date.now();
        const completionDate = new Date(now + (hoursNeeded * 60 * 60 * 1000));

        const stakeRecord = {
            id: Date.now() + Math.random(), 
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
            actualHoursStaked: 0,
            requiredGenColorTier: proj.requiredGenColor,
            hoursToComplete: hoursNeeded,
            estimatedCompletion: completionDate.toLocaleString('en-US', { 
                weekday: 'short', month: 'short', day: 'numeric', hour: '2-digit', minute: '2-digit' 
            })
        };
        
        ledgerLog.push(stakeRecord);
        
        proj.currentGens += gensDeployed;
        
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

        const confirmEnd = confirm("End this stake? Payout will be precisely calculated based on hours staked and active Spark.");
        if (!confirmEnd) return;

        const now = Date.now();
        let hoursStaked = (now - stake.timestampAccepted) / (1000 * 60 * 60);
        
        if (hoursStaked > stake.hoursToComplete) {
            hoursStaked = stake.hoursToComplete;
        }

        const sparkHoursContributed = stake.activeSpark * hoursStaked;
        
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
            let displayColor = stake.requiredGenColorTier ? stake.requiredGenColorTier : stake.category;
            let div = document.createElement('div');
            
            if (stake.status === 'active') {
                totalSubLiability += Math.floor((stake.activeSpark * stake.hoursToComplete) * subcontractorPayoutPercentage); 
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
        csvContent += "Stake ID,Status,Subcontractor,Address,Property Name,Required Gen Color Tier,Total Active Spark,Hours Required,Actual Hours Staked,Sub Payout (95%),Company Cut (5%)\n";

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
                row.requiredGenColorTier, 
                row.activeSpark.toFixed(2),
                row.hoursToComplete.toFixed(2),
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
