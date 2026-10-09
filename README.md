<html lang="ur" dir="ltr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  
  <!-- SEO & GOOGLE META TAGS -->
  <title>GB Drive - Gilgit-Baltistan Live Taxi & Ride Sharing App</title>
  <meta name="description" content="GB Drive - Gilgit-Baltistan's #1 intelligent ride-sharing platform. Book rides with live map tracking, fair fuel-based pricing, bidding system, and complete safety verification.">
  <meta name="keywords" content="GB Drive, Gilgit Taxi, Skardu Ride Sharing, Hunza Taxi Service, InDrive Gilgit, Uber Pakistan, Ride Bidding App">
  <meta name="author" content="GB Drive Network">
  <meta name="robots" content="index, follow">

  <!-- OPEN GRAPH / SOCIAL MEDIA META TAGS -->
  <meta property="og:title" content="GB Drive - Smart Taxi & Ride Sharing">
  <meta property="og:description" content="Book affordable rides in Gilgit-Baltistan with live GPS tracking and fair bidding prices.">
  <meta property="og:type" content="website">
  <meta property="og:image" content="https://images.unsplash.com/photo-1449965408869-eaa3f722e40d?auto=format&fit=crop&w=1200&q=80">
  
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

    header h1 {
      font-size: 1.5rem;
      font-weight: 700;
    }

    header p {
      font-size: 0.8rem;
      opacity: 0.9;
    }

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

    #map {
      height: 220px;
      width: 100%;
      z-index: 1;
    }

    .content {
      padding: 15px;
    }

    .map-instruction {
      font-size: 0.8rem;
      color: #6b7280;
      text-align: center;
      margin-bottom: 10px;
      background: #eff6ff;
      padding: 6px;
      border-radius: 6px;
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

    .suggestion-item:hover {
      background: #eff6ff;
    }

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
      transition: background 0.2s;
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

    .ride-card h4 {
      color: var(--primary);
      margin-bottom: 6px;
      font-size: 0.95rem;
    }

    .ride-info {
      font-size: 0.85rem;
      margin-bottom: 4px;
    }

    .bid-input-group {
      display: flex;
      gap: 6px;
      margin-top: 8px;
    }

    .bid-input-group input {
      flex: 1;
      padding: 8px;
      border: 1px solid #d1d5db;
      border-radius: 6px;
    }

    .auth-toggle {
      text-align: center;
      margin-top: 12px;
      font-size: 0.85rem;
      color: var(--primary);
      cursor: pointer;
    }

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

    .msg-mine {
      background: #dbeafe;
      margin-left: auto;
      text-align: right;
    }

    .msg-other {
      background: #f3f4f6;
    }

    .hidden { display: none !important; }
  </style>
</head>
<body>

  <div class="app-container">
    <header>
      <h1>GB Drive</h1>
      <p id="app-tagline">Intelligent Smart Taxi Network</p>
      <div class="top-controls">
        <button class="top-btn" onclick="toggleLanguage()" id="lang-btn">English</button>
        <button id="logout-btn" class="top-btn hidden">Logout</button>
      </div>
    </header>

    <!-- STEP 1: AUTHENTICATION -->
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

    <!-- STEP 2: SAFETY PROFILE FORM -->
    <div id="profile-setup-section" class="content hidden">
      <h2 style="text-align: center; margin-bottom: 10px;">Safety Verification</h2>
      <form id="profile-setup-form">
        <div class="form-group">
          <label>Account Type</label>
          <select id="setup-role" onchange="toggleDriverSetupFields()">
            <option value="passenger">Passenger</option>
            <option value="driver">Driver</option>
          </select>
        </div>
        <div class="form-group">
          <label>Full Name (CNIC Name)</label>
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
          <label>Emergency Contact Phone</label>
          <input type="text" id="setup-emergency" placeholder="Emergency Relative Number" required>
        </div>

        <div id="driver-setup-fields" class="hidden">
          <div class="form-group">
            <label>Driving License Number</label>
            <input type="text" id="setup-license" placeholder="License Number">
          </div>
          <div class="form-group">
            <label>Vehicle Type</label>
            <select id="setup-vehicle-type">
              <option value="Car">Car / Taxi (Petrol)</option>
              <option value="Bike">Bike / Rickshaw</option>
              <option value="Van">Van / Hiace (Diesel)</option>
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

    <!-- STEP 3: MAIN APP DASHBOARD -->
    <div id="app-section" class="hidden">
      <div class="content" style="padding-bottom: 0;">
        <div class="user-badge">
          <span id="user-display-name">Welcome User</span>
          <strong id="user-display-role" style="text-transform: uppercase;">PASSENGER</strong>
        </div>

        <!-- EMERGENCY SOS BUTTON -->
        <button class="btn btn-danger" onclick="triggerSOSAlert()" style="font-size: 0.85rem; padding: 8px; margin-bottom: 10px;">
          🚨 EMERGENCY SOS ALERT (Share Live Location)
        </button>
      </div>

      <!-- Map Container -->
      <div id="map"></div>

      <div class="content">
        <!-- PASSENGER DASHBOARD -->
        <div id="passenger-section" class="hidden">
          <div class="map-instruction" id="map-instr-text">
            📍 Location type karein ya map par click karke route choose karein.
          </div>

          <div id="fare-badge" class="fare-calculator-badge hidden">
            <div>📏 Distance: <strong id="calc-distance">0 km</strong></div>
            <div>⛽ Fuel Fare Estimate: <strong id="calc-fare" style="color: #059669; font-size: 1.1rem;">0 PKR</strong></div>
          </div>
          
          <form id="ride-form" autocomplete="off">
            <div class="form-group">
              <label id="lbl-vehicle-type">Vehicle Type</label>
              <select id="ride-vehicle-type" onchange="calculateFuelFare()">
                <option value="Car">Car / Taxi (Petrol ~ PKR 48/km)</option>
                <option value="Bike">Bike / Rickshaw (~ PKR 16/km)</option>
                <option value="Van">Van / Hiace (Diesel ~ PKR 58/km)</option>
              </select>
            </div>

            <div class="form-group">
              <label id="lbl-pickup">Pickup Location</label>
              <input type="text" id="pickup" placeholder="Type location e.g. Gilgit Bazaar" oninput="searchLocation('pickup')" required>
              <div id="pickup-suggestions" class="suggestions-box hidden"></div>
            </div>

            <!-- OPTIONAL VIA-STOP -->
            <div class="form-group">
              <label>Via Stop (Optional)</label>
              <input type="text" id="via-stop" placeholder="Optional stop on the way" oninput="searchLocation('via-stop')">
              <div id="via-stop-suggestions" class="suggestions-box hidden"></div>
            </div>

            <div class="form-group">
              <label id="lbl-dropoff">Dropoff Location</label>
              <input type="text" id="dropoff" placeholder="Type location e.g. Skardu Airport" oninput="searchLocation('dropoff')" required>
              <div id="dropoff-suggestions" class="suggestions-box hidden"></div>
            </div>

            <!-- SCHEDULE FOR LATER -->
            <div class="form-group">
              <label>Schedule Date & Time (Optional)</label>
              <input type="datetime-local" id="schedule-time">
            </div>

            <div class="form-group">
              <label id="lbl-fare">Your Offer Fare (PKR)</label>
              <input type="number" id="fare" placeholder="Recommended fare auto-applies" required>
            </div>
            <button type="submit" class="btn" id="btn-offer-ride">Offer Ride Now</button>
          </form>

          <div id="passenger-ride-status" style="margin-top: 15px;"></div>
        </div>

        <!-- DRIVER DASHBOARD -->
        <div id="driver-section" class="hidden">
          <div class="driver-dashboard-header">
            <div>Status: <strong id="driver-online-text" style="color: #10b981;">ONLINE</strong></div>
            <button class="btn btn-warning" id="driver-toggle-btn" onclick="toggleDriverOnlineStatus()" style="width: auto; padding: 6px 12px; margin:0; font-size: 0.8rem;">Go Offline</button>
          </div>

          <div class="fare-calculator-badge" style="background: #eff6ff; border-color: #2563eb; color: #1e40af;">
            💰 Driver Wallet Earnings: <strong id="driver-daily-earnings" style="font-size: 1.1rem;">0 PKR</strong>
          </div>

          <h3>Available Live Rides</h3>
          <div id="rides-list" style="margin-top: 10px;">
            <p style="color: #6b7280; font-size: 0.85rem;">Searching for passenger requests...</p>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Leaflet JS -->
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

  <!-- FIREBASE SDKs -->
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
    import { 
      getAuth, 
      signInWithEmailAndPassword, 
      createUserWithEmailAndPassword, 
      GoogleAuthProvider, 
      signInWithPopup, 
      onAuthStateChanged, 
      signOut 
    } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-auth.js";
    import { getDatabase, ref, push, set, onValue, update, get } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

    // Firebase Config
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
    let isSignUpMode = false;
    let isDriverOnline = true;
    let currentLanguage = 'ur';

    // LANGUAGE SWITCHER ENGINE
    window.toggleLanguage = function() {
      currentLanguage = currentLanguage === 'ur' ? 'en' : 'ur';
      document.getElementById('lang-btn').innerText = currentLanguage === 'ur' ? 'English' : 'اردو';
      
      if (currentLanguage === 'en') {
        document.getElementById('app-tagline').innerText = 'Intelligent Smart Taxi Network';
        document.getElementById('lbl-email').innerText = 'Email Address';
        document.getElementById('lbl-password').innerText = 'Password';
        document.getElementById('btn-offer-ride').innerText = 'Offer Ride Now';
        document.getElementById('map-instr-text').innerText = '📍 Type location or click on map to choose route.';
      } else {
        document.getElementById('app-tagline').innerText = 'گلگت بلتستان اسمارٹ ٹیکسی سروس';
        document.getElementById('lbl-email').innerText = 'ای میل ایڈریس';
        document.getElementById('lbl-password').innerText = 'پاس ورڈ';
        document.getElementById('btn-offer-ride').innerText = 'رائڈ آفر کریں';
        document.getElementById('map-instr-text').innerText = '📍 لوکیشن ٹائپ کریں یا میپ پر کلک کریں۔';
      }
    };

    // AUTH TOGGLE
    window.toggleAuthMode = function() {
      isSignUpMode = !isSignUpMode;
      document.getElementById('auth-title').innerText = isSignUpMode ? 'Register on GB Drive' : 'Login to GB Drive';
      document.getElementById('auth-submit-btn').innerText = isSignUpMode ? 'Sign Up' : 'Login';
      document.getElementById('auth-toggle-btn').innerText = isSignUpMode ? 'Already have an account? Login' : "Don't have an account? Sign Up";
    };

    window.toggleDriverSetupFields = function() {
      const role = document.getElementById('setup-role').value;
      if (role === 'driver') {
        document.getElementById('driver-setup-fields').classList.remove('hidden');
      } else {
        document.getElementById('driver-setup-fields').classList.add('hidden');
      }
    };

    // AUTH ACTIONS
    const authForm = document.getElementById('auth-form');
    authForm.addEventListener('submit', async (e) => {
      e.preventDefault();
      const email = document.getElementById('auth-email').value;
      const password = document.getElementById('auth-password').value;

      try {
        if (isSignUpMode) {
          await createUserWithEmailAndPassword(auth, email, password);
        } else {
          await signInWithEmailAndPassword(auth, email, password);
        }
      } catch (err) { alert('Auth Error: ' + err.message); }
    });

    document.getElementById('google-login-btn').addEventListener('click', async () => {
      try { await signInWithPopup(auth, googleProvider); } catch (err) { alert('Google Login Error: ' + err.message); }
    });

    document.getElementById('logout-btn').addEventListener('click', () => signOut(auth));

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
          document.getElementById('app-section').classList.add('hidden');
          document.getElementById('setup-name').value = user.displayName || '';
        }
      } else {
        currentUser = null;
        currentUserProfile = null;
        document.getElementById('auth-section').classList.remove('hidden');
        document.getElementById('profile-setup-section').classList.add('hidden');
        document.getElementById('app-section').classList.add('hidden');
        document.getElementById('logout-btn').classList.add('hidden');
      }
    });

    // PROFILE SETUP
    const profileSetupForm = document.getElementById('profile-setup-form');
    profileSetupForm.addEventListener('submit', async (e) => {
      e.preventDefault();
      const role = document.getElementById('setup-role').value;
      const profileData = {
        uid: currentUser.uid,
        email: currentUser.email,
        name: document.getElementById('setup-name').value,
        cnic: document.getElementById('setup-cnic').value,
        phone: document.getElementById('setup-phone').value,
        emergencyPhone: document.getElementById('setup-emergency').value,
        role,
        isProfileComplete: true,
        updatedAt: Date.now()
      };

      if (role === 'driver') {
        profileData.licenseNum = document.getElementById('setup-license').value;
        profileData.vehicleType = document.getElementById('setup-vehicle-type').value;
        profileData.vehicleNum = document.getElementById('setup-vehicle-num').value;
      }

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
        loadDriverEarnings();
      } else {
        document.getElementById('passenger-section').classList.remove('hidden');
        document.getElementById('driver-section').classList.add('hidden');
      }

      initMap();
    }

    // EMERGENCY SOS TRIGGER
    window.triggerSOSAlert = function() {
      if (!currentUserProfile) return;
      const emergencyNo = currentUserProfile.emergencyPhone || '03000000000';
      const mapLink = pickupMarker ? `https://maps.google.com/?q=${pickupMarker.getLatLng().lat},${pickupMarker.getLatLng().lng}` : 'Live Location';
      const message = `EMERGENCY SOS ALERT! I am in danger during GB Drive Ride. Track Location: ${mapLink}`;
      
      window.open(`https://wa.me/${emergencyNo}?text=${encodeURIComponent(message)}`, '_blank');
    };

    // DRIVER ONLINE/OFFLINE TOGGLE
    window.toggleDriverOnlineStatus = function() {
      isDriverOnline = !isDriverOnline;
      const statusText = document.getElementById('driver-online-text');
      const toggleBtn = document.getElementById('driver-toggle-btn');

      if (isDriverOnline) {
        statusText.innerText = 'ONLINE';
        statusText.style.color = '#10b981';
        toggleBtn.innerText = 'Go Offline';
        toggleBtn.className = 'btn btn-warning';
      } else {
        statusText.innerText = 'OFFLINE';
        statusText.style.color = '#ef4444';
        toggleBtn.innerText = 'Go Online';
        toggleBtn.className = 'btn btn-accent';
      }
    };

    // LEAFLET MAP & AUTOCOMPLETE
    let map = null;
    let pickupMarker = null;
    let dropoffMarker = null;
    let routePolyline = null;
    let clickState = 'pickup';
    let routeDistanceKm = 0;
    let searchDebounce = null;

    function initMap() {
      if (map) return;

      map = L.map('map').setView([35.9208, 74.3144], 12);
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', { attribution: '© OpenStreetMap' }).addTo(map);

      map.on('click', (e) => {
        const { lat, lng } = e.latlng;
        if (clickState === 'pickup') {
          setPickupPoint(lat, lng, `Pin (${lat.toFixed(4)}, ${lng.toFixed(4)})`);
          clickState = 'dropoff';
        } else {
          setDropoffPoint(lat, lng, `Pin (${lat.toFixed(4)}, ${lng.toFixed(4)})`);
          clickState = 'pickup';
        }
      });
    }

    window.searchLocation = function(type) {
      const query = document.getElementById(type).value;
      const suggestionsBox = document.getElementById(`${type}-suggestions`);

      if (query.length < 3) {
        suggestionsBox.classList.add('hidden');
        return;
      }

      clearTimeout(searchDebounce);
      searchDebounce = setTimeout(async () => {
        try {
          const res = await fetch(`https://nominatim.openstreetmap.org/search?format=json&q=${encodeURIComponent(query)}&countrycodes=pk&limit=5`);
          const data = await res.json();

          suggestionsBox.innerHTML = '';
          if (data.length === 0) {
            suggestionsBox.classList.add('hidden');
            return;
          }

          data.forEach(item => {
            const div = document.createElement('div');
            div.className = 'suggestion-item';
            div.innerText = item.display_name;
            div.onclick = () => {
              document.getElementById(type).value = item.display_name.split(',')[0];
              suggestionsBox.classList.add('hidden');

              const lat = parseFloat(item.lat);
              const lon = parseFloat(item.lon);

              if (type === 'pickup') setPickupPoint(lat, lon, item.display_name);
              else setDropoffPoint(lat, lon, item.display_name);
            };
            suggestionsBox.appendChild(div);
          });

          suggestionsBox.classList.remove('hidden');
        } catch (e) { console.error(e); }
      }, 300);
    };

    function setPickupPoint(lat, lng, label) {
      if (pickupMarker) map.removeLayer(pickupMarker);
      pickupMarker = L.marker([lat, lng]).addTo(map).bindPopup('Pickup: ' + label).openPopup();
      document.getElementById('pickup').value = label;
      map.setView([lat, lng], 13);
      updateRoute();
    }

    function setDropoffPoint(lat, lng, label) {
      if (dropoffMarker) map.removeLayer(dropoffMarker);
      dropoffMarker = L.marker([lat, lng]).addTo(map).bindPopup('Dropoff: ' + label).openPopup();
      document.getElementById('dropoff').value = label;
      updateRoute();
    }

    function updateRoute() {
      if (pickupMarker && dropoffMarker) {
        if (routePolyline) map.removeLayer(routePolyline);

        const pLat = pickupMarker.getLatLng();
        const dLat = dropoffMarker.getLatLng();

        routePolyline = L.polyline([pLat, dLat], { color: '#2563eb', weight: 4 }).addTo(map);
        map.fitBounds(routePolyline.getBounds(), { padding: [20, 20] });

        const distanceMeters = pLat.distanceTo(dLat);
        routeDistanceKm = (distanceMeters / 1000) * 1.35;
        calculateFuelFare();
      }
    }

    window.calculateFuelFare = function() {
      if (!routeDistanceKm) return;

      const vehicleType = document.getElementById('ride-vehicle-type').value;
      let ratePerKm = 48;

      if (vehicleType === 'Bike') ratePerKm = 16;
      else if (vehicleType === 'Van') ratePerKm = 58;

      let calculatedFare = Math.round(120 + (routeDistanceKm * ratePerKm));

      document.getElementById('calc-distance').innerText = `${routeDistanceKm.toFixed(1)} km`;
      document.getElementById('calc-fare').innerText = `${calculatedFare} PKR`;
      document.getElementById('fare').value = calculatedFare;
      document.getElementById('fare-badge').classList.remove('hidden');
    };

    // RIDE BROADCAST WITH OTP GENERATION & SCHEDULING
    const rideForm = document.getElementById('ride-form');
    rideForm.addEventListener('submit', (e) => {
      e.preventDefault();

      const pickup = document.getElementById('pickup').value;
      const viaStop = document.getElementById('via-stop').value || 'None';
      const dropoff = document.getElementById('dropoff').value;
      const scheduleTime = document.getElementById('schedule-time').value || 'Immediate';
      const fare = document.getElementById('fare').value;
      const vehicleType = document.getElementById('ride-vehicle-type').value;
      const otpCode = Math.floor(1000 + Math.random() * 9000);

      const ridesRef = ref(db, 'rides/');
      const newRideRef = push(ridesRef);

      set(newRideRef, {
        passengerId: currentUser.uid,
        passengerName: currentUserProfile.name,
        passengerPhone: currentUserProfile.phone,
        pickup,
        viaStop,
        dropoff,
        scheduleTime,
        distanceKm: routeDistanceKm.toFixed(1),
        vehicleType,
        fare: Number(fare),
        otpCode,
        status: 'pending',
        createdAt: Date.now()
      }).then(() => {
        alert('Ride Request Posted!');
        listenToMyRide(newRideRef.key);
      });
    });

    function listenToMyRide(rideId) {
      const rideRef = ref(db, `rides/${rideId}`);
      const statusDiv = document.getElementById('passenger-ride-status');

      onValue(rideRef, (snapshot) => {
        const ride = snapshot.val();
        if (!ride) return;

        if (ride.status === 'pending' && !ride.bids) {
          statusDiv.innerHTML = `<div class="ride-card">⏳ Broadcasted! Waiting for drivers...</div>`;
        } else if (ride.bids && ride.status === 'pending') {
          let html = `<h4>Drivers Offered Bids:</h4>`;
          Object.keys(ride.bids).forEach(bidId => {
            const bid = ride.bids[bidId];
            html += `
              <div class="ride-card">
                <div>Driver: <strong>${bid.driverName} (${bid.vehicleType})</strong></div>
                <div>Bid Fare: <strong style="color: green;">PKR ${bid.amount}</strong></div>
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
              ✅ <strong>Ride Accepted!</strong><br>
              Driver: ${ride.acceptedDriverName} (${ride.acceptedFare} PKR)<br>
              🔐 <strong>Your Start Ride OTP: <span style="font-size: 1.2rem; color: #1e40af;">${ride.otpCode}</span></strong>
            </div>

            <!-- IN-APP CHAT -->
            <div class="chat-container">
              <h5 style="margin-bottom:6px;">💬 In-App Live Chat with Driver</h5>
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

    // DRIVER RIDES MONITOR & OTP VERIFY
    const ridesList = document.getElementById('rides-list');
    const ridesRef = ref(db, 'rides/');

    onValue(ridesRef, (snapshot) => {
      if (!currentUserProfile || currentUserProfile.role !== 'driver' || !isDriverOnline) return;
      const data = snapshot.val();
      ridesList.innerHTML = '';

      if (!data) {
        ridesList.innerHTML = '<p style="color: #6b7280; font-size: 0.85rem;">No active ride requests...</p>';
        return;
      }

      Object.keys(data).forEach(rideId => {
        const ride = data[rideId];
        if (ride.status === 'pending') {
          const card = document.createElement('div');
          card.className = 'ride-card';
          card.innerHTML = `
            <h4>Passenger: ${ride.passengerName} (${ride.vehicleType || 'Taxi'})</h4>
            <div class="ride-info">Route: <strong>${ride.pickup} ➔ ${ride.dropoff}</strong></div>
            <div class="ride-info">Via Stop: <strong>${ride.viaStop || 'None'}</strong></div>
            <div class="ride-info">Schedule: <strong>${ride.scheduleTime || 'Immediate'}</strong></div>
            <div class="ride-info">Distance: <strong>${ride.distanceKm || 'N/A'} km</strong></div>
            <div class="ride-info">Offered Fuel Fare: <strong style="color: green;">PKR ${ride.fare}</strong></div>
            <div class="bid-input-group">
              <input type="number" id="bid-price-${rideId}" placeholder="Counter Fare" value="${ride.fare}">
              <button class="btn btn-accent" onclick="window.sendDriverBid('${rideId}')">Send Bid</button>
            </div>
          `;
          ridesList.appendChild(card);
        } else if (ride.status === 'accepted' && ride.acceptedDriverPhone === currentUserProfile.phone) {
          const card = document.createElement('div');
          card.className = 'ride-card';
          card.style.background = '#eff6ff';
          card.innerHTML = `
            <h4>Active Trip with ${ride.passengerName}</h4>
            <div class="ride-info">Fare: <strong>PKR ${ride.acceptedFare}</strong></div>
            <div class="bid-input-group">
              <input type="number" id="verify-otp-${rideId}" placeholder="Enter Passenger 4-Digit OTP">
              <button class="btn btn-accent" onclick="window.verifyRideOTP('${rideId}', ${ride.otpCode}, ${ride.acceptedFare})">Start & Complete Ride</button>
            </div>

            <!-- IN-APP CHAT FOR DRIVER -->
            <div class="chat-container" style="margin-top:10px;">
              <h5 style="margin-bottom:6px;">💬 Live Chat with Passenger</h5>
              <div class="chat-messages" id="chat-box-${rideId}"></div>
              <div style="display:flex; gap:6px;">
                <input type="text" id="chat-input-${rideId}" placeholder="Type message..." style="flex:1; padding:6px; border-radius:6px; border:1px solid #ccc;">
                <button class="btn btn-accent" onclick="window.sendChatMessage('${rideId}')" style="width:auto; padding:6px 12px; margin:0;">Send</button>
              </div>
            </div>
          `;
          ridesList.appendChild(card);
          listenToChatMessages(rideId);
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
        vehicleType: currentUserProfile.vehicleType || 'Taxi',
        amount: Number(price)
      }).then(() => alert('Bid offer sent to passenger!'));
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

    window.verifyRideOTP = function(rideId, correctOtp, fareAmount) {
      const enteredOtp = document.getElementById(`verify-otp-${rideId}`).value;
      if (Number(enteredOtp) === correctOtp) {
        const rideRef = ref(db, `rides/${rideId}`);
        update(rideRef, { status: 'completed' }).then(() => {
          alert('Ride Completed Successfully!');
          const earningsRef = ref(db, `earnings/${currentUser.uid}/${Date.now()}`);
          set(earningsRef, { amount: fareAmount });
          loadDriverEarnings();
        });
      } else {
        alert('Incorrect OTP Code!');
      }
    };

    function loadDriverEarnings() {
      const earningsRef = ref(db, `earnings/${currentUser.uid}`);
      onValue(earningsRef, (snapshot) => {
        const data = snapshot.val();
        let total = 0;
        if (data) {
          Object.values(data).forEach(item => total += item.amount);
        }
        document.getElementById('driver-daily-earnings').innerText = `${total} PKR`;
      });
    }

    // IN-APP CHAT LOGIC
    window.sendChatMessage = function(rideId) {
      const input = document.getElementById(`chat-input-${rideId}`);
      const text = input.value.trim();
      if (!text) return;

      const chatRef = ref(db, `chats/${rideId}`);
      const newMsgRef = push(chatRef);

      set(newMsgRef, {
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
