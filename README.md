# RTC-GDSboard
Dispatxh app for generators in the Upland Metaverse!
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Raytown Construction Dispatch Command</title>
    <style>
        /* UPLAND RACING STYLE BACKGROUND */
        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
            background-color: #1a1c23; /* Deep Dark Space */
            background-image: 
                linear-gradient(rgba(26, 28, 35, 0.8), rgba(26, 28, 35, 0.95)), /* Dark overlay */
                url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI0MCIgaGVpZ2h0PSI0MCIgdmlld0JveD0iMCAwIDQwIDQwIj48ZyBmaWxsLXJ1bGU9ImV2ZW5vZGQiPjxnIGZpbGw9IiM0NDRBNTkiIGZpbGwtb3BhY2l0eT0iMC4wNSI+PHBhdGggZD0iTTAgMGg0MHY0MEgwVjB6bTIwIDIwaDIwdjIwSDIWMjB6TTAgMjBoMjB2MjBIMFYyMHoyMCAwaDIwdjIwSDIwVjB6Ii8+PC9nPjwvZz48L3N2Zz4='); /* Concrete Pavement Pattern */
            background-attachment: fixed;
            color: #ecf0f1; 
            margin: 0; 
            padding: 20px; 
            background-size: initial, 60px 60px; /* Specific concrete tile size */
        }
        
        .container { max-width: 1200px; margin: auto; }
        
        /* SLEEK GRADIENT HEADER */
        .dashboard-header { 
            display: flex; 
            justify-content: space-between; 
            align-items: center; 
            background: linear-gradient(135deg, #16191f, #34495e); /* Automotive metal gradient */
            color: white; 
            padding: 15px 20px; 
            border-radius: 8px; 
            margin-bottom: 20px; 
            box-shadow: 0 4px 15px rgba(0,0,0,0.5);
            border: 1px solid #34495e;
        }
        
        .company-logo { height: 70px; width: auto; margin-right: 18px; border-radius: 4px; }
        
        /* GLOWING STATS BAR */
        .stats-bar { 
            background: #2c3e50; 
            color: white; 
            padding: 15px 20px; 
            border-radius: 8px; 
            margin-bottom: 20px; 
            display: flex; 
            justify-content: space-between; 
            font-size: 1.2em; 
            font-weight: bold; 
            box-shadow: 0 0 10px rgba(52, 152, 219, 0.3); /* Neon Blue Glow */
            border: 1px solid rgba(52, 152, 219, 0.2);
        }
        
        #total-spark-stat { color: #f1c40f; font-weight: bold;}
        
        .panels { display: flex; gap: 20px; flex-wrap: wrap; align-items: flex-start; }
        
        /* FLOATING GLOW PANELS */
        .panel { 
            background: rgba(44, 62, 80, 0.8); /* Semi-transparent panel */
            backdrop-filter: blur(5px); /* Optional frost effect */
            padding: 20px; 
            border-radius: 8px; 
            box-shadow: 0 10px 30px rgba(0,0,0,0.4); 
            flex: 1; 
            min-width: 320px; 
            border: 1px solid rgba(255, 255, 255, 0.05);
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        
        .panel:hover {
            transform: translateY(-5px); /* Racing 'lift' effect */
            box-shadow: 0 15px 40px rgba(0,0,0,0.6), 0 0 15px rgba(52, 152, 219, 0.2);
        }
        
        .panel h3 { color: #3498db; margin-top: 0; font-weight: 800; text-transform: uppercase; letter-spacing: 1px; }
        .panel h4 { color: #ecf0f1; margin-bottom: 10px; }

        /* ADMIN RESTRICTED PANEL */
        .admin-only { display: none; background: rgba(26, 28, 35, 0.9); border: 2px solid #e67e22; box-shadow: 0 0 20px rgba(230, 126, 34, 0.2); }
        .admin-only h3 { color: #e67e22 !important;}

        label { font-weight: bold; font-size: 0.9em; color: #bdc3c7; display: block; margin-top: 10px; }
        
        /* MODERN DARK INPUTS */
        select, input { 
            width: 100%; 
            padding: 12px; 
            margin-top: 5px; 
            margin-bottom: 10px; 
            border-radius: 5px; 
            border: 1px solid #34495e; 
            background-color: #1a1c23;
            color: #ecf0f1;
            font-size: 16px; 
            box-sizing: border-box; 
            transition: border-color 0.2s;
        }
        
        select:focus, input:focus {
            border-color: #3498db;
            outline: none;
            box-shadow: 0 0 5px rgba(52, 152, 219, 0.5);
        }

        select:disabled { background-color: #2c3e50; cursor: not-allowed; color: #7f8c8d; }
        
        /* RACING BLUE BUTTON */
        button { 
            background: linear-gradient(135deg, #3498db, #16191f); /* Automotive gradient */
            color: white; 
            border: 1px solid #3498db; 
            cursor: pointer; 
            font-weight: bold; 
            text-transform: uppercase; 
            letter-spacing: 1px;
            padding: 12px;
            border-radius: 5px;
            font-size: 16px;
            transition: 0.2s; 
        }
        
        button:hover { 
            background: linear-gradient(135deg, #16191f, #3498db); 
            box-shadow: 0 0 15px rgba(52, 152, 219, 0.7); /* Electric Blue Glow */
        }
        
        /* DYNAMIC PROJECT CARDS */
        .project-card { 
            border-left: 5px solid #bdc3c7; 
            background: #2c3e50; 
            margin-bottom: 15px; 
            padding: 15px; 
            border-radius: 4px; 
            position: relative; 
            transition: transform 0.2s, box-shadow 0.2s;
            box-shadow: 0 4px 6px rgba(0,0,0,0.3);
        }
        
        .project-card:hover {
            transform: scale(1.02); /* Pop effect */
            box-shadow: 0 8px 15px rgba(0,0,0,0.5);
        }

        .green-gen { border-left-color: #2ecc71; box-shadow: 0 0 10px rgba(46, 204, 113, 0.2); }
        .red-gen { border-left-color: #e74c3c; box-shadow: 0 0 10px rgba(231, 76, 60, 0.2); }
        .yellow-gen { border-left-color: #f1c40f; box-shadow: 0 0 10px rgba(241, 196, 15, 0.2); }
        
        /* RACING PROGRESS BAR */
        .progress-bar { background: #34495e; border-radius: 4px; height: 10px; width: 100%; margin: 10px 0; overflow: hidden; border: 1px solid rgba(255,255,255,0.05); }
        .progress-fill { background: linear-gradient(90deg, #3498db, #ecf0f1); /* Electric gradient */ height: 100%; transition: width 0.3s ease; }
        
        .meta-data { font-size: 0.85em; color: #bdc3c7; line-height: 1.5; }
        .approve-btn { background: linear-gradient(135deg, #2ecc71, #16191f); border-color: #2ecc71; margin-top: 10px; }
        .deny-btn { background: linear-gradient(135deg, #e74c3c, #16191f); border-color: #e74c3c; margin-top: 5px; width: auto; padding: 10px 20px; }
        .end-btn { background: linear-gradient(135deg, #e67e22, #16191f); border-color: #e67e22; font-size: 0.8em; margin-top: 10px; }
        
        .completed-card { opacity: 0.6; background: #34495e; border-left-color: #7f8c8d !important; }
        
        .login-status-bar { font-size: 0.85em; text-align: right; margin-bottom: 10px; color: #bdc3c7; }
        .status-dot { height: 10px; width: 10px; background-color: #2ecc71; border-radius: 50%; display: inline-block; margin-right: 5px; animation: pulse 2s infinite; }
        @keyframes pulse { 0% { opacity: 1; } 50% { opacity: 0.4; } 100% { opacity: 1; } }
    </style>
</head>
<body>

<div class="container">
    <div class="login-status-bar">
        <span id="user-status">Public Access</span> | 
        <a href="#" id="auth-action" onclick="window.toggleAuth()" style="color:#3498db; text-decoration:none; font-weight:bold;">Admin Login</a>
    </div>

    <div class="dashboard-header">
        <div class="header-title-group" style="display:flex; align-items:center;">
            <img src="GDSlogo2.png" alt="Raytown Construction Logo" class="company-logo">
            <h2 style="margin: 0; text-transform: uppercase; font-weight: 800; letter-spacing: 2px;">Dispatch Command</h2>
        </div>
        <div id="live-clock" style="font-weight:bold; color: #bdc3c7;"></div>
    </div>

    <div class="stats-bar">
        <span>Active Spark on Field:</span>
        <span id="total-spark-stat">0.00 Spark</span>
    </div>

    <div class="panels">
        <div class="panel">
            <h3>Customer: Submit Project</h3>
            <p style="font-size: 0.85em; color: #bdc3c7;">Submit your property for approval & construction.</p>
            
            <label>Build Category:</label>
            <select id="category-select" onchange="window.populateProperties()">
                <option value="">-- Choose Category --</option>
                <option value="green">Green (Houses)</option>
                <option value="red">Red (Townhouses)</option>
                <option value="yellow">Yellow (Towers)</option>
                <option value="service_structures">Service Structures</option>
                <option value="custom">Custom / Manual Entry</option>
            </select>

            <div id="property-group">
                <label>Property Model:</label>
                <select id="property-select" disabled><option value="">-- Await Category --</option></select>
            </div>

            <div id="custom-group" style="display:none;">
                <label>Manual Sparklet Hours Required:</label>
                <input type="number" id="custom-hours" placeholder="e.g. 15000">
                <label>Project Name / Description:</label>
                <input type="text" id="custom-name" placeholder="e.g. Rare Fountain Build">
            </div>

            <label>Upland Address:</label>
            <input type="text" id="project-address" placeholder="e.g. 123 Upland Ave, NYC">

            <button onclick="window.submitForApproval()">Submit for Approval</button>
        </div>

        <div class="panel admin-only" id="admin-panel">
            <h3>Admin: Control Center</h3>
            <h4 style="margin-bottom:10px; color:#e67e22;">Pending Approval Queue</h4>
            <div id="pending-board"></div>
            
            <hr style="margin:20px 0; border:0; border-top:1px solid rgba(255,255,255,0.05);">
            
            <h4 style="margin-bottom:10px; color:#ecf0f1;">Live Payout Ledger</h4>
            <div id="ledger-board"></div>
            <button style="background:#9b59b6; margin-top:15px; border-color: #9b59b6;" onclick="window.exportToCSV()">Export Payout Ledger (CSV)</button>
        </div>

        <div class="panel">
            <h3>Fleet Dispatch Board</h3>
            <div id="dispatch-board"></div>
        </div>
    </div>
</div>

<script type="module">
    // (Firebase connection, property lists, auth logic, and mathematical algorithms are 100% intact)
    // Inherited from Turn 38 and validated.
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

    const properties = {
        green: [
            { name: "Luxury Modern House", value: 75000 }, { name: "Luxury Ranch House", value: 75000 },
            { name: "Cosy Cabin", value: 72000 }, { name: "Ranch House", value: 30000 }, { name: "Small Town House", value: 30000 }
        ],
        red: [
            { name: "Town House", value: 60000 }, { name: "Madrid Townhouse", value: 51000 },
            { name: "Buenos Aires Town House", value: 51000 }, { name: "Victorian House", value: 48000 }
        ],
        yellow: [
            { name: "Cypher Cyborgs Tower", value: 1500000 }, { name: "Apartment Building", value: 90000 },
            { name: "High-Rise Tower", value: 99000 }
        ],
        service_structures: [
            { name: "University", value: 147150 }, { name: "Hospital", value: 80100 },
            { name: "Planetarium", value: 90450 }, { name: "Fire HQ", value: 62100 },
            { name: "Kiosk", value: 7200 }, { name: "Supermarket", value: 66150 }
        ]
    };

    window.toggleAuth = () => {
        if (auth.currentUser) { signOut(auth); } 
        else {
            const email = prompt("Admin Email:");
            const pass = prompt("Password:");
            if(email && pass) signInWithEmailAndPassword(auth, email, pass).catch(() => alert("Access Denied"));
        }
    };

    onAuthStateChanged(auth, (user) => {
        document.querySelectorAll('.admin-only').forEach(el => el.style.display = user ? 'block' : 'none');
        document.getElementById('user-status').innerText = user ? "Mode: Admin" : "Mode: Public";
        document.getElementById('auth-action').innerText = user ? "Logout" : "Admin Login";
        renderLedgerBoard();
    });

    window.submitForApproval = () => {
        const cat = document.getElementById('category-select').value;
        const addr = document.getElementById('project-address').value.trim();
        let name, value, color;

        if(!cat || !addr) return alert("Address and Category required.");

        if(cat === 'custom') {
            name = document.getElementById('custom-name').value.trim();
            value = parseInt(document.getElementById('custom-hours').value);
            if(!name || isNaN(value)) return alert("Please fill Custom fields.");
            color = value < 40000 ? 'green' : value < 70000 ? 'red' : 'yellow';
        } else {
            const idx = document.getElementById('property-select').value;
            if(idx === "") return alert("Please select a property.");
            const p = properties[cat][idx];
            name = p.name;
            value = p.value;
            color = cat === 'service_structures' ? (value < 40000 ? 'green' : value < 70000 ? 'red' : 'yellow') : cat;
        }

        push(ref(db, 'pendingProjects'), {
            name, address: addr, value, requiredGenColor: color, maxGens: Math.ceil(value/10000)
        });
        alert("Submitted for approval!");
        location.reload(); 
    };

    onValue(ref(db, 'pendingProjects'), (snap) => {
        const board = document.getElementById('pending-board');
        if(!board) return;
        board.innerHTML = snap.val() ? "" : "<i>No pending submissions.</i>";
        if(snap.val()) {
            Object.entries(snap.val()).forEach(([id, data]) => {
                board.innerHTML += `<div class="project-card"><strong>${data.name}</strong> (${data.value} Hrs)<br>${data.address}<br><button class="approve-btn" onclick="window.approveProject('${id}')">Approve</button><button class="deny-btn" onclick="window.denyProject('${id}')">Deny</button></div>`;
            });
        }
    });

    window.approveProject = (id) => {
        onValue(ref(db, `pendingProjects/${id}`), (snap) => {
            const d = snap.val();
            if(d) { push(ref(db, 'activeProjects'), { ...d, currentGens: 0 }); remove(ref(db, `pendingProjects/${id}`)); }
        }, { onlyOnce: true });
    };

    window.denyProject = (id) => { if(confirm("Deny this project?")) remove(ref(db, `pendingProjects/${id}`)); };

    onValue(ref(db, 'activeProjects'), (snap) => {
        const board = document.getElementById('dispatch-board');
        board.innerHTML = snap.val() ? "" : "<i>No active projects.</i>";
        window.activeProjectsArr = snap.val() ? Object.entries(snap.val()).map(([id, data]) => ({id, ...data})) : [];
        if(snap.val()) {
            Object.entries(snap.val()).forEach(([id, p]) => {
                const avail = p.maxGens - p.currentGens;
                board.innerHTML += `<div class="project-card ${p.requiredGenColor}-gen"><strong>${p.name}</strong><br>${p.address}<br><div class="meta-data">Slots: ${avail} Open</div><div class="progress-bar"><div class="progress-fill" style="width:${(p.currentGens/p.maxGens)*100}%"></div></div><button onclick="window.acceptProject('${id}')">Claim Slot</button></div>`;
            });
        }
    });

    window.acceptProject = (id) => {
        const p = window.activeProjectsArr.find(x => x.id === id);
        const user = prompt("Upland Username:");
        if(!user) return;
        const gens = parseInt(prompt(`How many generators? (Max ${p.maxGens - p.currentGens})`));
        if(isNaN(gens) || gens <= 0 || gens > (p.maxGens - p.currentGens)) return alert("Invalid count.");
        push(ref(db, 'ledgerLog'), { name: p.name, address: p.address, subcontractor: user, activeSpark: gens * 1.0, status: 'active', timestamp: Date.now(), hoursNeeded: p.value/(gens * 1.0), color: p.requiredGenColor });
        const newCount = p.currentGens + gens;
        if(newCount >= p.maxGens) remove(ref(db, `activeProjects/${id}`));
        else update(ref(db, `activeProjects/${id}`), { currentGens: newCount });
    };

    onValue(ref(db, 'ledgerLog'), (snap) => {
        window.ledgerDataArr = snap.val() ? Object.entries(snap.val()).map(([id, data]) => ({id, ...data})) : [];
        renderLedgerBoard();
        const total = window.ledgerDataArr.reduce((acc, s) => s.status === 'active' ? acc + s.activeSpark : acc, 0);
        document.getElementById('total-spark-stat').innerText = total.toFixed(2) + " Spark";
    });

    function renderLedgerBoard() {
        const board = document.getElementById('ledger-board');
        if(!board) return;
        board.innerHTML = window.ledgerDataArr.length ? "" : "<i>No ledger history.</i>";
        window.ledgerDataArr.forEach(s => {
            const isAdmin = auth.currentUser;
            const btn = (isAdmin && s.status === 'active') ? `<button class="end-btn" onclick="window.finalizeStake('${s.id}')">Finalize Payout (95/5)</button>` : "";
            board.innerHTML += `<div class="project-card ${s.status === 'finished' ? 'completed-card' : ''}"><strong>${s.name}</strong> (${s.subcontractor})<br>${s.status === 'active' ? 'Building...' : 'PAID'} ${btn}</div>`;
        });
    }

    window.finalizeStake = (id) => {
        const s = window.ledgerDataArr.find(x => x.id === id);
        const hours = Math.min(s.hoursNeeded, (Date.now() - s.timestamp) / 3600000);
        const payout = Math.floor((s.activeSpark * hours) * 0.95);
        update(ref(db, `ledgerLog/${id}`), { status: 'finished', payout, actualHours: hours.toFixed(2), companyCut: Math.floor(payout * 0.05) });
    };

    window.populateProperties = () => {
        const cat = document.getElementById('category-select').value;
        const pGroup = document.getElementById('property-group');
        const cGroup = document.getElementById('custom-group');
        const sel = document.getElementById('property-select');

        if(cat === 'custom') {
            pGroup.style.display = 'none';
            cGroup.style.display = 'block';
        } else {
            pGroup.style.display = 'block';
            cGroup.style.display = 'none';
            sel.innerHTML = '<option value="">-- Select --</option>';
            if(cat && properties[cat]) {
                sel.disabled = false;
                properties[cat].forEach((p, i) => { sel.innerHTML += `<option value="${i}">${p.name}</option>`; });
            } else { sel.disabled = true; }
        }
    };

    window.exportToCSV = () => {
        let csv = "Subcontractor,Property,Payout(95%),Hours\n";
        window.ledgerDataArr.forEach(s => { if(s.status === 'finished') csv += `${s.subcontractor},${s.name},${s.payout},${s.actualHours}\n`; });
        const blob = new Blob([csv], { type: 'text/csv' });
        const a = document.createElement('a');
        a.href = URL.createObjectURL(blob); a.download = 'Raytown_Ledger.csv'; a.click();
    };

    setInterval(() => { document.getElementById('live-clock').innerText = new Date().toLocaleString(); }, 1000);
</script>
</body>
</html>
