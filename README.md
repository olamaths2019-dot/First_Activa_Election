<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>FIRST ACTIVA COLLEGE – Election</title>

<style>
body {
    font-family: Arial, sans-serif;
    background: #f2f5f9;
    padding: 20px;
}

.header {
    text-align: center;
}

.header img {
    width: 120px;
    margin-bottom: 10px;
}

h1 {
    color: #7a1f1f;
}

.post {
    background: white;
    padding: 15px;
    margin-bottom: 15px;
    border-radius: 6px;
    border: 1px solid #ddd;
}

button {
    width: 100%;
    padding: 12px;
    background: #7a1f1f;
    color: white;
    font-size: 16px;
    border: none;
    border-radius: 6px;
    cursor: pointer;
}

button:hover {
    background: #a93232;
}

.results {
    margin-top: 30px;
    background: #fff;
    padding: 20px;
    border-radius: 6px;
}

.winner {
    color: green;
    font-weight: bold;
}
</style>
</head>

<body>

<div class="header">
    <img src="logo.png" alt="First Activa College Logo">
    <h1>FIRST ACTIVA COLLEGE</h1>
    <h3>STUDENT ELECTION VOTING</h3>
</div>

<form id="voteForm">

<!-- FUNCTION TO CREATE POSTS -->
<script>
const posts = [
 "Senior Boy","Assistant Senior Boy","Senior Girl","Assistant Senior Girl",
 "SRC Member 1","SRC Member 2","SRC Member 3",
 "Time Keeper","Assistant Time Keeper",
 "Library Prefect","Assistant Library Prefect",
 "Assembly Prefect 1","Assembly Prefect 2",
 "Health Prefect (Boy)","Health Prefect (Girl)",
 "Social Prefect (Boy)","Social Prefect (Girl)"
];

document.write(posts.map(post => `
<div class="post">
<h3>${post}</h3>
<label><input type="radio" name="${post}" value="Candidate A" required> Candidate A</label><br>
<label><input type="radio" name="${post}" value="Candidate B"> Candidate B</label>
</div>
`).join(""));
</script>

<button type="submit">Submit Vote</button>
</form>

<div class="results">
<h2>Live Results & Winners</h2>
<div id="results"></div>
</div>

<script>
if (localStorage.getItem("hasVoted")) {
    alert("You have already voted. Multiple voting is not allowed.");
    document.getElementById("voteForm").style.display = "none";
}

const voteCounts = JSON.parse(localStorage.getItem("voteCounts")) || {};

document.getElementById("voteForm").addEventListener("submit", function(e) {
    e.preventDefault();

    const data = new FormData(this);
    for (let [post, candidate] of data.entries()) {
        voteCounts[post] = voteCounts[post] || {};
        voteCounts[post][candidate] = (voteCounts[post][candidate] || 0) + 1;
    }

    localStorage.setItem("voteCounts", JSON.stringify(voteCounts));
    localStorage.setItem("hasVoted", "true");

    this.reset();
    this.style.display = "none";
    displayResults();
});

function displayResults() {
    const resultsDiv = document.getElementById("results");
    resultsDiv.innerHTML = "";

    for (let post in voteCounts) {
        let maxVotes = 0;
        let winner = "";

        for (let candidate in voteCounts[post]) {
            if (voteCounts[post][candidate] > maxVotes) {
                maxVotes = voteCounts[post][candidate];
                winner = candidate;
            }
        }

        resultsDiv.innerHTML += `
            <h4>${post}</h4>
            <p>Candidate A: ${voteCounts[post]["Candidate A"] || 0} vote(s)</p>
            <p>Candidate B: ${voteCounts[post]["Candidate B"] || 0} vote(s)</p>
            <p class="winner">Winner: ${winner}</p>
            <hr>
        `;
    }
}

displayResults();
</script>

</body>
</html>
