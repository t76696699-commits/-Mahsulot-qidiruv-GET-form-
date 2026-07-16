1. HTML (index.html)
HTML
<div class="player">
    <img id="cover" src="default.jpg" alt="Cover">
    <h2 id="title">Qo'shiq Nomi</h2>
    
    <audio id="audio"></audio>

    <div class="progress-container" id="progressContainer">
        <div class="progress" id="progress"></div>
    </div>

    <div class="controls">
        <button id="prevBtn">⏮</button>
        <button id="playBtn">▶️</button>
        <button id="nextBtn">⏭</button>
        <button id="shuffleBtn">🔀</button>
        <button id="repeatBtn">🔁</button>
    </div>

    <input type="range" id="volumeSlider" min="0" max="1" step="0.1" value="1">
</div>
2. CSS (style.css)
CSS
.player { width: 300px; padding: 20px; border: 1px solid #ccc; text-align: center; }
.progress-container { width: 100%; height: 10px; background: #eee; cursor: pointer; margin: 10px 0; }
.progress { width: 0%; height: 100%; background: #007bff; }
#cover { width: 100%; height: 200px; object-fit: cover; }
3. JavaScript (script.js)
JavaScript
const audio = document.getElementById('audio');
const playBtn = document.getElementById('playBtn');
const progress = document.getElementById('progress');
const progressContainer = document.getElementById('progressContainer');
const volumeSlider = document.getElementById('volumeSlider');

let songs = [
    { title: "Song 1", src: "song1.mp3", cover: "cover1.jpg" },
    { title: "Song 2", src: "song2.mp3", cover: "cover2.jpg" }
];
let index = 0;
let isShuffle = false;
let isRepeat = false;

// Play/Pause
playBtn.addEventListener('click', () => {
    if (audio.paused) {
        audio.play();
        playBtn.innerText = "⏸";
    } else {
        audio.pause();
        playBtn.innerText = "▶️";
    }
});

// Progress Bar
audio.addEventListener('timeupdate', () => {
    const percent = (audio.currentTime / audio.duration) * 100;
    progress.style.width = percent + "%";
});

progressContainer.addEventListener('click', (e) => {
    const width = progressContainer.clientWidth;
    const clickX = e.offsetX;
    audio.currentTime = (clickX / width) * audio.duration;
});

// Volume
volumeSlider.addEventListener('input', (e) => {
    audio.volume = e.target.value;
});

// Navigatsiya
function loadSong(song) {
    audio.src = song.src;
    document.getElementById('title').innerText = song.title;
    document.getElementById('cover').src = song.cover;
}

document.getElementById('nextBtn').addEventListener('click', () => {
    index = isShuffle ? Math.floor(Math.random() * songs.length) : (index + 1) % songs.length;
    loadSong(songs[index]);
    audio.play();
});

// Repeat
audio.addEventListener('ended', () => {
    if (isRepeat) {
        audio.play();
    } else {
        document.getElementById('nextBtn').click();
    }
});

// Shuffle/Repeat toggles
document.getElementById('shuffleBtn').onclick = () => isShuffle = !isShuffle;
document.getElementById('repeatBtn').onclick = () => isRepeat = !isRepeat;

loadSong(songs[index]);
