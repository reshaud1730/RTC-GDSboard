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
        .container { max-width: 1200px; margin: auto; }
        .dashboard-header { display: flex; justify-content: space-between; align-items: center; background: #2c3e50; color: white; padding: 15px 20px; border-radius: 8px; margin-bottom: 20px; }
        .company-logo { height: 70px; width: auto; margin-right: 18px; border-radius: 4px; }
        .stats-bar { background: #34495e; color: white; padding: 15px 20px; border-radius: 8px; margin-bottom: 20px; display: flex; justify-content: space-between; font-size: 1.2em; font-weight: bold; }
        #total-spark-stat { color: #f1c40f; }
        .panels { display: flex; gap: 20px; flex-wrap: wrap; }
        .panel { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1); flex: 1; min-width: 300px; }
        .admin-only { display: none; }
        label { font-weight: bold; font-size: 0.9em; color: #555; }
        select, button, input { width: 100%; padding: 10px; margin-top: 5px; margin-bottom: 15px; border-radius: 5px; border: 1px solid #ccc; font-size: 16px; box-sizing: border-box; }
        select:disabled { background-color: #e9ecef; cursor: not-allowed; }
        button { background-color: #3498db; color: white; border: none; cursor: pointer; font-weight: bold; }
        button:hover { background-color: #2980b9; }
        .project-card { border-left: 5px solid gray; background: #f9f9f9; margin-bottom: 15px; padding: 15px; border-radius: 4px; position: relative; }
        .green-gen { border-left-color: #2ecc71; }
        .red-gen { border-left-color: #e74c3c; }
        .yellow-gen { border-left-color: #f1c40f; }
        .progress-bar { background: #e0e0e0; border-radius: 4px; height: 10px; width: 100%; margin-top: 5px; overflow: hidden; }
        .progress-fill { background: #3498db; height: 100%; transition: width 0.3s ease; }
        .meta-data { font-size: 0.85em; color: #555; margin-top: 10px; }
        .end-btn { margin-top: 10px; background-color: #e74c3c; font-size: 0.8em; }
        .completed-card { opacity: 0.8; background: #ecf0f1; }
        .login-box { font-size: 0.85em; text-align: right; margin-bottom: 10px; color: #7f8c8d; }
        .liability-box { background: #e8f8f5; border: 2px solid #2ecc71; padding: 15px; text-align: center; border-radius: 8px; margin-top: 20px; display: flex; justify-content: space-between; }
        .status-dot { height: 10px; width: 10px; background-color: #2ecc71; border-radius: 50%; display: inline-block; margin-right: 5px; animation: pulse 2s infinite; }
        @keyframes pulse { 0% { opacity: 1; } 50% { opacity: 0.4; } 100% { opacity: 1; } }
    </style>
</head>
<body>

<div class="container">
    <div class="login-box">
        <span id="user-status">Fleet Access</span> | 
        <a href="#" id="auth-action" onclick="window.toggleAuth()" style="color:#3498db; text-decoration:none;">Admin Login</a>
    </div>

    <div class="dashboard-header">
        <div class="header-title-group" style="display:flex; align-items:center;">
            <img src="GDSlogo2.png" alt="Raytown Construction Logo" class="company-logo">
            <h2 style="margin: 0;">Dispatch Command</h2>
        </div>
        <div id="live-clock" style="font-weight:bold;"></div>
    </div>

    <div class="stats-bar">
        <span>Active Spark on Field:</span>
        <span id="total-spark-stat">0.00 Spark</span>
    </div>

    <div class="panels">
        <div class="panel admin-only" id="admin-panel">
            <h3>Admin: Deploy Project</h3>
            <label>Category:</label>
            <select id="category-select" onchange="window.populateProperties()">
                <option value="">-- Choose Category --</option>
                <option value="green">Green (Houses)</option>
                <option value="red">Red (Townhouses)</option>
                <option value="yellow">Yellow (Towers)</option>
                <option value="service_structures">Service Structures</option>
            </select>
            <label>Property Type:</label>
            <select id="property-select" disabled><option value="">-- Await Category --</option></select>
            <label>Address:</label>
            <input type="text" id="project-address" placeholder="Upland Address">
            <button onclick="window.listProject()">Deploy Project</button>
        </div>

        <div class="panel">
            <h3>Fleet Dispatch Board</h3>
            <div id="dispatch-board"></div>
        </div>

        <div class="panel">
            <h3>Construction Log</h3>
            <div id="completed-board"></div>
            <button class="admin-only" style="background:#9b59b6; margin-top:15px;" onclick="window.exportToCSV()">Export Ledger (CSV)</button>
        </div>
    </div>
</div>

<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/12.10.0/firebase-app.js";
    import { getDatabase, ref, push, onValue, update, remove } from "https://www.gstatic.com/firebasejs/12.10.0/firebase-database.js";
    import { getAuth, signInWithEmailAndPassword, signOut, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/12.10.0/firebase-auth.js";

    const firebaseConfig = {
        apiKey: "AIzaSyBIVDWi2uB1wVEfVhfYf1LHKy_Y2-w-ztI",
        authDomain: "rtc-gen-dispatch.firebaseapp.com",
        databaseURL: "https://rtc-gen-dispatch-default-rtdb.firebaseio.com",
        projectId: "rtc-gen-dispatch",
        storageBucket: "rtc-gen-dispatch.firebasestorage.app",
        messagingSenderId: "150229911040",
        appId: "1:150229911040:web:69f09fba247e7429dd7ae5"
    };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);
    const auth = getAuth(app);

    // Database of Properties
    const properties = {
        green: [{ name: "Luxury Modern House", value: 75000 }, { name: "Cosy Cabin", value: 72000 }, { name: "Ranch House", value: 30000 }],
        red: [{ name: "Town House", value: 60000 }, { name: "Madrid Townhouse", value: 51000 }],
        yellow: [{ name: "Cypher Cyborgs Tower", value: 1500000 }, { name: "Apartment Building", value: 90000 }],
        service_structures: [
            { name: "University", value: 147150 }, { name: "Hospital", value: 80100 }, { name: "High School", value: 110700 },
            { name: "Museum", value: 75150 }, { name: "Post Office Center", value: 74250 }, { name: "Fire HQ", value: 62100 },
            { name: "Supermarket", value: 66150 }, { name: "Courthouse", value: 34650 }, { name: "Kiosk", value: 7200 }
        ]
    };

    // Auth Actions
    window.toggleAuth = () => {
        if (auth.currentUser) { signOut(auth); } 
        else {
            const email = prompt("Admin Email:");
            const pass = prompt("Password:");
            if(email && pass) signInWithEmailAndPassword(auth, email, pass).catch(() => alert("Access Denied"));
        }
    };

    onAuthStateChanged(auth, (user) => {
        const admins = document.querySelectorAll('.admin-only');
        admins.forEach(el => el.style.display = user ? 'block' : 'none');
        document.getElementById('user-status').innerText = user ? "Logged in as Admin" : "Fleet Access";
        document.getElementById('auth-action').innerText = user ? "Logout" : "Admin Login";
        renderLogBoard(); // Re-render to show/hide admin buttons
    });

    // Core Logic
    onValue(ref(db, 'activeProjects'), (snapshot) => {
        window.activeProjects = snapshot.val() ? Object.entries(snapshot.val()).map(([id, data]) => ({id, ...data})) : [];
        renderBoard();
    });

    onValue(ref(db, 'ledgerLog'), (snapshot) => {
        window.ledgerLog = snapshot.val() ? Object.entries(snapshot.val()).map(([id, data]) => ({id, ...data})) : [];
        renderLogBoard();
        const total = window.ledgerLog.reduce((acc, s) => s.status === 'active' ? acc + s.activeSpark : acc, 0);
        document.getElementById('total-spark-stat').innerText = total.toFixed(2) + " Spark";
    });

    window.populateProperties = () => {
        const cat = document.getElementById('category-select').value;
        const sel = document.getElementById('property-select');
        sel.innerHTML = '<option value="">-- Select --</option>';
        if(cat && properties[cat]) {
            sel.disabled = false;
            properties[cat].forEach((p, i) => { sel.innerHTML += `<option value="${i}">${p.name} (${p.value.toLocaleString()} Hrs)</option>`; });
        } else { sel.disabled = true; }
    };

    window.listProject = () => {
        const cat = document.getElementById('category-select').value;
        const idx = document.getElementById('property-select').value;
        const addr = document.getElementById('project-address').value;
        if(!addr || idx === "") return alert("Check inputs");
        const p = properties[cat][idx];
        const color = cat === 'service_structures' ? (p.value < 40000 ? 'green' : p.value < 70000 ? 'red' : 'yellow') : cat;
        push(ref(db, 'activeProjects'), { name: p.name, address: addr, value: p.value, maxGens: Math.ceil(p.value/10000), currentGens: 0, requiredGenColor: color });
        document.getElementById('project-address').value = "";
    };

    function renderBoard() {
        const board = document.getElementById('dispatch-board');
        board.innerHTML = window.activeProjects.length ? "" : "<i>No open dispatches.</i>";
        window.activeProjects.forEach(proj => {
            const avail = proj.maxGens - proj.currentGens;
            board.innerHTML += `
                <div class="project-card ${proj.requiredGenColor}-gen">
                    <strong>${proj.name}</strong><br>${proj.address}<br>
                    <div class="meta-data">Slots: ${avail} Open / ${proj.maxGens}</div>
                    <div class="progress-bar"><div class="progress-fill" style="width:${(proj.currentGens/proj.maxGens)*100}%"></div></div>
                    <button style="margin-top:10px;" onclick="window.acceptProject('${proj.id}')">Claim Slots</button>
                </div>`;
        });
    }

    window.acceptProject = (id) => {
        const p = window.activeProjects.find(x => x.id === id);
        const user = prompt("Upland Username:");
        if(!user) return;
        const gens = parseInt(prompt(`Gens to stake? (Max ${p.maxGens - p.currentGens})`));
        if(isNaN(gens) || gens <= 0 || gens > (p.maxGens - p.currentGens)) return alert("Invalid");
        
        const power = gens * 1.0; // Assuming 1.0 Spark per gen for math
        push(ref(db, 'ledgerLog'), { 
            name: p.name, address: p.address, subcontractor: user, activeSpark: power, 
            status: 'active', timestamp: Date.now(), hoursNeeded: p.value/power, color: p.requiredGenColor 
        });

        const newGens = p.currentGens + gens;
        if(newGens >= p.maxGens) remove(ref(db, `activeProjects/${id}`));
        else update(ref(db, `activeProjects/${id}`), { currentGens: newGens });
    };

    window.finalizeProject = (id) => {
        const s = window.ledgerLog.find(x => x.id === id);
        const hours = Math.min(s.hoursNeeded, (Date.now() - s.timestamp) / 3600000);
        const payout = Math.floor((s.activeSpark * hours) * 0.95);
        update(ref(db, `ledgerLog/${id}`), { status: 'finished', payout, actualHours: hours.toFixed(2), companyCut: Math.floor(payout * 0.05) });
    };

    function renderLogBoard() {
        const board = document.getElementById('completed-board');
        board.innerHTML = "";
        window.ledgerLog.forEach(s => {
            const adminBtn = (auth.currentUser && s.status === 'active') ? `<button class="end-btn" onclick="window.finalizeProject('${s.id}')">Finalize Payout</button>` : "";
            const statusText = s.status === 'active' ? `<span class="status-dot"></span>Active` : `Paid: ${s.payout.toLocaleString()} UPX`;
            board.innerHTML += `<div class="project-card ${s.status === 'finished' ? 'completed-card' : ''}">
                <strong>${s.name}</strong> (${s.subcontractor})<br>${statusText} ${adminBtn}
            </div>`;
        });
    }

    window.exportToCSV = () => {
        let csv = "Subcontractor,Property,Payout(95%)\n";
        window.ledgerLog.forEach(s => { if(s.status === 'finished') csv += `${s.subcontractor},${s.name},${s.payout}\n`; });
        const blob = new Blob([csv], { type: 'text/csv' });
        const a = document.createElement('a');
        a.href = URL.createObjectURL(blob); a.download = 'Raytown_Ledger.csv'; a.click();
    };

    setInterval(() => { document.getElementById('live-clock').innerText = new Date().toLocaleString(); }, 1000);
</script>
</body>
</html>
