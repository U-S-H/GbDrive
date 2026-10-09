<html lang="ur" dir="ltr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  
  <!-- SEO & GOOGLE META TAGS -->
  <title>GB Drive - Gilgit-Baltistan Official InDrive Style Platform</title>
  <meta name="description" content="GB Drive - Gilgit-Baltistan's official InDrive-style platform. Book city rides, intercity travel, courier and freight delivery, and 4x4 jeeps with live bidding.">
  <meta name="keywords" content="GB Drive, InDrive Pakistan, Gilgit Intercity, Skardu Freight Delivery, Hunza 4x4 Jeep">
  <meta name="author" content="GB Drive Network">
  <meta name="robots" content="index, follow">

  <!-- Leaflet CSS for Maps -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  
  <style>
    :root {
      --primary: #2563eb;
      --primary-dark: #1d4ed8;
      --accent: #10b981;
      --danger: #ef4444;
      --warning: #f59e0b;
      --bg: #f3f4f6;
      --card-bg: #ffffff;
      --text: #1f2937;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu, sans-serif;
    }

    body {
      background-color: var(--bg);
      color: var(--text);
      display: flex;
      flex-direction: column;
      align-items: center;
      min-height: 100vh;
      padding: 10px;
    }

    .app-container {
      width: 100%;
      max-width: 500px;
      background: var(--card-bg);
      border-radius: 16px;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
      overflow: hidden;
    }

    header {
      background: var(--primary);
      color: white;
      padding: 15px;
      text-align: center;
      position: relative;
    }

    header h1 { font-size: 1.5rem; font-weight: 700; }
    header p { font-size: 0.8rem; opacity: 0.9; }

    .top-controls {
      position: absolute;
      right: 10px;
      top: 10px;
      display: flex;
      gap: 6px;
    }

    .top-btn {
      background: rgba(255, 255, 255, 0.25);
      border: none;
      color: white;
      padding: 4px 8px;
      border-radius: 6px;
      font-size: 0.75rem;
      cursor: pointer;
      font-weight: 600;
    }

    #map { height: 240px; width: 100%; z-index: 1; }

    .content { padding: 15px; }

    .gps-btn {
      background: #3b82f6;
      color: white;
      border: none;
      padding: 8px;
      border-radius: 8px;
      width: 100%;
      font-size: 0.85rem;
      font-weight: 600;
      cursor: pointer;
      margin-bottom: 10px;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 6px;
    }

    .fare-calculator-badge {
      background: #ecfdf5;
      border: 1px solid #10b981;
      color: #065f46;
      padding: 10px;
      border-radius: 10px;
      margin-bottom: 12px;
      font-size: 0.85rem;
    }

    .form-group {
      margin-bottom: 12px;
      position: relative;
    }

    .form-group label {
      display: block;
      font-size: 0.85rem;
      font-weight: 600;
      margin-bottom: 4px;
      color: #4b5563;
    }

    .form-group input, .form-group select {
      width: 100%;
      padding: 10px 12px;
      border: 1px solid #d1d5db;
      border-radius: 8px;
      font-size: 0.9rem;
      outline: none;
    }

    .suggestions-box {
      position: absolute;
      top: 100%;
      left: 0;
      right: 0;
      background: white;
      border: 1px solid #d1d5db;
      border-radius: 8px;
      max-height: 160px;
      overflow-y: auto;
      z-index: 1000;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }

    .suggestion-item {
      padding: 8px 12px;
      font-size: 0.85rem;
      cursor: pointer;
      border-bottom: 1px solid #f3f4f6;
    }

    .suggestion-item:hover { background: #eff6ff; }

    .btn {
      width: 100%;
      padding: 12px;
      border: none;
      border-radius: 8px;
      background: var(--primary);
      color: white;
      font-size: 1rem;
      font-weight: 600;
      cursor: pointer;
      margin-top: 5px;
    }

    .btn-google { background: #ea4335; margin-bottom: 10px; }
    .btn-accent { background: var(--accent); }
    .btn-danger { background: var(--danger); }
    .btn-warning { background: var(--warning); color: black; }

    .ride-card {
      border: 1px solid #e5e7eb;
      border-radius: 12px;
      padding: 12px;
      margin-bottom: 12px;
      background: #fafafa;
    }

    .ride-card h4 { color: var(--primary); margin-bottom: 6px; font-size: 0.95rem; }
    .ride-info { font-size: 0.85rem; margin-bottom: 4px; }

    .bid-input-group { display: flex; gap: 6px; margin-top: 8px; }
    .bid-input-group input { flex: 1; padding: 8px; border: 1px solid #d1d5db; border-radius: 6px; }

    .user-badge {
      background: #e0e7ff;
      color: #3730a3;
      padding: 8px 12px;
      border-radius: 8px;
      font-size: 0.85rem;
      margin-bottom: 12px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .driver-dashboard-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: #f3f4f6;
      padding: 10px;
      border-radius: 8px;
      margin-bottom: 12px;
    }

    .nearby-count {
      background: #d1fae5;
      color: #065f46;
      padding: 6px 10px;
      border-radius: 6px;
      font-size: 0.8rem;
      font-weight: 600;
      margin-bottom: 10px;
      text-align: center;
    }

    .chat-container {
      border: 1px solid #d1d5db;
      border-radius: 10px;
      background: #ffffff;
      padding: 10px;
      margin-top: 10px;
    }

    .chat-messages {
      height: 120px;
      overflow-y: auto;
      border-bottom: 1px solid #e5e7eb;
      padding-bottom: 8px;
      margin-bottom: 8px;
      font-size: 0.85rem;
    }

    .chat-msg {
      margin-bottom: 6px;
      padding: 6px 10px;
      border-radius: 6px;
      max-width: 80%;
    }

    .msg-mine { background: #dbeafe; margin-left: auto; text-align: right; }
    .msg-other { background: #f3f4f6; }

    .hidden { display: none !important; }
  </style>
</head>
<body>

  <div class="app-container">
    <header>
      <h1>GB Drive</h1>
      <p id="app-tagline">InDrive Official Style Platform (GB)</p>
      <div class="top-controls">
        <button class="top-btn" onclick="toggleLanguage()" id="lang-btn">English</button>
        <button id="logout-btn" class="top-btn hidden">Logout</button>
      </div>
    </header>

    <!-- AUTH SECTION -->
    <div id="auth-section" class="content">
      <h2 id="auth-title" style="text-align: center; margin-bottom: 15px;">Login to GB Drive</h2>
      <button id="google-login-btn" class="btn btn-google">Continue with Google</button>
      <div style="text-align: center; margin: 10px 0; color: #9ca3af; font-size: 0.8rem;">OR EMAIL LOGIN</div>

      <form id="auth-form">
        <div class="form-group">
          <label id="lbl-email">Email Address</label>
          <input type="email" id="auth-email" placeholder="name@example.com" required>
        </div>
        <div class="form-group">
          <label id="lbl-password">Password</label>
          <input type="password" id="auth-password" placeholder="••••••••" required>
        </div>
        <button type="submit" id="auth-submit-btn" class="btn">Login</button>
      </form>

      <div class="auth-toggle" id="auth-toggle-btn" onclick="toggleAuthMode()">
        Don't have an account? Sign Up
      </div>
    </div>

    <!-- PROFILE SETUP SECTION -->
    <div id="profile-setup-section" class="content hidden">
      <h2 style="text-align: center; margin-bottom: 10px;">InDrive Verification & Profile</h2>
      <form id="profile-setup-form">
        <div class="form-group">
          <label>Account Type</label>
          <select id="setup-role" onchange="toggleDriverSetupFields()">
            <option value="passenger">Passenger / Client</option>
            <option value="driver">Driver / Courier / Partner</option>
          </select>
        </div>
        <div class="form-group">
          <label>Full Name</label>
          <input type="text" id="setup-name" placeholder="Full Name as on CNIC" required>
        </div>
        <div class="form-group">
          <label>CNIC Number</label>
          <input type="text" id="setup-cnic" placeholder="71101-XXXXXXX-X" required>
        </div>
        <div class="form-group">
          <label>WhatsApp Phone Number</label>
          <input type="text" id="setup-phone" placeholder="03001234567" required>
        </div>
        <div class="form-group">
          <label>Emergency Contact</label>
          <input type="text" id="setup-emergency" placeholder="Emergency Relative Number" required>
        </div>

        <div id="driver-setup-fields" class="hidden">
          <div class="form-group">
            <label>Service Category</label>
            <select id="setup-vehicle-type">
              <option value="City Rides">City Rides (Local)</option>
              <option value="Intercity">Intercity Travel (Long Route)</option>
              <option value="Courier">Courier Delivery (Packages up to 20kg)</option>
              <option value="Freight">Freight & Cargo Delivery (Trucks)</option>
              <option value="Jeep 4x4">4x4 Jeep (Mountain Expedition)</option>
            </select>
          </div>
          <div class="form-group">
            <label>Vehicle Number Plate</label>
            <input type="text" id="setup-vehicle-num" placeholder="e.g. GIL-1234">
          </div>
        </div>

        <button type="submit" class="btn btn-accent">Save Profile & Continue</button>
      </form>
    </div>

    <!-- MAIN APP DASHBOARD -->
    <div id="app-section" class="hidden">
      <div class="content" style="padding-bottom: 0;">
        <div class="user-badge">
          <span id="user-display-name">Welcome User</span>
          <strong id="user-display-role" style="text-transform: uppercase;">PASSENGER</strong>
        </div>

        <button class="btn btn-danger" onclick="triggerSOSAlert()" style="font-size: 0.85rem; padding: 8px; margin-bottom: 10px;">
          🚨 EMERGENCY SOS ALERT
        </button>

        <button class="gps-btn" onclick="getCurrentGPSLocation()">
          🎯 Turn On Live GPS Location
        </button>

        <div id="nearby-info" class="nearby-count hidden">
          🚗 <span id="driver-count">0</span> Verified Partners Online Nearby
        </div>
      </div>

      <!-- LEAFLET MAP CONTAINER -->
      <div id="map"></div>

      <div class="content">
        <!-- PASSENGER DASHBOARD (INDRIVE OFFICIAL SERVICES) -->
        <div id="passenger-section" class="hidden">
          <div id="fare-badge" class="fare-calculator-badge hidden">
            <div>📏 Route Distance: <strong id="calc-distance">0 km</strong></div>
            <div>💰 Suggested InDrive Fare: <strong id="calc-fare" style="color: #059669; font-size: 1.1rem;">0 PKR</strong></div>
          </div>
          
          <form id="ride-form" autocomplete="off">
            <div class="form-group">
              <label>Select InDrive Service</label>
              <select id="ride-service-type" onchange="calculateInDriveFare()">
                <option value="City Rides">City Rides (Everyday Local Commute)</option>
                <option value="Intercity">Intercity (City-to-City Travel)</option>
                <option value="Courier">Courier (Door-to-door Package Delivery up to 20kg)</option>
                <option value="Freight">Freight (Truck / Cargo Transport over 20kg)</option>
                <option value="Jeep 4x4">4x4 Jeep (Deosai / Skardu Expedition)</option>
              </select>
            </div>

            <div class="form-group">
              <label>Pickup Location / Address</label>
              <input type="text" id="pickup" placeholder="GPS or type pickup location..." oninput="searchLocation('pickup')" required>
              <div id="pickup-suggestions" class="suggestions-box hidden"></div>
            </div>

            <div class="form-group">
              <label>Dropoff Destination / Address</label>
              <input type="text" id="dropoff" placeholder="Type destination address..." oninput="searchLocation('dropoff')" required>
              <div id="dropoff-suggestions" class="suggestions-box hidden"></div>
            </div>

            <div class="form-group">
              <label>Name Your Fare (Propose Your Price - PKR)</label>
              <input type="number" id="fare" placeholder="Enter what you want to pay" required>
            </div>
            <button type="submit" class="btn">Offer Fare & Request Service</button>
          </form>

          <div id="passenger-ride-status" style="margin-top: 15px;"></div>
        </div>

        <!-- DRIVER / PARTNER DASHBOARD -->
        <div id="driver-section" class="hidden">
          <div class="driver-dashboard-header">
            <div>Status: <strong id="driver-online-text" style="color: #10b981;">ONLINE</strong></div>
            <button class="btn btn-warning" id="driver-toggle-btn" onclick="toggleDriverOnlineStatus()" style="width: auto; padding: 6px 12px; margin:0; font-size: 0.8rem;">Go Offline</button>
          </div>

          <div class="fare-calculator-badge" style="background: #eff6ff; border-color: #2563eb; color: #1e40af;">
            💰 Partner Earnings Wallet: <strong id="driver-daily-earnings" style="font-size: 1.1rem;">0 PKR</strong>
          </div>

          <h3>Incoming Client Requests & Bidding</h3>
          <div id="rides-list" style="margin-top: 10px;"></div>
        </div>
      </div>
    </div>
  </div>

  <!-- Leaflet JS -->
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

  <!-- FIREBASE SDKs -->
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
    import { getAuth, signInWithEmailAndPassword, createUserWithEmailAndPassword, GoogleAuthProvider, signInWithPopup, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
    import { getDatabase, ref, push, set, onValue, update, get } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

    const firebaseConfig = {
      apiKey: "AIzaSyBe5Q5jXpx3UvrHC9WOky9UWeDnP9SPfZI",
      authDomain: "verbose-6c008.firebaseapp.com",
      databaseURL: "https://verbose-6c008-default-rtdb.firebaseio.com",
      projectId: "verbose-6c008",
      storageBucket: "verbose-6c008.firebasestorage.app",
      messagingSenderId: "867100945312",
      appId: "1:867100945312:web:315dfb48fb34496cee12c5"
    };

    const app = initializeApp(firebaseConfig);
    const auth = getAuth(app);
    const db = getDatabase(app);
    const googleProvider = new GoogleAuthProvider();

    let currentUser = null;
    let currentUserProfile = null;
    let isDriverOnline = true;
    let currentLanguage = 'ur';
    let driverMarkers = {};

    window.toggleLanguage = function() {
      currentLanguage = currentLanguage === 'ur' ? 'en' : 'ur';
      document.getElementById('lang-btn').innerText = currentLanguage === 'ur' ? 'English' : 'اردو';
      if (currentLanguage === 'en') {
        document.getElementById('app-tagline').innerText = 'InDrive Official Style Platform (GB)';
      } else {
        document.getElementById('app-tagline').innerText = 'ان ڈرائیو طرز کا آفیشل پلیٹ فارم (گلگت بلتستان)';
      }
    };

    let map = null;
    let userMarker = null;
    let userCoords = null;
    let routeDistanceKm = 15.0;
    let searchDebounce = null;

    function initLeafletMap() {
      if (map) return;
      map = L.map('map').setView([35.8039, 74.8726], 8);
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '© OpenStreetMap contributors'
      }).addTo(map);
    }

    window.getCurrentGPSLocation = function() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition((pos) => {
          const lat = pos.coords.latitude;
          const lng = pos.coords.longitude;
          userCoords = [lat, lng];

          if (userMarker) map.removeLayer(userMarker);
          userMarker = L.marker([lat, lng]).addTo(map).bindPopup("Your Location").openPopup();

          map.setView([lat, lng], 13);
          document.getElementById('pickup').value = `Current GPS Location (${lat.toFixed(4)}, ${lng.toFixed(4)})`;

          if (currentUserProfile && currentUserProfile.role === 'driver') {
            updateDriverGPSLocation(lat, lng);
          }
        });
      }
    };

    function updateDriverGPSLocation(lat, lng) {
      if (!isDriverOnline) return;
      set(ref(db, `active_drivers/${currentUser.uid}`), {
        driverId: currentUser.uid,
        name: currentUserProfile.name,
        lat,
        lng,
        serviceType: currentUserProfile.vehicleType || 'City Rides',
        updatedAt: Date.now()
      });
    }

    function listenToNearbyDrivers() {
      const driversRef = ref(db, 'active_drivers');
      onValue(driversRef, (snapshot) => {
        const data = snapshot.val();
        let count = 0;
        if (data) {
          Object.values(data).forEach(d => {
            count++;
            if (!driverMarkers[d.driverId]) {
              driverMarkers[d.driverId] = L.marker([d.lat, d.lng])
                .addTo(map)
                .bindPopup(`🚗 ${d.name} (${d.serviceType})`);
            } else {
              driverMarkers[d.driverId].setLatLng([d.lat, d.lng]);
            }
          });
        }
        document.getElementById('driver-count').innerText = count;
        document.getElementById('nearby-info').classList.remove('hidden');
      });
    }

    window.searchLocation = function(type) {
      const query = document.getElementById(type).value;
      const suggestionsBox = document.getElementById(`${type}-suggestions`);
      if (query.length < 3) { suggestionsBox.classList.add('hidden'); return; }

      clearTimeout(searchDebounce);
      searchDebounce = setTimeout(async () => {
        try {
          const res = await fetch(`https://nominatim.openstreetmap.org/search?format=json&q=${encodeURIComponent(query)}&countrycodes=pk&limit=5`);
          const data = await res.json();
          suggestionsBox.innerHTML = '';
          if (data.length === 0) { suggestionsBox.classList.add('hidden'); return; }

          data.forEach(item => {
            const div = document.createElement('div');
            div.className = 'suggestion-item';
            div.innerText = item.display_name;
            div.onclick = () => {
              document.getElementById(type).value = item.display_name.split(',')[0];
              suggestionsBox.classList.add('hidden');
              calculateInDriveFare();
            };
            suggestionsBox.appendChild(div);
          });
          suggestionsBox.classList.remove('hidden');
        } catch (e) { console.error(e); }
      }, 300);
    };

    window.calculateInDriveFare = function() {
      const service = document.getElementById('ride-service-type').value;
      let ratePerKm = 50;
      if (service === 'Intercity') { ratePerKm = 70; routeDistanceKm = 100.0; }
      else if (service === 'Courier') { ratePerKm = 60; routeDistanceKm = 10.0; }
      else if (service === 'Freight') { ratePerKm = 110; routeDistanceKm = 50.0; }
      else if (service === 'Jeep 4x4') { ratePerKm = 140; routeDistanceKm = 40.0; }
      else { routeDistanceKm = 15.0; }

      let calculatedFare = Math.round(250 + (routeDistanceKm * ratePerKm));
      document.getElementById('calc-distance').innerText = `${routeDistanceKm.toFixed(1)} km`;
      document.getElementById('calc-fare').innerText = `${calculatedFare} PKR`;
      document.getElementById('fare').value = calculatedFare;
      document.getElementById('fare-badge').classList.remove('hidden');
    };

    window.triggerSOSAlert = function() {
      if (!currentUserProfile) return;
      const emergencyNo = currentUserProfile.emergencyPhone || '03000000000';
      const message = `EMERGENCY SOS ALERT! I need immediate help during GB Drive service.`;
      window.open(`https://wa.me/${emergencyNo}?text=${encodeURIComponent(message)}`, '_blank');
    };

    window.toggleDriverOnlineStatus = function() {
      isDriverOnline = !isDriverOnline;
      const statusText = document.getElementById('driver-online-text');
      const toggleBtn = document.getElementById('driver-toggle-btn');
      if (isDriverOnline) {
        statusText.innerText = 'ONLINE'; statusText.style.color = '#10b981';
        toggleBtn.innerText = 'Go Offline'; toggleBtn.className = 'btn btn-warning';
      } else {
        statusText.innerText = 'OFFLINE'; statusText.style.color = '#ef4444';
        toggleBtn.innerText = 'Go Online'; toggleBtn.className = 'btn btn-accent';
      }
    };

    onAuthStateChanged(auth, async (user) => {
      if (user) {
        currentUser = user;
        document.getElementById('logout-btn').classList.remove('hidden');
        document.getElementById('auth-section').classList.add('hidden');
        const snapshot = await get(ref(db, `users/${user.uid}`));
        const profile = snapshot.val();
        if (profile && profile.isProfileComplete) {
          currentUserProfile = profile;
          loadMainAppDashboard();
        } else {
          document.getElementById('profile-setup-section').classList.remove('hidden');
        }
      } else {
        document.getElementById('auth-section').classList.add('hidden');
      }
    });

    const authForm = document.getElementById('auth-form');
    authForm.addEventListener('submit', async (e) => {
      e.preventDefault();
      await signInWithEmailAndPassword(auth, document.getElementById('auth-email').value, document.getElementById('auth-password').value);
    });

    document.getElementById('google-login-btn').addEventListener('click', async () => {
      await signInWithPopup(auth, googleProvider);
    });

    document.getElementById('logout-btn').addEventListener('click', () => signOut(auth));

    const profileSetupForm = document.getElementById('profile-setup-form');
    profileSetupForm.addEventListener('submit', async (e) => {
      e.preventDefault();
      const profileData = {
        uid: currentUser.uid,
        email: currentUser.email,
        name: document.getElementById('setup-name').value,
        cnic: document.getElementById('setup-cnic').value,
        phone: document.getElementById('setup-phone').value,
        emergencyPhone: document.getElementById('setup-emergency').value,
        role: document.getElementById('setup-role').value,
        vehicleType: document.getElementById('setup-vehicle-type') ? document.getElementById('setup-vehicle-type').value : 'City Rides',
        isProfileComplete: true
      };
      await set(ref(db, `users/${currentUser.uid}`), profileData);
      currentUserProfile = profileData;
      document.getElementById('profile-setup-section').classList.add('hidden');
      loadMainAppDashboard();
    });

    function loadMainAppDashboard() {
      document.getElementById('app-section').classList.remove('hidden');
      document.getElementById('user-display-name').innerText = currentUserProfile.name;
      document.getElementById('user-display-role').innerText = currentUserProfile.role;

      if (currentUserProfile.role === 'driver') {
        document.getElementById('driver-section').classList.remove('hidden');
        document.getElementById('passenger-section').classList.add('hidden');
        loadDriverWallet();
      } else {
        document.getElementById('passenger-section').classList.remove('hidden');
        document.getElementById('driver-section').classList.add('hidden');
        listenToNearbyDrivers();
      }
      initLeafletMap();
      getCurrentGPSLocation();
    }

    const rideForm = document.getElementById('ride-form');
    rideForm.addEventListener('submit', (e) => {
      e.preventDefault();
      const ridesRef = ref(db, 'rides/');
      const newRideRef = push(ridesRef);
      const otpCode = Math.floor(1000 + Math.random() * 9000);

      set(newRideRef, {
        clientId: currentUser.uid,
        clientName: currentUserProfile.name,
        clientPhone: currentUserProfile.phone,
        pickup: document.getElementById('pickup').value,
        dropoff: document.getElementById('dropoff').value,
        fare: Number(document.getElementById('fare').value),
        serviceType: document.getElementById('ride-service-type').value,
        otpCode,
        status: 'pending',
        createdAt: Date.now()
      }).then(() => {
        alert('Request posted successfully! Waiting for partner bids...');
        listenToMyRequest(newRideRef.key);
      });
    });

    function listenToMyRequest(rideId) {
      const rideRef = ref(db, `rides/${rideId}`);
      const statusDiv = document.getElementById('passenger-ride-status');
      onValue(rideRef, (snapshot) => {
        const ride = snapshot.val();
        if (!ride) return;
        if (ride.status === 'pending' && !ride.bids) {
          statusDiv.innerHTML = `<div class="ride-card">⏳ Looking for partners & couriers...</div>`;
        } else if (ride.bids && ride.status === 'pending') {
          let html = `<h4>Partner & Driver Bids Received:</h4>`;
          Object.keys(ride.bids).forEach(bidId => {
            const bid = ride.bids[bidId];
            html += `
              <div class="ride-card">
                <div>Partner: <strong>⭐ 4.9 (Verified)</strong></div>
                <div>Name & Service: <strong>${bid.driverName} (${bid.vehicleType})</strong></div>
                <div>Proposed Counter Bid: <strong style="color: green;">PKR ${bid.amount}</strong></div>
                <div style="display:flex; gap:6px; margin-top:8px;">
                  <button class="btn btn-accent" onclick="window.acceptBid('${rideId}', '${bid.driverName}', '${bid.driverPhone}', ${bid.amount})">Accept Offer</button>
                </div>
              </div>
            `;
          });
          statusDiv.innerHTML = html;
        } else if (ride.status === 'accepted') {
          statusDiv.innerHTML = `
            <div class="ride-card" style="background: #d1fae5; border-color: #10b981;">
              ✅ <strong>Offer Accepted!</strong><br>
              Partner: ${ride.acceptedDriverName} (${ride.acceptedFare} PKR)<br>
              🔐 <strong>Secure OTP Code: <span style="font-size: 1.2rem; color: #1e40af;">${ride.otpCode}</span></strong>
            </div>
            <div class="chat-container">
              <h5 style="margin-bottom:6px;">💬 In-App Live Chat</h5>
              <div class="chat-messages" id="chat-box-${rideId}"></div>
              <div style="display:flex; gap:6px;">
                <input type="text" id="chat-input-${rideId}" placeholder="Type message..." style="flex:1; padding:6px; border-radius:6px; border:1px solid #ccc;">
                <button class="btn btn-accent" onclick="window.sendChatMessage('${rideId}')" style="width:auto; padding:6px 12px; margin:0;">Send</button>
              </div>
            </div>
          `;
          listenToChatMessages(rideId);
        }
      });
    }

    const ridesList = document.getElementById('rides-list');
    const ridesRef = ref(db, 'rides/');

    onValue(ridesRef, (snapshot) => {
      if (!currentUserProfile || currentUserProfile.role !== 'driver' || !isDriverOnline) return;
      const data = snapshot.val();
      ridesList.innerHTML = '';
      if (!data) {
        ridesList.innerHTML = '<p style="color: #6b7280; font-size: 0.85rem;">No active requests...</p>';
        return;
      }
      Object.keys(data).forEach(rideId => {
        const ride = data[rideId];
        if (ride.status === 'pending') {
          const card = document.createElement('div');
          card.className = 'ride-card';
          card.innerHTML = `
            <h4>Service: ${ride.serviceType}</h4>
            <div class="ride-info">Client: <strong>${ride.clientName}</strong></div>
            <div class="ride-info">Route: <strong>${ride.pickup} ➔ ${ride.dropoff}</strong></div>
            <div class="ride-info">Client's Offered Fare: <strong style="color: green;">PKR ${ride.fare}</strong></div>
            <div class="bid-input-group">
              <input type="number" id="bid-price-${rideId}" placeholder="Your Counter Bid" value="${ride.fare}">
              <button class="btn btn-accent" onclick="window.sendDriverBid('${rideId}')">Send Counter Bid</button>
            </div>
          `;
          ridesList.appendChild(card);
        }
      });
    });

    window.sendDriverBid = function(rideId) {
      const price = document.getElementById(`bid-price-${rideId}`).value;
      const bidsRef = ref(db, `rides/${rideId}/bids`);
      const newBidRef = push(bidsRef);
      set(newBidRef, {
        driverId: currentUser.uid,
        driverName: currentUserProfile.name,
        driverPhone: currentUserProfile.phone,
        vehicleType: currentUserProfile.vehicleType || 'City Rides',
        amount: Number(price)
      }).then(() => alert('Counter bid sent to client successfully!'));
    };

    window.acceptBid = function(rideId, driverName, driverPhone, fare) {
      const rideRef = ref(db, `rides/${rideId}`);
      update(rideRef, {
        status: 'accepted',
        acceptedDriverName: driverName,
        acceptedDriverPhone: driverPhone,
        acceptedFare: fare
      });
    };

    function loadDriverWallet() {
      const walletRef = ref(db, `wallet/${currentUser.uid}`);
      onValue(walletRef, (snapshot) => {
        const data = snapshot.val();
        let total = data ? data.balance || 0 : 5000;
        document.getElementById('driver-daily-earnings').innerText = `${total} PKR`;
      });
    }

    window.sendChatMessage = function(rideId) {
      const input = document.getElementById(`chat-input-${rideId}`);
      const text = input.value.trim();
      if (!text) return;
      const chatRef = ref(db, `chats/${rideId}`);
      push(chatRef, {
        senderId: currentUser.uid,
        senderName: currentUserProfile.name,
        text,
        timestamp: Date.now()
      }).then(() => { input.value = ''; });
    };

    function listenToChatMessages(rideId) {
      const chatRef = ref(db, `chats/${rideId}`);
      onValue(chatRef, (snapshot) => {
        const data = snapshot.val();
        const chatBox = document.getElementById(`chat-box-${rideId}`);
        if (!chatBox) return;
        chatBox.innerHTML = '';
        if (data) {
          Object.values(data).forEach(msg => {
            const isMine = msg.senderId === currentUser.uid;
            const div = document.createElement('div');
            div.className = `chat-msg ${isMine ? 'msg-mine' : 'msg-other'}`;
            div.innerHTML = `<strong>${isMine ? 'You' : msg.senderName}:</strong> ${msg.text}`;
            chatBox.appendChild(div);
          });
          chatBox.scrollTop = chatBox.scrollHeight;
        }
      });
    }
  </script>
</body>
</html>
