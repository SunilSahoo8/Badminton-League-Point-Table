<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Badminton Tournament Points Table</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f2f2f2;
      padding: 20px;
    }
    h1 {
      text-align: center;
      color: #333;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      background: white;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    th, td {
      padding: 12px;
      border: 1px solid #ccc;
      text-align: center;
    }
    th {
      background: #4CAF50;
      color: white;
    }
  </style>
</head>
<body>
  <h1>Badminton Tournament Points Table</h1>
  <table id="pointsTable">
    <thead>
      <tr>
        <th>Rank</th>
        <th>Team No.</th>
        <th>Team Name</th>
        <th>Matches Played</th>
        <th>Matches Won</th>
        <th>Matches Lost</th>
        <th>Matches Drawn</th>
        <th>Points</th>
        <th>Sets Won</th>
        <th>Sets Lost</th>
        <th>Points Scored</th>
        <th>Points Conceded</th>
      </tr>
    </thead>
    <tbody id="tableBody"></tbody>
  </table>

  <script>
    const teams = [
      { id: 1, name: "Lakra & Mahali" },
      { id: 2, name: "Ranjeet & Sanak" },
      { id: 3, name: "Lalit & Sipu" },
      { id: 4, name: "Tete & Dehuri" },
      { id: 5, name: "Majhi & Bikas" },
      { id: 6, name: "Surath & Mrutyunjay" },
      { id: 7, name: "Ashis & Sunil" },
    ];

    const matches = [
      { teamA: 1, teamB: 3, scores: [[19, 21], [18, 21]] },
      { teamA: 4, teamB: 5, scores: [[21, 16], [4, 21], [14, 21]] },
      { teamA: 6, teamB: 7, scores: [[8, 21], [21, 16], [13, 21]] }
    ];

    const stats = {};
    teams.forEach(team => {
      stats[team.id] = {
        ...team,
        played: 0,
        won: 0,
        lost: 0,
        drawn: 0,
        setsWon: 0,
        setsLost: 0,
        pointsScored: 0,
        pointsConceded: 0,
        points: 0,
        rank: '-'
      };
    });

    matches.forEach(({ teamA, teamB, scores }) => {
      let teamAWins = 0, teamBWins = 0;
      let teamAPoints = 0, teamBPoints = 0;

      scores.forEach(([a, b]) => {
        a > b ? teamAWins++ : teamBWins++;
        teamAPoints += a;
        teamBPoints += b;
      });

      const winner = teamAWins > teamBWins ? teamA : teamB;
      const loser = teamAWins > teamBWins ? teamB : teamA;

      stats[teamA].played++;
      stats[teamB].played++;

      stats[teamA].setsWon += teamAWins;
      stats[teamA].setsLost += teamBWins;
      stats[teamB].setsWon += teamBWins;
      stats[teamB].setsLost += teamAWins;

      stats[teamA].pointsScored += teamAPoints;
      stats[teamA].pointsConceded += teamBPoints;
      stats[teamB].pointsScored += teamBPoints;
      stats[teamB].pointsConceded += teamAPoints;

      stats[winner].won++;
      stats[loser].lost++;
      stats[winner].points += 2;
    });

    const sorted = Object.values(stats).sort((a, b) => {
      if (b.points !== a.points) return b.points - a.points;
      const aSetDiff = a.setsWon - a.setsLost;
      const bSetDiff = b.setsWon - b.setsLost;
      if (bSetDiff !== aSetDiff) return bSetDiff - aSetDiff;

      const aTieBreaker = a.won > 0 ? a.pointsConceded : -a.pointsScored;
      const bTieBreaker = b.won > 0 ? b.pointsConceded : -b.pointsScored;
      return aTieBreaker - bTieBreaker;
    });

    sorted.forEach((team, index) => {
      team.rank = team.played > 0 ? index + 1 : "-";
    });

    const tbody = document.getElementById("tableBody");
    sorted.forEach(team => {
      const row = document.createElement("tr");
      row.innerHTML = `
        <td>${team.rank}</td>
        <td>${team.id}</td>
        <td>${team.name}</td>
        <td>${team.played}</td>
        <td>${team.won}</td>
        <td>${team.lost}</td>
        <td>${team.drawn}</td>
        <td>${team.points}</td>
        <td>${team.setsWon}</td>
        <td>${team.setsLost}</td>
        <td>${team.pointsScored}</td>
        <td>${team.pointsConceded}</td>
      `;
      tbody.appendChild(row);
    });
  </script>
</body>
</html>