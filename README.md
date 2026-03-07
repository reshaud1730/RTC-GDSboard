# RTC-GDSboard
Dispatxh app for generators in the Upland Metaverse!
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Raytown Construction Dispatch Command</title>
    <style>
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background-color: #1a1c23; background-image: linear-gradient(rgba(26, 28, 35, 0.8), rgba(26, 28, 35, 0.95)), url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSI0MCIgaGVpZ2h0PSI0MCIgdmlld0JveD0iMCAwIDQwIDQwIj48ZyBmaWxsLXJ1bGU9ImV2ZW5vZGQiPjxnIGZpbGw9IiM0NDRBNTkiIGZpbGwtb3BhY2l0eT0iMC4wNSI+PHBhdGggZD0iTTAgMGg0MHY0MEgwVjB6bTIwIDIwaDIwdjIwSDIWMjB6TTAgMjBoMjB2MjBIMFYyMHoyMCAwaDIwdjIwSDIwVjB6Ii8+PC9nPjwvZz48L3N2Zz4='); background-attachment: fixed; color: #ecf0f1; margin: 0; padding: 20px; background-size: initial, 60px 60px; }
        .container { max-width: 1400px; margin: auto; }
        .dashboard-header { display: flex; justify-content: space-between; align-items: center; background: linear-gradient(135deg, #16191f, #34495e); color: white; padding: 15px 20px; border-radius: 8px; margin-bottom: 20px; box-shadow: 0 4px 15px rgba(0,0,0,0.5); border: 1px solid #34495e; }
        .company-logo { height: 70px; width: auto; margin-right: 18px; border-radius: 4px; }
        .stats-bar { background: #2c3e50; color: white; padding: 15px 20px; border-radius: 8px; margin-bottom: 20px; display: flex; justify-content: space-between; font-size: 1.2em; font-weight: bold; border: 1px solid rgba(52, 152, 219, 0.2); }
        #total-spark-stat { color: #f1c40f; }
        .panels { display: flex; gap: 20px; flex-wrap: wrap; align-items: flex-start; }
        .panel { background: rgba(44, 62, 80, 0.9); backdrop-filter: blur(5px); padding: 20px; border-radius: 8px; box-shadow: 0 10px 30px rgba(0,0,0,0.4); flex: 1; min-width: 350px; border: 1px solid rgba(255, 255, 255, 0.05); }
        .admin-only { display: none; border: 2px solid #e67e22; }
        label { font-weight: bold; font-size: 0.9em; color: #bdc3c7; display: block; margin-top: 10px; }
        select, input { width: 100%; padding: 12px; margin-top: 5px; margin-bottom: 10px; border-radius: 5px; border: 1px solid #34495e; background-color: #1a1c23; color: #ecf0f1; font-size: 16px; box-sizing: border-box; }
        button { background: linear-gradient(135deg, #3498db, #16191f); color: white; border: 1px solid #3498db; cursor: pointer; font-weight: bold; text-transform: uppercase; letter-spacing: 1px; padding: 12px; border-radius: 5px; transition: 0.2s; }
        button:hover { box-shadow: 0 0 15px rgba(52, 152, 219, 0.7); }
        .project-card { border-left: 5px solid #bdc3c7; background: #2c3e50; margin-bottom: 15px; padding: 15px; border-radius: 4px; box-shadow: 0 4px 6px rgba(0,0,0,0.3); }
        .timer-display { font-family: 'Courier New', Courier, monospace; font-size: 1.3em; color: #2ecc71; font-weight: bold; margin: 10px 0; background: #16191f; padding: 5px; border-radius: 4px; text-align: center; }
        .green-gen { border-left-color: #2ecc71; }
        .red-gen { border-left-color: #e74c3c; }
        .yellow-gen { border-left-color: #f1c40f; }
        .progress-bar { background: #34495e; border-radius: 4px; height: 10px; width: 100%; margin: 10px 0; overflow: hidden; }
        .progress-fill { background: linear-gradient(90deg, #3498db, #ecf0f1); height: 100%; transition: width 0.3s ease; }
        .ledger-table { width: 100%; border-collapse: collapse; font-size: 0.85em; margin-top: 10px; }
        .ledger-table th, .ledger-table td { border: 1px solid #34495e; padding: 8px; text-align: left; }
        .ledger-table th { background: #16191f; color: #3498db; }
        .status-dot { height: 10px; width: 10px; background-color: #2ecc71; border-radius: 50%; display: inline-block; margin-right: 5px; animation: pulse 2s infinite; }
        @keyframes pulse { 0% { opacity: 1; } 50% { opacity: 0.4; } 100% { opacity: 1; } }
    </style>
</head>
<body>

<div class="container">
    <div style="text-align: right; margin-bottom: 10px; font-size: 0.85em; color: #bdc3c7;">
        <span id="user-status">Public Access</span> | <a href="#" onclick="window.toggleAuth()" style="color:#3498db; text-decoration:none; font-weight:bold;">Admin Login</a>
    </div>

    <div class="dashboard-header">
        <div style="display:flex; align-items:center;">
            <img src="GDSlogo2.png" alt="Raytown Construction Logo" class="company-logo">
            <h2 style="margin: 0; text-transform: uppercase; font-weight: 800; letter-spacing: 2px;">Dispatch Command</h2>
        </div>
        <div id="live-clock" style="font-weight:bold;"></div>
    </div>

    <div class="stats-bar">
        <span>Active Spark on Field:</span>
        <span id="total-spark-stat">0.00 Spark</span>
    </div>

    <div class="panels">
        <div class="panel">
            <h3>Customer: Submit Project</h3>
            <label>Build Category:</label>
            <select id="category-select" onchange="window.populateProperties()">
                <option value="">-- Choose Category --</option>
                <option value="green">Green (Houses)</option>
                <option value="red">Red (Townhouses)</option>
                <option value="yellow">Yellow (Towers)</option>
                <option value="service_structures">Service Structures</option>
                <option value="custom">Custom Entry</option>
            </select>
            <div id="property-group"><label>Property Model:</label><select id="property-select" disabled></select></div>
            <div id="custom-group" style="display:none;"><label>Spark Hours:</label><input type="number" id="custom-hours"><label>Name:</label><input type="text" id="custom-name"></div>
            <label>Address:</label><input type="text" id="project-address">
            <button onclick="window.submitForApproval()">Submit for Approval</button>
        </div>

        <div class="panel admin-only">
            <h3>Admin: Control Center</h3>
            <h4 style="color:#e67e22;">Pending Queue</h4>
            <div id="pending-board"></div>
            <h4 style="color:#3498db;">Active Stakes (Pay when done)</h4>
            <div id="active-stakes-admin"></div>
        </div>

        <div class="panel">
            <h3>Live Dispatch Board</h3>
            <div id="dispatch-board"></div>
        </div>
    </div>

    <div class="panels" style="margin-top:20px;">
        <div class="panel">
            <h3 style="color:#2ecc71;">Weekly Payout Ledger</h3>
            <table class="ledger-table"><thead id="weekly-head"></thead><tbody id="weekly-body"></tbody></table>
        </div>
        <div class="panel">
            <h3 style="color:#9b59b6;">Monthly Payout Ledger</h3>
            <table class="ledger-table"><thead id="monthly-head"></thead><tbody id="monthly-body"></tbody></table>
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

    const properties = {
        green: [{ name: "Luxury Modern House", value: 75000 }, { name: "Cosy Cabin", value: 72000 }, { name: "Ranch House", value: 30000 }],
        red: [{ name: "Town House", value: 60000 }, { name: "Madrid Townhouse", value: 51000 }],
        yellow: [{ name: "Cypher Cyborgs Tower", value: 1500000 }, { name: "Apartment Building", value: 90000 }],
        service_structures: [{ name: "University", value: 147150 }, { name: "Hospital", value: 80100 }, { name: "Fire HQ", value: 62100 }]
    };

    const sparkletRules = { green: { min: 5000, max: 30000 }, red: { min: 10000, max: 30000 }, yellow: { min: 15000, max: 30000 } };

    window.toggleAuth = () => {
        if (auth.currentUser) signOut(auth);
        else {
            const e = prompt("Admin Email:"), p = prompt("Password:");
            if(e && p) signInWithEmailAndPassword(auth, e, p).catch(() => alert("Denied"));
        }
    };

    onAuthStateChanged(auth, u => {
        document.querySelectorAll('.admin-only').forEach(el => el.style.display = u ? 'block' : 'none');
        document.getElementById('user-status').innerText = u ? "Admin Active" : "Public Access";
    });

    // CUSTOMER SUBMIT
    window.submitForApproval = () => {
        const cat = document.getElementById('category-select').value;
        const addr = document.getElementById('project-address').value.trim();
        if(!cat || !addr) return alert("Missing info");
        let name, value, color;
        if(cat === 'custom') {
            name = document.getElementById('custom-name').value;
            value = parseInt(document.getElementById('custom-hours').value);
            color = value < 40000 ? 'green' : value < 70000 ? 'red' : 'yellow';
        } else {
            const idx = document.getElementById('property-select').value;
            const p = properties[cat][idx];
            name = p.name; value = p.value;
            color = cat === 'service_structures' ? (value < 40000 ? 'green' : value < 70000 ? 'red' : 'yellow') : cat;
        }
        push(ref(db, 'pendingProjects'), { name, address: addr, value, color, maxGens: Math.ceil(value/10000) });
        alert("Sent for approval!");
    };

    // ADMIN APPROVAL
    onValue(ref(db, 'pendingProjects'), snap => {
        const b = document.getElementById('pending-board');
        b.innerHTML = "";
        if(snap.val()) Object.entries(snap.val()).forEach(([id, d]) => {
            b.innerHTML += `<div class="project-card"><strong>${d.name}</strong><br>${d.address}<br><button onclick="window.approve('${id}')" style="background:#2ecc71">Approve</button></div>`;
        });
    });

    window.approve = id => {
        onValue(ref(db, `pendingProjects/${id}`), s => {
            if(s.val()) { push(ref(db, 'activeProjects'), { ...s.val(), currentGens: 0 }); remove(ref(db, `pendingProjects/${id}`)); }
        }, { onlyOnce: true });
    };

    // DISPATCH & TIMERS
    onValue(ref(db, 'activeProjects'), snap => {
        const b = document.getElementById('dispatch-board');
        b.innerHTML = snap.val() ? "" : "<i>No projects live.</i>";
        window.activeProjectsArr = snap.val() ? Object.entries(snap.val()).map(([id, d]) => ({id, ...d})) : [];
        if(snap.val()) Object.entries(snap.val()).forEach(([id, p]) => {
            b.innerHTML += `<div class="project-card ${p.color}-gen"><strong>${p.name}</strong><br>${p.address}<br>Slots: ${p.maxGens - p.currentGens} Open<button onclick="window.accept('${id}')" style="margin-top:10px">Claim Slot</button></div>`;
        });
    });

    window.accept = id => {
        const p = window.activeProjectsArr.find(x => x.id === id);
        const user = prompt("Username:");
        const gens = parseInt(prompt(`Gens? (Max ${p.maxGens - p.currentGens})`));
        const rules = sparkletRules[p.color];
        const sparklet = parseInt(prompt(`Sparklet per gen? (${rules.min}-${rules.max})`));
        if(!user || !gens || !sparklet || sparklet < rules.min || sparklet > rules.max) return alert("Invalid entry");
        
        const power = (gens * sparklet) / 1000;
        const hours = p.value / power;
        push(ref(db, 'ledgerLog'), { 
            name: p.name, address: p.address, subcontractor: user, activeSpark: power, 
            status: 'active', timestamp: Date.now(), hoursNeeded: hours, color: p.color,
            endTime: Date.now() + (hours * 3600000)
        });
        const nc = p.currentGens + gens;
        if(nc >= p.maxGens) remove(ref(db, `activeProjects/${id}`));
        else update(ref(db, `activeProjects/${id}`), { currentGens: nc });
    };

    // LEDGER & FINALIZE
    onValue(ref(db, 'ledgerLog'), snap => {
        window.ledgerArr = snap.val() ? Object.entries(snap.val()).map(([id, d]) => ({id, ...d})) : [];
        renderAll();
    });

    function renderAll() {
        const adminList = document.getElementById('active-stakes-admin');
        const weeklyBody = document.getElementById('weekly-body');
        const monthlyBody = document.getElementById('monthly-body');
        const weeklyHead = document.getElementById('weekly-head');
        const monthlyHead = document.getElementById('monthly-head');
        
        const h = "<tr><th>Sub</th><th>Property</th><th>Power</th><th>Payout</th><th>Date</th></tr>";
        weeklyHead.innerHTML = h; monthlyHead.innerHTML = h;
        adminList.innerHTML = ""; weeklyBody.innerHTML = ""; monthlyBody.innerHTML = "";

        let totalPower = 0;
        const now = Date.now();

        window.ledgerArr.forEach(s => {
            if(s.status === 'active') {
                totalPower += s.activeSpark;
                const remaining = s.endTime - now;
                const timeStr = remaining > 0 ? formatTime(remaining) : "COMPLETE - ALERT!";
                if(remaining <= 0) alertOnce(s.name);

                adminList.innerHTML += `<div class="project-card"><strong>${s.name} (${s.subcontractor})</strong><div class="timer-display">${timeStr}</div><button onclick="window.finalize('${s.id}')" style="background:#e67e22">Finalize Payout</button></div>`;
            } else {
                const date = new Date(s.timestamp).toLocaleDateString();
                const row = `<tr><td>${s.subcontractor}</td><td>${s.name}</td><td>${s.activeSpark}</td><td>${s.payout}</td><td>${date}</td></tr>`;
                const weekAgo = now - (7 * 24 * 3600 * 1000);
                const monthAgo = now - (30 * 24 * 3600 * 1000);
                if(s.timestamp > weekAgo) weeklyBody.innerHTML += row;
                if(s.timestamp > monthAgo) monthlyBody.innerHTML += row;
            }
        });
        document.getElementById('total-spark-stat').innerText = totalPower.toFixed(2) + " Spark";
    }

    window.finalize = id => {
        const s = window.ledgerArr.find(x => x.id === id);
        const h = Math.min(s.hoursNeeded, (Date.now() - s.timestamp) / 3600000);
        const p = Math.floor((s.activeSpark * h) * 0.95);
        update(ref(db, `ledgerLog/${id}`), { status: 'finished', payout: p, finishDate: Date.now() });
    };

    function formatTime(ms) {
        const h = Math.floor(ms / 3600000);
        const m = Math.floor((ms % 3600000) / 60000);
        const s = Math.floor((ms % 60000) / 1000);
        return `${h}h ${m}m ${s}s`;
    }

    let alerted = {};
    function alertOnce(name) { if(!alerted[name]) { alert(`PROJECT COMPLETE: ${name}`); alerted[name] = true; } }

    window.populateProperties = () => {
        const cat = document.getElementById('category-select').value;
        const sel = document.getElementById('property-select');
        const cG = document.getElementById('custom-group');
        const pG = document.getElementById('property-group');
        cG.style.display = cat === 'custom' ? 'block' : 'none';
        pG.style.display = cat === 'custom' ? 'none' : 'block';
        sel.innerHTML = "";
        if(cat && properties[cat]) {
            sel.disabled = false;
            properties[cat].forEach((p, i) => sel.innerHTML += `<option value="${i}">${p.name} (${p.value})</option>`);
        } else sel.disabled = true;
    };

    setInterval(() => { 
        document.getElementById('live-clock').innerText = new Date().toLocaleString(); 
        renderAll(); 
    }, 1000);
</script>
</body>
</html>
