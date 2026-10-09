<html lang="ur" dir="ltr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>GB Drive - Gilgit-Baltistan Ride Sharing</title>
  <style>
    :root {
      --primary: #2563eb;
      --primary-dark: #1d4ed8;
      --accent: #10b981;
      --bg: #f3f4f6;
      --card-bg: #ffffff;
      --text: #1f2937;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
    }

    body {
      background-color: var(--bg);
      color: var(--text);
      display: flex;
      flex-direction: column;
      align-items: center;
      min-height: 100vh;
      padding: 15px;
    }

    .app-container {
      width: 100%;
      max-width: 480px;
      background: var(--card-bg);
      border-radius: 16px;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.08);
      overflow: hidden;
      margin-top: 10px;
    }

    header {
      background: var(--primary);
      color: white;
      padding: 20px;
      text-align: center;
    }

    header h1 {
      font-size: 1.6rem;
      font-weight: 700;
    }

    header p {
      font-size: 0.85rem;
      opacity: 0.9;
      margin-top: 4px;
    }

    .role-switcher {
      display: flex;
      background: #e5e7eb;
      padding: 4px;
      margin: 15px;
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

    .content {
      padding: 20px;
    }

    .form-group {
      margin-bottom: 15px;
    }

    .form-group label {
      display: block;
      font-size: 0.85rem;
      font-weight: 600;
      margin-bottom: 6px;
      color: #4b5563;
    }

    .form-group input, .form-group select {
      width: 100%;
      padding: 12px 14px;
      border: 1px solid #d1d5db;
      border-radius: 8px;
      font-size: 0.95rem;
      outline: none;
      transition: border-color 0.2s;
    }

    .form-group input:focus {
      border-color: var(--primary);
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

    .btn:hover {
      background: var(--primary-dark);
    }

    .btn-accent {
      background: var(--accent);
    }

    .btn-accent:hover {
      background: #059669;
    }

    .ride-card {
      border: 1px solid #e5e7eb;
      border-radius: 12px;
      padding: 15px;
      margin-bottom: 12px;
      background: #fafafa;
    }

    .ride-card h4 {
      color: var(--primary);
      margin-bottom: 8px;
    }

    .ride-info {
      font-size: 0.9rem;
      margin-bottom: 4px;
    }

    .bid-input-group {
      display: flex;
      gap: 8px;
      margin-top: 10px;
    }

    .bid-input-group input {
      flex: 1;
      padding: 8px;
      border: 1px solid #d1d5db;
      border-radius: 6px;
    }

    .hidden {
      display: none;
    }
  </style>
</head>
<body>

  <div class="app-container">
    <header>
      <h1>GB Drive</h1>
      <p>Gilgit-Baltistan Local Ride & Bid Service</p>
    </header>

    <!-- Role Switcher -->
    <div class="role-switcher">
      <button class="role-btn active" id="btn-passenger-view" onclick="switchRole('passenger')">Passenger</button>
      <button class="role-btn" id="btn-driver-view" onclick="switchRole('driver')">Driver</button>
    </div>

    <div class="content">
      <!-- PASSENGER VIEW -->
      <div id="passenger-section">
        <h3>Book a Ride</h3>
        <form id="ride-form" style="margin-top: 15px;">
          <div class="form-group">
            <label>Pickup Location</label>
            <input type="text" id="pickup" placeholder="e.g. Gilgit Airport / Skardu Bazaar" required>
          </div>
          <div class="form-group">
            <label>Dropoff Location</label>
            <input type="text" id="dropoff" placeholder="e.g. Hunza / Astore Main Bazaar" required>
          </div>
          <div class="form-group">
            <label>Your Offer Price (PKR)</label>
            <input type="number" id="fare" placeholder="e.g. 1500" required>
          </div>
          <button type="submit" class="btn">Offer Ride</button>
        </form>

        <div id="passenger-status" style="margin-top: 20px;"></div>
      </div>

      <!-- DRIVER VIEW -->
      <div id="driver-section" class="hidden">
        <h3>Available Rides</h3>
        <div id="rides-list" style="margin-top: 15px;">
          <p style="color: #6b7280; font-size: 0.9rem;">No active ride requests...</p>
        </div>
      </div>
    </div>
  </div>

  <!-- FIREBASE SDKs -->
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-app.js";
    import { getDatabase, ref, push, set, onValue, update } from "https://www.gstatic.com/firebasejs/10.8.0/firebase-database.js";

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

    // Initialize Firebase
    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);

    // Ride Request Submit (Passenger)
    const rideForm = document.getElementById('ride-form');
    rideForm.addEventListener('submit', (e) => {
      e.preventDefault();

      const pickup = document.getElementById('pickup').value;
      const dropoff = document.getElementById('dropoff').value;
      const fare = document.getElementById('fare').value;

      const ridesRef = ref(db, 'rides/');
      const newRideRef = push(ridesRef);

      set(newRideRef, {
        pickup,
        dropoff,
        fare: Number(fare),
        status: 'pending',
        createdAt: Date.now()
      }).then(() => {
        alert('Ride request successfully posted!');
        rideForm.reset();
      }).catch(err => {
        alert('Error: ' + err.message);
      });
    });

    // Realtime Rides Monitoring (Driver)
    const ridesList = document.getElementById('rides-list');
    const ridesRef = ref(db, 'rides/');

    onValue(ridesRef, (snapshot) => {
      const data = snapshot.val();
      ridesList.innerHTML = '';

      if (!data) {
        ridesList.innerHTML = '<p style="color: #6b7280; font-size: 0.9rem;">No active ride requests...</p>';
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
              <input type="number" id="counter-${rideId}" placeholder="Counter Fare (PKR)" value="${ride.fare}">
              <button class="btn btn-accent" onclick="window.sendCounterBid('${rideId}')">Send Bid</button>
            </div>
          `;
          ridesList.appendChild(card);
        }
      });
    });

    // Send Counter Offer / Accept Ride
    window.sendCounterBid = function(rideId) {
      const bidInput = document.getElementById(`counter-${rideId}`);
      const bidPrice = bidInput.value;

      const rideRef = ref(db, `rides/${rideId}`);
      update(rideRef, {
        driverBid: Number(bidPrice),
        status: 'bid_offered'
      }).then(() => {
        alert('Offer sent to passenger!');
      });
    };
  </script>

  <script>
    // Toggle UI Role Views
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
