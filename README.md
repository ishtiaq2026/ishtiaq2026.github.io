<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Leaderboard - Top Earners</title>
<style>
  body {
    background: #121212;
    color: #eee;
    font-family: Arial, sans-serif;
    max-width: 420px;
    margin: 20px auto;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 0 15px #222;
  }
  h1 {
    text-align: center;
  }
  ul {
    list-style: none;
    padding: 0;
  }
  li {
    background: #222;
    margin: 8px 0;
    padding: 10px 15px;
    border-radius: 8px;
    font-size: 17px;
  }
  .back-btn {
    display: block;
    margin: 15px auto;
    background: #4caf50;
    color: white;
    border: none;
    padding: 10px 15px;
    border-radius: 8px;
    cursor: pointer;
    width: 90%;
  }
</style>
</head>
<body>
<h1>🏆 Leaderboard - Top Earners</h1>
<ul id="leaderboardList"></ul>
<button class="back-btn" onclick="window.location.href='index.html'">Back to App</button>

<script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-database.js"></script>

<script>
  // TODO: Same Firebase Config as in index.html
  const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT.firebaseapp.com",
    databaseURL: "https://YOUR_PROJECT.firebaseio.com",
    projectId: "YOUR_PROJECT",
    storageBucket: "YOUR_PROJECT.appspot.com",
    messagingSenderId: "YOUR_SENDER_ID",
    appId: "YOUR_APP_ID"
  };
  firebase.initializeApp(firebaseConfig);
  const db = firebase.database();

  const leaderboardList = document.getElementById('leaderboardList');

  function loadLeaderboard() {
    db.ref('users').orderByChild('earned').limitToLast(10).once('value', snapshot => {
      let users = [];
      snapshot.forEach(childSnap => {
        const data = childSnap.val();
        users.push({
          id: childSnap.key,
          earned: data.earned,
          watched: data.watched,
          lastUpdated: data.lastUpdated
        });
      });
      // Sort descending by earned
      users.sort((a, b) => b.earned - a.earned);

      leaderboardList.innerHTML = '';
      users.forEach((user, i) => {
        leaderboardList.innerHTML += `<li>🏅 ${i+1}. <b>${user.id}</b> - ৳${user.earned.toFixed(2)} (${user.watched} Ads)</li>`;
      });
    });
  }

  loadLeaderboard();
</script>
</body>
</html>
