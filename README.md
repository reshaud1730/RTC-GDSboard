# RTC-GDSboard
Dispatxh app for generators in the Upland Metaverse!
[GDS_HUB.htnl.txt](https://github.com/user-attachments/files/25804762/GDS_HUB.htnl.txt)
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
        
        /* Branded Logo Styling using the exact filename provided */
        .company-logo { height: 70px; width: auto; margin-right: 18px; border-radius: 4px; }
        
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
        
        /* Restored Progress Bar for Fractional Staking */
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
                <option value="">-- Choose Category --</option>
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

    // Generator Rules (Sparklet) - 30k Max Fuel Tank capacity
    const sparkletRules = {
        green: { min: 5000, max: 30000 },
        red: { min: 10000, max: 30000 },
        yellow: { min: 15000, max: 30000 }
    };

    const subcontractorPayoutPercentage = 0.95; // 95% to sub
    const companyCutPercentage = 0.05; // 5% company cut

    // Database
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
