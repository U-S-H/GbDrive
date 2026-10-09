<html lang="ur" dir="ltr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>GB Drive - Ultimate InDrive & Uber Clone</title>
  
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

    .btn-google {
      background: #ea4335;
      margin-bottom: 10px;
    }

    .btn-accent { background: var(--accent); }
    .btn-danger { background: var(--danger); }
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
      <p>Gilgit-Baltistan Live Ride & Bidding</p>
      <button id="logout-btn" class="logout-btn hidden">Logout</button>
    </header>

    <!-- AUTH SECTION (Login / Signup) -->
    <div id="auth-section" class="content">
      <h2 id="auth-title" style="text-align: center; margin-bottom: 15px;">Login to GB Drive</h2>

      <button id="google-login-btn" class="btn btn-google">Continue with Google</button>
      <div style="text-align: center; margin: 10px 0; color: #9ca3af; font-size: 0.8rem;">OR EMAIL LOGIN</div>

      <form id="auth-form">
        <!-- Extra Profile Fields for Registration -->
        <div id="register-fields" class="hidden">
          <div class="form-group">
            <label>Select Your Account Type</label>
            <select id="user-role" onchange="toggleDriverFields()">
              <option value="passenger">Passenger</option>
              <option value="driver">Driver</option>
            </select>
          </div>

          <div class="form-group">
            <label>Full Name</label>
            <input type="text" id="user-name" placeholder="Enter full name">
          </div>

          <div class="form-group">
            <label>WhatsApp Phone Number</label>
            <input type="text" id="user-phone" placeholder="e.g. 03001234567">
          </div>

          <!-- Driver Specific Fields -->
          <div id="driver-fields" class="hidden">
            <div class="form-group">
              <label>Vehicle Type</label>
              <select id="vehicle-type">
                <option value="Car">Car / Taxi</option>
                <option value="Bike">Bike / Rickshaw</option>
                <option value="Van">Van / Hiace</option>
              </select>
            </div>
            <div class="form-group">
              <label>Vehicle Number Plate</label>
              <input type="text" id="vehicle-num" placeholder="e.g. GIL-1234">
            </div>
          </div>
        </div>

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

    <!-- MAIN APP DASHBOARD (Hidden until logged in) -->
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
            📍 Map par click karke <b>Pickup</b> aur <b>Dropoff</b> choose karein.
          </div>
          
          <form id="ride-form">
            <div class="form-group">
              <label>Pickup Location</label>
              <input type="text" id="pickup" placeholder="Map click ya location likhein" required>
            </div>
            <div class="form-group">
              <label>Dropoff Location</label>
              <input type="text" id="dropoff" placeholder="Map click ya location likhein" required>
            </div>
            <div class="form-group">
              <label>Your Offer Fare (PKR)</label>
              <input type="number" id="fare" placeholder="e.g. 1500" required>
            </div>
            <button type="submit" class="btn">Broadcast Ride Offer</button>
          </form>

          <div id="passenger-ride-status" style="margin-top: 15px;"></div>
        </div>

        <!-- DRIVER DASHBOARD -->
        <div id="driver-section" class="hidden">
          <h3>Available Rides for Drivers</h3>
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

    // --- AUTH UI CONTROLS ---
    window.toggleAuthMode = function() {
      isSignUpMode = !isSignUpMode;
      document.getElementById('auth-title').innerText = isSignUpMode ? 'Register on GB Drive' : 'Login to GB Drive';
      document.getElementById('auth-submit-btn').innerText = isSignUpMode ? 'Sign Up' : 'Login';
      document.getElementById('auth-toggle-btn').innerText = isSignUpMode ? 'Already have an account? Login' : "Don't have an account? Sign Up";
      
      if (isSignUpMode) {
        document.getElementById('register-fields').classList.remove('hidden');
      } else {
        document.getElementById('register-fields').classList.add('hidden');
      }
    };

    window.toggleDriverFields = function() {
      const role = document.getElementById('user-role').value;
      if (role === 'driver') {
        document.getElementById('driver-fields').classList.remove('hidden');
      } else {
        document.getElementById('driver-fields').classList.add('hidden');
      }
    };

    // --- AUTHENTICATION LOGIC ---
    const authForm = document.getElementById('auth-form');
    authForm.addEventListener('submit', async (e) => {
      e.preventDefault();
      const email = document.getElementById('auth-email').value;
      const password = document.getElementById('auth-password').value;

      try {
        if (isSignUpMode) {
          const res = await createUserWithEmailAndPassword(auth, email, password);
          const uid = res.user.uid;
          
          const role = document.getElementById('user-role').value;
          const name = document.getElementById('user-name').value || 'User';
          const phone = document.getElementById('user-phone').value || '';

          const profileData = {
            uid,
            name,
            phone,
            email,
            role,
            createdAt: Date.now()
          };

          if (role === 'driver') {
            profileData.vehicleType = document.getElementById('vehicle-type').value;
            profileData.vehicleNum = document.getElementById('vehicle-num').value;
          }

          await set(ref(db, `users/${uid}`), profileData);
        } else {
          await signInWithEmailAndPassword(auth, email, password);
        }
      } catch (err) {
        alert('Authentication Error: ' + err.message);
      }
    });

    // Google Login
    document.getElementById('google-login-btn').addEventListener('click', async () => {
      try {
        const res = await signInWithPopup(auth, googleProvider);
        const uid = res.user.uid;

        // Check if user profile already exists
        const snapshot = await get(ref(db, `users/${uid}`));
        if (!snapshot.exists()) {
          // Default Google user as Passenger
          await set(ref(db, `users/${uid}`), {
            uid,
            name: res.user.displayName,
            email: res.user.email,
            phone: '',
            role: 'passenger',
            createdAt: Date.now()
          });
        }
      } catch (err) {
        alert('Google Sign-In Error: ' + err.message);
      }
    });

    // Logout
    document.getElementById('logout-btn').addEventListener('click', () => {
      signOut(auth);
    });

    // AUTH STATE MONITORING
    onAuthStateChanged(auth, async (user) => {
      if (user) {
        currentUser = user;
        const snapshot = await get(ref(db, `users/${user.uid}`));
        currentUserProfile = snapshot.val() || { role: 'passenger', name: user.email };

        document.getElementById('auth-section').classList.add('hidden');
        document.getElementById('app-section').classList.remove('hidden');
        document.getElementById('logout-btn').classList.remove('hidden');

        document.getElementById('user-display-name').innerText = currentUserProfile.name || user.email;
        document.getElementById('user-display-role').innerText = currentUserProfile.role;

        // Load respective dashboard
        if (currentUserProfile.role === 'driver') {
          document.getElementById('driver-section').classList.remove('hidden');
          document.getElementById('passenger-section').classList.add('hidden');
        } else {
          document.getElementById('passenger-section').classList.remove('hidden');
          document.getElementById('driver-section').classList.add('hidden');
        }

        initMap();
      } else {
        currentUser = null;
        currentUserProfile = null;
        document.getElementById('auth-section').classList.remove('hidden');
        document.getElementById('app-section').classList.add('hidden');
        document.getElementById('logout-btn').classList.add('hidden');
      }
    });

    // --- MAP & RIDE SYSTEM ---
    let map = null;
    let pickupMarker = null;
    let dropoffMarker = null;
    let routePolyline = null;
    let clickState = 'pickup';

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
          routePolyline = L.polyline([pickupMarker.getLatLng(), dropoffMarker.getLatLng()], { color: '#2563eb', weight: 4 }).addTo(map);
          map.fitBounds(routePolyline.getBounds(), { padding: [20, 20] });
        }
      });
    }

    // Passenger Ride Submit
    const rideForm = document.getElementById('ride-form');
    rideForm.addEventListener('submit', (e) => {
      e.preventDefault();

      const pickup = document.getElementById('pickup').value;
      const dropoff = document.getElementById('dropoff').value;
      const fare = document.getElementById('fare').value;

      const ridesRef = ref(db, 'rides/');
      const newRideRef = push(ridesRef);

      set(newRideRef, {
        passengerId: currentUser.uid,
        passengerName: currentUserProfile.name || 'Passenger',
        phone: currentUserProfile.phone || '03000000000',
        pickup,
        dropoff,
        fare: Number(fare),
        status: 'pending',
        createdAt: Date.now()
      }).then(() => {
        alert('Ride Broadcasted successfully!');
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
          statusDiv.innerHTML = `<div class="ride-card">⏳ Waiting for drivers to bid...</div>`;
        } else if (ride.bids) {
          let html = `<h4>Drivers Offered Bids:</h4>`;
          Object.keys(ride.bids).forEach(bidId => {
            const bid = ride.bids[bidId];
            html += `
              <div class="ride-card">
                <div>Driver: <strong>${bid.driverName} (${bid.vehicleType || 'Vehicle'})</strong></div>
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

    // Driver Live Rides Monitor
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
            <h4>Passenger: ${ride.passengerName}</h4>
            <div class="ride-info">Route: <strong>${ride.pickup} ➔ ${ride.dropoff}</strong></div>
            <div class="ride-info">Offered Fare: <strong>PKR ${ride.fare}</strong></div>
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
        driverPhone: currentUserProfile.phone || '03000000000',
        vehicleType: currentUserProfile.vehicleType || 'Taxi',
        amount: Number(price)
      }).then(() => {
        alert('Bid offer sent to passenger!');
      });
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
