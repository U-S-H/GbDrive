<html lang="ur" dir="ltr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>GB Drive - Fuel Rate Calculated Ride App</title>
  
  <!-- Leaflet CSS for OpenStreetMap -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  
  <style>
    :root {
      --primary: #2563eb;
      --primary-dark: #1d4ed8;
      --accent: #10b981;
      --danger: #ef4444;
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

    .logout-btn {
      position: absolute;
      right: 15px;
      top: 15px;
      background: rgba(255, 255, 255, 0.2);
      border: none;
      color: white;
      padding: 6px 12px;
      border-radius: 6px;
      font-size: 0.8rem;
      cursor: pointer;
    }

    #map {
      height: 240px;
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
    .btn-whatsapp { background: #25d366; }

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

    .hidden { display: none !important; }
  </style>
</head>
<body>

  <div class="app-container">
    <header>
      <h1>GB Drive</h1>
      <p>Fuel & Distance Rate Intelligent Taxi App</p>
      <button id="logout-btn" class="logout-btn hidden">Logout</button>
    </header>

    <!-- STEP 1: AUTHENTICATION -->
    <div id="auth-section" class="content">
      <h2 id="auth-title" style="text-align: center; margin-bottom: 15px;">Login to GB Drive</h2>
      <button id="google-login-btn" class="btn btn-google">Continue with Google</button>
      <div style="text-align: center; margin: 10px 0; color: #9ca3af; font-size: 0.8rem;">OR EMAIL LOGIN</div>

      <form id="auth-form">
        <div class="form-group">
          <label>Email Address</label>
          <input type="email" id="auth-email" placeholder="name@example.com" required>
        </div>
        <div class="form-group">
          <label>Password</label>
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
      <h2 style="text-align: center; margin-bottom: 10px;">Safety Profile Verification</h2>
      <form id="profile-setup-form">
        <div class="form-group">
          <label>Account Type</label>
          <select id="setup-role" onchange="toggleDriverSetupFields()">
            <option value="passenger">Passenger</option>
            <option value="driver">Driver</option>
          </select>
        </div>
        <div class="form-group">
          <label>Full Name (Identity Name)</label>
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
      </div>

      <!-- Map Container -->
      <div id="map"></div>

      <div class="content">
        <!-- PASSENGER DASHBOARD -->
        <div id="passenger-section" class="hidden">
          <div class="map-instruction">
            📍 Map par <b>Pickup</b> aur <b>Dropoff</b> choose karein (Distance auto-calculate hoga).
          </div>

          <div id="fare-badge" class="fare-calculator-badge hidden">
            <div>📏 Calculated Distance: <strong id="calc-distance">0 km</strong></div>
            <div>⛽ Fuel Based Fare Estimate: <strong id="calc-fare" style="color: #059669; font-size: 1.1rem;">0 PKR</strong></div>
          </div>
          
          <form id="ride-form">
            <div class="form-group">
              <label>Vehicle Ride Type</label>
              <select id="ride-vehicle-type" onchange="calculateFuelFare()">
                <option value="Car">Car / Taxi (Petrol ~ PKR 35/km)</option>
                <option value="Bike">Bike / Rickshaw (~ PKR 12/km)</option>
                <option value="Van">Van / Hiace (Diesel ~ PKR 45/km)</option>
              </select>
            </div>
            <div class="form-group">
              <label>Pickup Location</label>
              <input type="text" id="pickup" placeholder="Map click ya location" required>
            </div>
            <div class="form-group">
              <label>Dropoff Location</label>
              <input type="text" id="dropoff" placeholder="Map click ya location" required>
            </div>
            <div class="form-group">
              <label>Your Offer Fare (PKR)</label>
              <input type="number" id="fare" placeholder="Recommended fare auto-applies" required>
            </div>
            <button type="submit" class="btn">Broadcast Ride Offer</button>
          </form>

          <div id="passenger-ride-status" style="margin-top: 15px;"></div>
        </div>

        <!-- DRIVER DASHBOARD -->
        <div id="driver-section" class="hidden">
          <h3>Available Rides (Fuel Fair Prices)</h3>
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

    // --- AUTH TOGGLE ---
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
      } else {
        document.getElementById('passenger-section').classList.remove('hidden');
        document.getElementById('driver-section').classList.add('hidden');
      }

      initMap();
    }

    // --- LEAFLET MAP & FUEL FARE ENGINE ---
    let map = null;
    let pickupMarker = null;
    let dropoffMarker = null;
    let routePolyline = null;
    let clickState = 'pickup';
    let routeDistanceKm = 0;

    function initMap() {
      if (map) return;

      map = L.map('map').setView([35.9208, 74.3144], 12);
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', { attribution: '© OpenStreetMap' }).addTo(map);

      map.on('click', (e) => {
        const { lat, lng } = e.latlng;
        const coordsText = `${lat.toFixed(4)}, ${lng.toFixed(4)}`;

        if (clickState === 'pickup') {
          if (pickupMarker) map.removeLayer(pickupMarker);
          pickupMarker = L.marker([lat, lng]).addTo(map).bindPopup('Pickup Location').openPopup();
          document.getElementById('pickup').value = `Gilgit Pin (${coordsText})`;
          clickState = 'dropoff';
        } else {
          if (dropoffMarker) map.removeLayer(dropoffMarker);
          dropoffMarker = L.marker([lat, lng]).addTo(map).bindPopup('Dropoff Location').openPopup();
          document.getElementById('dropoff').value = `Drop Pin (${coordsText})`;
          clickState = 'pickup';
        }

        if (pickupMarker && dropoffMarker) {
          if (routePolyline) map.removeLayer(routePolyline);
          
          const pLat = pickupMarker.getLatLng();
          const dLat = dropoffMarker.getLatLng();
          
          routePolyline = L.polyline([pLat, dLat], { color: '#2563eb', weight: 4 }).addTo(map);
          map.fitBounds(routePolyline.getBounds(), { padding: [20, 20] });

          // Calculate Straight Distance (Haversine Formula)
          const distanceMeters = pLat.distanceTo(dLat);
          routeDistanceKm = (distanceMeters / 1000) * 1.3; // 1.3 Factor for Road Route estimation
          window.calculateFuelFare();
        }
      });
    }

    window.calculateFuelFare = function() {
      if (!routeDistanceKm) return;

      const vehicleType = document.getElementById('ride-vehicle-type').value;
      let ratePerKm = 35; // Car Petrol Default

      if (vehicleType === 'Bike') ratePerKm = 12;
      else if (vehicleType === 'Van') ratePerKm = 45;

      // Base fare 100 PKR + Distance * Fuel Rate
      let calculatedFare = Math.round(100 + (routeDistanceKm * ratePerKm));

      document.getElementById('calc-distance').innerText = `${routeDistanceKm.toFixed(1)} km`;
      document.getElementById('calc-fare').innerText = `${calculatedFare} PKR`;
      document.getElementById('fare').value = calculatedFare;
      document.getElementById('fare-badge').classList.remove('hidden');
    };

    // RIDE BROADCAST & BIDDING SYSTEM
    const rideForm = document.getElementById('ride-form');
    rideForm.addEventListener('submit', (e) => {
      e.preventDefault();

      const pickup = document.getElementById('pickup').value;
      const dropoff = document.getElementById('dropoff').value;
      const fare = document.getElementById('fare').value;
      const vehicleType = document.getElementById('ride-vehicle-type').value;

      const ridesRef = ref(db, 'rides/');
      const newRideRef = push(ridesRef);

      set(newRideRef, {
        passengerId: currentUser.uid,
        passengerName: currentUserProfile.name,
        passengerPhone: currentUserProfile.phone,
        pickup,
        dropoff,
        distanceKm: routeDistanceKm.toFixed(1),
        vehicleType,
        fare: Number(fare),
        status: 'pending',
        createdAt: Date.now()
      }).then(() => {
        alert('Ride Request Broadcasted with Fuel Rate Fare!');
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
        } else if (ride.bids) {
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
          const waUrl = `https://wa.me/${ride.acceptedDriverPhone}?text=Hi%20${ride.acceptedDriverName},%20I%20accepted%20your%20offer%20of%20PKR%20${ride.acceptedFare}%20for%20GB%20Drive`;
          statusDiv.innerHTML = `
            <div class="ride-card" style="background: #d1fae5; border-color: #10b981;">
              ✅ <strong>Ride Accepted!</strong><br>
              Driver: ${ride.acceptedDriverName} (${ride.acceptedFare} PKR)<br><br>
              <a href="${waUrl}" target="_blank" class="btn btn-whatsapp" style="display:block; text-align:center; text-decoration:none;">Open WhatsApp Chat</a>
            </div>
          `;
        }
      });
    }

    // DRIVER MONITOR
    const ridesList = document.getElementById('rides-list');
    const ridesRef = ref(db, 'rides/');

    onValue(ridesRef, (snapshot) => {
      if (!currentUserProfile || currentUserProfile.role !== 'driver') return;
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
            <div class="ride-info">Distance: <strong>${ride.distanceKm || 'N/A'} km</strong></div>
            <div class="ride-info">Offered Fuel Fare: <strong style="color: green;">PKR ${ride.fare}</strong></div>
            <div class="bid-input-group">
              <input type="number" id="bid-price-${rideId}" placeholder="Counter Fare" value="${ride.fare}">
              <button class="btn btn-accent" onclick="window.sendDriverBid('${rideId}')">Send Bid</button>
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
  </script>
</body>
</html>
