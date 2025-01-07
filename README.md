<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Music Player</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="player">
        <h1>Music Player</h1>
        <audio id="audio" controls>
            <source id="audioSource" src="" type="audio/mpeg">
            Your browser does not support the audio element.
        </audio>
        <div class="controls">
            <button id="prev">Previous</button>
            <button id="next">Next</button>
        </div>
    </div>

    <script src="scripts.js"></script>
</body>
</html>

body {
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    background-color: #f0f0f0;
}

.player {
    text-align: center;
    background: #fff;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.controls {
    margin-top: 20px;
}

button {
    padding: 10px 20px;
    margin: 0 10px;
    border: none;
    background: #007bff;
    color: #fff;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background: #0056b3;
}

const songs = [
    "song1.mp3",
    "song2.mp3",
    "song3.mp3"
];

let currentIndex = 0;
const audio = document.getElementById("audio");
const audioSource = document.getElementById("audioSource");

function loadSong(index) {
    audioSource.src = `http://localhost:3000/music/${songs[index]}`;
    audio.load();
    audio.play();
}

document.getElementById("prev").addEventListener("click", () => {
    currentIndex = (currentIndex - 1 + songs.length) % songs.length;
    loadSong(currentIndex);
});

document.getElementById("next").addEventListener("click", () => {
    currentIndex = (currentIndex + 1) % songs.length;
    loadSong(currentIndex);
});

// Load the first song when the page loads
loadSong(currentIndex);
