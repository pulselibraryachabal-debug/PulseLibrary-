<!DOCTYPE html>
<html>
<head>
    <title>Pulse Library</title>

    <!-- QR Code Library -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrious/4.0.2/qrious.min.js"></script>

    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 20px;
            background: #fafafa;
            color: #333;
        }

        .box {
            background: white;
            padding: 25px;
            border-radius: 12px;
            max-width: 900px;
            margin: auto;
            box-shadow: 0 0 12px rgba(0,0,0,0.08);
        }

        h2 { color: #005bff; }

        button {
            padding: 10px 20px;
            border: none;
            background: #005bff;
            color: white;
            border-radius: 6px;
            cursor: pointer;
            margin: 5px;
        }

        button:hover { background: #0041c4; }

        .seat {
            padding: 8px;
            border: 1px solid #ddd;
            margin: 4px;
            border-radius: 5px;
            display: inline-block;
            width: 50px;
            text-align: center;
            font-size: 12px;
        }

        .vacant { background: #ccffcc; cursor: pointer; }
        .occupied { background: #ffb3b3; cursor: not-allowed; }
        .pending { background: #ffe6a3; cursor: not-allowed; }

        input {
            width: 100%;
            padding: 10px;
            margin: 6px 0;
            border-radius: 6px;
            border: 1px solid #ccc;
        }

        .back-btn { background: #555; }

        /* Receipt popup */
        #receiptPopup {
            display:none;
            position:fixed;
            top:0; left:0;
            width:100%; height:100%;
            background:rgba(0,0,0,0.6);
            padding-top:40px;
            z-index:9999;
        }
        #receiptBox {
            background:white;
            width:350px;
            margin:auto;
            padding:20px;
            border-radius:12px;
            text-align:center;
        }
    </style>
</head>
<body>

<!-- ============== HOME PAGE ============== -->
<div class="box" id="homepage">

    <!-- Logo -->
    <div style="text-align:center;">
        <svg width="160" height="160" viewBox="0 0 200 200">
            <circle cx="100" cy="100" r="92" stroke="#333" stroke-width="4" fill="white"/>
            <path d="M60 70 C45 65, 45 95, 60 90 L95 112 L95 50 Z" fill="#393939"/>
            <path d="M140 70 C155 65, 155 95, 140 90 L105 112 L105 50 Z" fill="#393939"/>
            <line x1="100" y1="48" x2="100" y2="118" stroke="white" stroke-width="4" />
            <circle cx="55" cy="72" r="3" fill="#393939"/>
            <circle cx="48" cy="80" r="3" fill="#393939"/>
            <circle cx="53" cy="88" r="2.6" fill="#393939"/>
            <circle cx="44" cy="92" r="2.4" fill="#393939"/>
            <circle cx="58" cy="96" r="2.6" fill="#393939"/>
            <circle cx="145" cy="72" r="3" fill="#393939"/>
            <circle cx="152" cy="80" r="3" fill="#393939"/>
            <circle cx="147" cy="88" r="2.6" fill="#393939"/>
            <circle cx="156" cy="92" r="2.4" fill="#393939"/>
            <circle cx="142" cy="96" r="2.6" fill="#393939"/>
            <line x1="38" y1="122" x2="162" y2="122" stroke="#333" stroke-width="4"/>
            <text x="100" y="148" font-size="18" font-family="Arial Black" text-anchor="middle" fill="#333">
                PULSE LIBRARY
            </text>
            <text x="100" y="170" font-size="22" text-anchor="middle" fill="#333">★ ★ ★</text>
        </svg>
    </div>

    <h2>Pulse Library</h2>

    <p><b>Total Seats:</b> <span id="totalSeats"></span></p>
    <p><b>Occupied Seats:</b> <span id="occupiedSeats"></span></p>
    <p><b>Pending Approval:</b> <span id="pendingSeats"></span></p>
    <p><b>Vacant Seats:</b> <span id="vacantSeats"></span></p>

    <button onclick="goToAvailability()">Book Seat</button>
    <button onclick="openAdminLogin()">Admin Panel</button>

    <hr><br>

    <h3>About Pulse Library</h3>
    <p style="text-align:justify;">
        A Library That Unlocks Every Door to Knowledge, Inspires Every Dream, Shapes Bright Futures,
        Empowers Young Minds, Builds Confidence, Encourages Curiosity, Guides Learners Toward Excellence,
        Opens Pathways to Growth, Strengthens Imagination, Creates Leaders, Transforms Hard Work Into Achievement,
        Offers Wisdom at Every Step, Lights the Way to Possibilities, Supports Every Goal, Turns Questions Into Solutions,
        Nurtures Talent, Expands Horizons, Enriches Understanding, Connects Students With Opportunities, Fuels Determination,
        Plants the Seeds of Greatness, Develops Skills for Life, Encourages Independent Learning, Provides a Peaceful Place
        to Study, Motivates Readers to Aim Higher, Builds Strong Foundations for Success, Makes Learning Enjoyable, Cultivates
        Good Habits, Helps Students Discover Their Potential, Enhances Creativity, Boosts Academic Performance, Brings Knowledge
        Closer to Everyone, Promotes Discipline, Guides Ambition in the Right Direction, Inspires Students to Work Hard, Offers
        Unlimited Learning Resources, Becomes a Gateway to Dreams, Strengthens Focus, Sparks New Ideas, Teaches the Value of
        Education, Supports Students in Every Subject, Encourages Smart Study Practices, Turns Ordinary Learners Into Extraordinary
        Achievers, Stands as a Symbol of Growth and Progress, Shapes the Future Through Books, Builds a Community of Readers,
        Provides Tools for Personal Development, Encourages Lifelong Learning, Helps Students Stay Ahead, Makes Success Achievable
        for All, Brings Together Knowledge and Effort, Creates an Environment for Achievement, Cultivates Wisdom, Forms the Heart
        of Education, Encourages Critical Thinking, Offers Guidance Through Every Challenge, Helps Students Dream Bigger, Fills
        Minds With Knowledge That Leads to Success, Builds Confidence Through Learning, Strengthens Memory and Skills, Inspires
        Hard Work, Makes Students Believe They Can Achieve Anything, Provides Support for Every Goal, Encourages Reading as the
        First Step to Success, Turns Time Into Productivity, Gives Students a Place to Grow, Makes Every Learner Stronger, Helps
        Build a Better Tomorrow, Shows That Books Are the True Key to Success, and Ultimately Stands as a Powerful Source of
        Knowledge, Motivation, and Achievement.
    </p>

    <hr>
    <h3>Contact</h3>
    <p><b>+91 8803586121</b></p>
    <p><b>+91 9797033586</b></p>
</div>

<!-- ============== SEAT AVAILABILITY ============== -->
<div class="box" id="availability" style="display:none;">
    <h2>Select a Seat</h2>
    <div id="seatList"></div>
    <button class="back-btn" onclick="goHome()">Back</button>
</div>

<!-- ============== FORM PAGE ============== -->
<div class="box" id="formPage" style="display:none;">
    <h2>Seat Booking Form</h2>
    <p><b>Selected Seat:</b> <span id="selectedSeat"></span></p>

    <input id="nameInput" placeholder="Your Name">
    <input id="phoneInput" placeholder="Phone Number">
    <input id="emailInput" placeholder="Email">
    <input id="studentInput" placeholder="Student ID (Optional)">
    <input type="date" id="dateInput">
    <input id="timeInput" placeholder="10:00 AM – 12:00 PM">

    <button onclick="confirmBooking()">Submit Request</button>
    <button class="back-btn" onclick="goHome()">Cancel</button>
</div>

<!-- ============== SUCCESS PAGE ============== -->
<div class="box" id="successPage" style="display:none;">
    <h2>Request Submitted ✔</h2>
    <p>Your Seat: <b id="sSeat"></b></p>
    <p>Date: <b id="sDate"></b></p>
    <p>Time Slot: <b id="sTime"></b></p>

    <h3 style="color:blue;">Your request is pending admin approval.</h3>
    <h3 style="color:green;">After approval, pay fee at the fee counter.</h3>

    <button onclick="goHome()">Done</button>
</div>

<!-- ============== ADMIN LOGIN ============== -->
<div class="box" id="adminLogin" style="display:none;">
    <h2>Admin Login</h2>
    <input id="adminUser" placeholder="Admin Name">
    <input id="adminPass" placeholder="Password" type="password">
    <button onclick="adminLogin()">Login</button>
    <button class="back-btn" onclick="goHome()">Back</button>
</div>

<!-- ============== ADMIN PANEL ============== -->
<div class="box" id="adminPanel" style="display:none;">

    <!-- Logo in admin panel -->
    <div style="text-align:center;">
        <svg width="130" height="130" viewBox="0 0 200 200">
            <circle cx="100" cy="100" r="92" stroke="#333" stroke-width="4" fill="white"/>
            <path d="M60 70 C45 65, 45 95, 60 90 L95 112 L95 50 Z" fill="#393939"/>
            <path d="M140 70 C155 65, 155 95, 140 90 L105 112 L105 50 Z" fill="#393939"/>
            <line x1="100" y1="48" x2="100" y2="118" stroke="white" stroke-width="4" />
            <circle cx="55" cy="72" r="3" fill="#393939"/>
            <circle cx="48" cy="80" r="3" fill="#393939"/>
            <circle cx="53" cy="88" r="2.6" fill="#393939"/>
            <circle cx="44" cy="92" r="2.4" fill="#393939"/>
            <circle cx="58" cy="96" r="2.6" fill="#393939"/>
            <circle cx="145" cy="72" r="3" fill="#393939"/>
            <circle cx="152" cy="80" r="3" fill="#393939"/>
            <circle cx="147" cy="88" r="2.6" fill="#393939"/>
            <circle cx="156" cy="92" r="2.4" fill="#393939"/>
            <circle cx="142" cy="96" r="2.6" fill="#393939"/>
            <line x1="38" y1="122" x2="162" y2="122" stroke="#333" stroke-width="4"/>
            <text x="100" y="148" font-size="18" font-family="Arial Black" text-anchor="middle" fill="#333">
                PULSE LIBRARY
            </text>
            <text x="100" y="170" font-size="22" text-anchor="middle" fill="#333">★ ★ ★</text>
        </svg>
    </div>

    <h2>Admin Panel</h2>
    <button onclick="showPending()">Pending Requests</button>
    <button onclick="showBookings()">All Bookings</button>
    <button onclick="showSeatControl()">Manage Seats</button>
    <button class="back-btn" onclick="logoutAdmin()">Logout</button>
</div>

<!-- ============== PENDING REQUESTS ============== -->
<div class="box" id="pendingPage" style="display:none;">
    <h2>Pending Bookings</h2>
    <div id="pendingList"></div>
    <button class="back-btn" onclick="openAdminPanel()">Back</button>
</div>

<!-- ============== ALL BOOKINGS ============== -->
<div class="box" id="bookingPage" style="display:none;">
    <h2>All Bookings</h2>
    <div id="bookingListDiv"></div>
    <button class="back-btn" onclick="openAdminPanel()">Back</button>
</div>

<!-- ============== SEAT CONTROL ============== -->
<div class="box" id="seatControlPage" style="display:none;">
    <h2>Seat Management</h2>
    <div id="seatControlList"></div>

    <h3>Add New Seat</h3>
    <input id="newSeatId" placeholder="Seat ID (e.g., S126)">
    <button onclick="addSeat()">Add Seat</button>

    <button class="back-btn" onclick="openAdminPanel()">Back</button>
</div>

<!-- ============== RECEIPT POPUP ============== -->
<div id="receiptPopup">
    <div id="receiptBox">

        <!-- Logo on receipt -->
        <svg width="120" height="120" viewBox="0 0 200 200">
            <circle cx="100" cy="100" r="92" stroke="#333" stroke-width="4" fill="white"/>
            <path d="M60 70 C45 65, 45 95, 60 90 L95 112 L95 50 Z" fill="#393939"/>
            <path d="M140 70 C155 65, 155 95, 140 90 L105 112 L105 50 Z" fill="#393939"/>
            <line x1="100" y1="48" x2="100" y2="118" stroke="white" stroke-width="4" />
            <circle cx="55" cy="72" r="3" fill="#393939"/>
            <circle cx="48" cy="80" r="3" fill="#393939"/>
            <circle cx="53" cy="88" r="2.6" fill="#393939"/>
            <circle cx="44" cy="92" r="2.4" fill="#393939"/>
            <circle cx="58" cy="96" r="2.6" fill="#393939"/>
            <circle cx="145" cy="72" r="3" fill="#393939"/>
            <circle cx="152" cy="80" r="3" fill="#393939"/>
            <circle cx="147" cy="88" r="2.6" fill="#393939"/>
            <circle cx="156" cy="92" r="2.4" fill="#393939"/>
            <circle cx="142" cy="96" r="2.6" fill="#393939"/>
            <line x1="38" y1="122" x2="162" y2="122" stroke="#333" stroke-width="4"/>
            <text x="100" y="148" font-size="18" text-anchor="middle" fill="#333">
                PULSE LIBRARY
            </text>
            <text x="100" y="170" font-size="22" text-anchor="middle" fill="#333">★ ★ ★</text>
        </svg>

        <h3>Booking Receipt</h3>

        <p><b>Name:</b> <span id="rName"></span></p>
        <p><b>Seat:</b> <span id="rSeat"></span></p>
        <p><b>Date:</b> <span id="rDate"></span></p>
        <p><b>Time:</b> <span id="rTime"></span></p>
        <p><b>Status:</b> Approved</p>

        <p style="color:green; font-weight:bold;">
            Your request has been successfully approved. You will be called soon.
        </p>

        <canvas id="qrCode"></canvas>

        <br><br>
        <button onclick="window.print()">Print</button>
        <button class="back-btn" type="button" onclick="closeReceipt()">Close</button>
    </div>
</div>

<script>
/* ======== DATA STORAGE ======== */
let seats = JSON.parse(localStorage.getItem("seats")) || [];
let bookings = JSON.parse(localStorage.getItem("bookings")) || [];

// Initialize seats if empty (125 seats)
if (seats.length === 0) {
    for (let i = 1; i <= 125; i++) {
        seats.push({ id: "S" + i, status: "vacant" });
    }
    saveSeats();
}

function saveSeats() {
    localStorage.setItem("seats", JSON.stringify(seats));
}

function saveBookings() {
    localStorage.setItem("bookings", JSON.stringify(bookings));
}

function updateStats() {
    document.getElementById("totalSeats").innerText = seats.length;
    document.getElementById("occupiedSeats").innerText = seats.filter(s => s.status === "occupied").length;
    document.getElementById("pendingSeats").innerText = seats.filter(s => s.status === "pending").length;
    document.getElementById("vacantSeats").innerText = seats.filter(s => s.status === "vacant").length;
}
updateStats();

/* ======== VIEW HELPERS ======== */
function hideAllBoxes() {
    document.querySelectorAll(".box").forEach(b => b.style.display = "none");
}

function goHome() {
    hideAllBoxes();
    document.getElementById("homepage").style.display = "block";
    updateStats();
}

/* ======== USER BOOKING FLOW ======== */
let chosenSeat = null;

function goToAvailability() {
    hideAllBoxes();
    document.getElementById("availability").style.display = "block";

    const div = document.getElementById("seatList");
    div.innerHTML = "";

    seats.forEach(seat => {
        const box = document.createElement("div");
        box.innerText = seat.id;
        box.className = "seat " + seat.status;

        if (seat.status === "vacant") {
            box.onclick = () => selectSeat(seat.id);
        }

        div.appendChild(box);
    });
}

function selectSeat(seatId) {
    chosenSeat = seatId;
    hideAllBoxes();
    document.getElementById("formPage").style.display = "block";
    document.getElementById("selectedSeat").innerText = seatId;
}

function confirmBooking() {
    const name = document.getElementById("nameInput").value.trim();
    const phone = document.getElementById("phoneInput").value.trim();
    const email = document.getElementById("emailInput").value.trim();
    const student = document.getElementById("studentInput").value.trim();
    const date = document.getElementById("dateInput").value;
    const time = document.getElementById("timeInput").value.trim();

    if (!name || !phone || !email || !date || !time || !chosenSeat) {
        alert("Please fill all required fields and select a seat.");
        return;
    }

    const seat = seats.find(s => s.id === chosenSeat);
    if (!seat || seat.status !== "vacant") {
        alert("This seat is no longer available. Please choose another seat.");
        return;
    }

    seat.status = "pending";
    saveSeats();

    const booking = {
        id: Date.now(),
        name,
        phone,
        email,
        student,
        date,
        time,
        seat: chosenSeat,
        status: "pending"
    };
    bookings.push(booking);
    saveBookings();

    document.getElementById("sSeat").innerText = chosenSeat;
    document.getElementById("sDate").innerText = date;
    document.getElementById("sTime").innerText = time;

    hideAllBoxes();
    document.getElementById("successPage").style.display = "block";
    updateStats();
}

/* ======== ADMIN LOGIN & PANEL ======== */
function openAdminLogin() {
    hideAllBoxes();
    document.getElementById("adminLogin").style.display = "block";
}

function adminLogin() {
    const user = document.getElementById("adminUser").value.trim();
    const pass = document.getElementById("adminPass").value.trim();

    if (user === "PL" && pass === "192201") {
        openAdminPanel();
    } else {
        alert("Incorrect admin name or password.");
    }
}

function openAdminPanel() {
    hideAllBoxes();
    document.getElementById("adminPanel").style.display = "block";
    updateStats();
}

function logoutAdmin() {
    goHome();
}

/* ======== ADMIN: PENDING REQUESTS ======== */
function showPending() {
    hideAllBoxes();
    document.getElementById("pendingPage").style.display = "block";

    const div = document.getElementById("pendingList");
    div.innerHTML = "";

    const pendingBookings = bookings.filter(b => b.status === "pending");

    if (pendingBookings.length === 0) {
        div.innerHTML = "<p>No pending requests.</p>";
        return;
    }

    pendingBookings.forEach(b => {
        const item = document.createElement("div");
        item.style.padding = "10px";
        item.style.border = "1px solid #ccc";
        item.style.margin = "10px 0";

        item.innerHTML = `
            <b>${b.name}</b><br>
            Seat: ${b.seat}<br>
            Date: ${b.date}<br>
            Time: ${b.time}<br>
            Phone: ${b.phone}<br>
            Status: ${b.status.toUpperCase()}<br><br>
            <button onclick="approveBooking(${b.id})">Approve</button>
            <button onclick="rejectBooking(${b.id})" style="background:#d00;">Reject</button>
        `;

        div.appendChild(item);
    });
}

function approveBooking(bookingId) {
    const b = bookings.find(x => x.id === bookingId);
    if (!b) return;

    const seat = seats.find(s => s.id === b.seat);
    if (!seat) return;

    b.status = "approved";
    seat.status = "occupied";

    saveBookings();
    saveSeats();
    updateStats();

    alert("Your request has been successfully approved. You will be called soon.");

    showReceipt(b);
    showPending();
}

function rejectBooking(bookingId) {
    const b = bookings.find(x => x.id === bookingId);
    if (!b) return;

    const seat = seats.find(s => s.id === b.seat);
    if (seat) seat.status = "vacant";

    b.status = "rejected";

    saveBookings();
    saveSeats();
    updateStats();

    showPending();
}

/* ======== ADMIN: ALL BOOKINGS ======== */
function showBookings() {
    hideAllBoxes();
    document.getElementById("bookingPage").style.display = "block";

    const div = document.getElementById("bookingListDiv");
    div.innerHTML = "";

    if (bookings.length === 0) {
        div.innerHTML = "<p>No bookings yet.</p>";
        return;
    }

    bookings.forEach(b => {
        const item = document.createElement("div");
        item.style.padding = "10px";
        item.style.border = "1px solid #ddd";
        item.style.margin = "10px 0";

        item.innerHTML = `
            <b>${b.name}</b><br>
            Seat: ${b.seat}<br>
            Status: ${b.status.toUpperCase()}<br>
            Date: ${b.date}<br>
            Time: ${b.time}<br>
            Phone: ${b.phone}<br>
            Email: ${b.email}<br>
        `;
        div.appendChild(item);
    });
}

/* ======== ADMIN: SEAT CONTROL (MANUAL) ======== */
function showSeatControl() {
    hideAllBoxes();
    document.getElementById("seatControlPage").style.display = "block";

    const div = document.getElementById("seatControlList");
    div.innerHTML = "";

    seats.forEach(seat => {
        const item = document.createElement("div");
        item.style.padding = "6px 0";
        item.style.borderBottom = "1px solid #eee";

        item.innerHTML = `
            <b>${seat.id}</b> → ${seat.status.toUpperCase()}
            <button onclick="setSeatStatus('${seat.id}','vacant')">Vacant</button>
            <button onclick="setSeatStatus('${seat.id}','occupied')">Occupied</button>
            <button onclick="setSeatStatus('${seat.id}','pending')">Pending</button>
            <button onclick="removeSeat('${seat.id}')" style="background:#d00;">Remove</button>
        `;
        div.appendChild(item);
    });
}

function setSeatStatus(seatId, status) {
    const seat = seats.find(s => s.id === seatId);
    if (!seat) return;
    seat.status = status;
    saveSeats();
    updateStats();
    showSeatControl();
}

function addSeat() {
    const id = document.getElementById("newSeatId").value.trim();
    if (!id) {
        alert("Enter a seat ID.");
        return;
    }
    if (seats.find(s => s.id === id)) {
        alert("Seat with this ID already exists.");
        return;
    }
    seats.push({ id, status: "vacant" });
    document.getElementById("newSeatId").value = "";
    saveSeats();
    updateStats();
    showSeatControl();
}

function removeSeat(seatId) {
    seats = seats.filter(s => s.id !== seatId);
    saveSeats();
    updateStats();
    showSeatControl();
}

/* ======== RECEIPT + QR CODE ======== */
function showReceipt(booking) {
    document.getElementById("rName").innerText = booking.name;
    document.getElementById("rSeat").innerText = booking.seat;
    document.getElementById("rDate").innerText = booking.date;
    document.getElementById("rTime").innerText = booking.time;

    const qrCanvas = document.getElementById("qrCode");
    new QRious({
        element: qrCanvas,
        size: 180,
        value: `Pulse Library Booking
Name: ${booking.name}
Seat: ${booking.seat}
Date: ${booking.date}
Time: ${booking.time}
Status: APPROVED`
    });

    document.getElementById("receiptPopup").style.display = "block";
}

function closeReceipt() {
    document.getElementById("receiptPopup").style.display = "none";
}

/* ======== INIT ======== */
goHome();
</script>

</body>
</html>
