<html lang="ur" dir="ltr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>GB Drive - Ultimate InDrive Clone</title>
  
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
    }

    header h1 {
      font-size: 1.5rem;
      font-weight: 700;
    }

    header p {
      font-size: 0.8rem;
      opacity: 0.9;
    }

    .role-switcher {
      display: flex;
      background: #e5e7eb;
      padding: 4px;
      margin: 12px;
      border-radius: 10px;
    }

    .role-btn {
      flex: 1;
      padding: 10px;
      border: none;
      background: transparent;
      font-weight: 600;
      color: #6b7280;
      border-radius: 8px;
      cursor: pointer;
      transition: all 0.2s;
    }

    .role-btn.active {
      background: white;
      color: var(--primary);
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }

    #map {
      height: 250px;
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

    .form-group input {
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

    .hidden { display: none; }
  </style>
</head>
<body>

  <div class="app-container">
    <header>
      <h1>GB Drive</h1>
      <p>Realtime Map, Fare Offer & Bidding System</p>
    </header>

    <!-- Role Switcher -->
    <div class="role-switcher">
      <button class="role-btn active" id="btn-passenger-view" onclick="switchRole('passenger')">Passenger</button>
      <button class="role-btn" id="btn-driver-view" onclick="switchRole('driver')">Driver</button>
    </div>

    <!-- Map Container -->
    <div id="map"></div>

    <div class="content">
      <!-- PASSENGER VIEW -->
      <div id="passenger-section">
        <div class="map-instruction">
          📍 Map par pehla click <b>Pickup</b> aur doosra click <b>Dropoff</b> set karega.
        </div>
        
        <form id="ride-form">
          <div class="form-group">
            <label>Pickup Location</label>
            <input type="text" id="pickup" placeholder="Map par click karein ya likhein" required>
          </div>
          <div class="form-group">
            <label>Dropoff Location</label>
            <input type="text" id="dropoff" placeholder="Map par click karein ya likhein" required>
          </div>
          <div class="form-group">
            <label>Passenger Phone Number (WhatsApp)</label>
            <input type="text" id="passenger-phone" placeholder="e.g. 03001234567" required>
          </div>
          <div class="form-group">
            <label>Your Offer Fare (PKR)</label>
            <input type="number" id="fare" placeholder="e.g. 1200" required>
          </div>
          <button type="submit" class="btn">Offer Ride Now</button>
        </form>

        <div id="passenger-ride-status" style="margin-top: 15px;"></div>
      </div>

      <!-- DRIVER VIEW -->
      <div id="driver-section" class="hidden">
        <h3>Available Live Rides</h3>
        <div id="rides-list" style="margin-top: 10px;">
          <p style="color: #6b7280; font-size: 0.85rem;">Searching for rides near Gilgit-Baltistan...</p>
        </div>
      </div>
    </div>
  </div>

  <!-- Leaflet JS -->
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

  <!-- FIREBASE SDKs -->
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
    import { getDatabase, ref, push, set, onValue, update, remove } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

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
    const db = getDatabase(app);

    // --- LEAFLET MAP SETUP ---
    // Gilgit City Coordinates
    const map = L.map('map').setView([35.9208, 74.3144], 12);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '© OpenStreetMap'
    }).addTo(map);

    let pickupMarker = null;
    let dropoffMarker = null;
    let routePolyline = null;
    let clickState = 'pickup';

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

      // Draw Route Line
      if (pickupMarker && dropoffMarker) {
        if (routePolyline) map.removeLayer(routePolyline);
        routePolyline = L.polyline([pickupMarker.getLatLng(), dropoffMarker.getLatLng()], { color: '#2563eb', weight: 4 }).addTo(map);
        map.fitBounds(routePolyline.getBounds(), { padding: [20, 20] });
      }
    });

    // --- RIDE SUBMISSION (PASSENGER) ---
    let currentRideId = null;
    const rideForm = document.getElementById('ride-form');

    rideForm.addEventListener('submit', (e) => {
      e.preventDefault();

      const pickup = document.getElementById('pickup').value;
      const dropoff = document.getElementById('dropoff').value;
      const phone = document.getElementById('passenger-phone').value;
      const fare = document.getElementById('fare').value;

      const ridesRef = ref(db, 'rides/');
      const newRideRef = push(ridesRef);
      currentRideId = newRideRef.key;

      set(newRideRef, {
        pickup,
        dropoff,
        phone,
        fare: Number(fare),
        status: 'pending',
        bids: {},
        createdAt: Date.now()
      }).then(() => {
        alert('Ride Request Broadcasted to Drivers!');
        listenToMyRide(currentRideId);
      });
    });

    // Passenger Listens to Driver Bids
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
                <div>Driver: <strong>${bid.driverName}</strong></div>
                <div>Bid Fare: <strong style="color: green;">PKR ${bid.amount}</strong></div>
                <div style="display:flex; gap:6px; margin-top:8px;">
                  <button class="btn btn-accent" onclick="window.acceptBid('${rideId}', '${bid.driverName}', '${bid.driverPhone}', ${bid.amount})">Accept Offer</button>
                </div>
              </div>
            `;
          });
          statusDiv.innerHTML = html;
        } else if (ride.status === 'accepted') {
          const waUrl = `https://wa.me/${ride.acceptedDriverPhone}?text=Hi%20${ride.acceptedDriverName},%20I%20accepted%20your%20GB%20Drive%20offer%20of%20PKR%20${ride.acceptedFare}%20from%20${ride.pickup}%20to%20${ride.dropoff}`;
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

    // --- DRIVER SIDE FUNCTIONALITY ---
    const ridesList = document.getElementById('rides-list');
    const ridesRef = ref(db, 'rides/');

    onValue(ridesRef, (snapshot) => {
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
            <h4>Route: ${ride.pickup} ➔ ${ride.dropoff}</h4>
            <div class="ride-info">Offered Fare: <strong>PKR ${ride.fare}</strong></div>
            <div class="bid-input-group">
              <input type="number" id="bid-price-${rideId}" placeholder="Counter Fare" value="${ride.fare}">
              <input type="text" id="driver-name-${rideId}" placeholder="Your Name" style="flex:1;">
            </div>
            <div class="bid-input-group">
              <input type="text" id="driver-phone-${rideId}" placeholder="WhatsApp No">
              <button class="btn btn-accent" onclick="window.sendDriverBid('${rideId}')">Send Bid</button>
            </div>
          `;
          ridesList.appendChild(card);
        }
      });
    });

    // Driver Counter-Bid Submit
    window.sendDriverBid = function(rideId) {
      const price = document.getElementById(`bid-price-${rideId}`).value;
      const driverName = document.getElementById(`driver-name-${rideId}`).value || 'Driver';
      const driverPhone = document.getElementById(`driver-phone-${rideId}`).value || '03000000000';

      const bidsRef = ref(db, `rides/${rideId}/bids`);
      const newBidRef = push(bidsRef);

      set(newBidRef, {
        driverName,
        driverPhone,
        amount: Number(price)
      }).then(() => {
        alert('Bid offer sent to passenger!');
      });
    };

    // Passenger Accepts Bid
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

  <script>
    // Role Switcher Toggle
    function switchRole(role) {
      const passengerSec = document.getElementById('passenger-section');
      const driverSec = document.getElementById('driver-section');
      const btnPassenger = document.getElementById('btn-passenger-view');
      const btnDriver = document.getElementById('btn-driver-view');

      if (role === 'passenger') {
        passengerSec.classList.remove('hidden');
        driverSec.classList.add('hidden');
        btnPassenger.classList.add('active');
        btnDriver.classList.remove('active');
      } else {
        passengerSec.classList.add('hidden');
        driverSec.classList.remove('hidden');
        btnDriver.classList.add('active');
        btnPassenger.classList.remove('active');
      }
    }
  </script>
</body>
</html>
