<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Creative Corner</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: 'Georgia', serif;
            background: #f9f7f4;
            color: #2c2c2c;
            line-height: 1.6;
            padding: 20px;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
        }
        header {
            text-align: center;
            padding: 40px 20px;
            background: #2c3e50;
            color: white;
            border-radius: 10px;
            margin-bottom: 30px;
        }
        h1 {
            font-size: 2.5rem;
        }
        nav {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 15px;
        }
        nav button {
            background: #e67e22;
            color: white;
            border: none;
            padding: 10px 20px;
            cursor: pointer;
            border-radius: 5px;
            font-size: 1rem;
        }
        nav button:hover {
            background: #d35400;
        }
        .section {
            background: white;
            padding: 25px;
            margin-bottom: 30px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        .section h2 {
            color: #e67e22;
            margin-bottom: 15px;
            border-bottom: 2px solid #ecf0f1;
            padding-bottom: 10px;
        }
        .post {
            margin-bottom: 30px;
            padding: 15px;
            background: #fef9e8;
            border-left: 4px solid #e67e22;
        }
        .post h3 {
            color: #2c3e50;
            margin-bottom: 10px;
        }
        .post-date {
            color: #7f8c8d;
            font-size: 0.85rem;
            margin-bottom: 10px;
        }
        .word-card {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 30px;
            text-align: center;
            border-radius: 15px;
        }
        .word-card h3 {
            font-size: 2rem;
            margin-bottom: 10px;
        }
        input, textarea {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-family: inherit;
        }
        button {
            background: #e67e22;
            color: white;
            border: none;
            padding: 10px 20px;
            cursor: pointer;
            border-radius: 5px;
        }
        button:hover {
            background: #d35400;
        }
        .video-container {
            position: relative;
            padding-bottom: 56.25%;
            height: 0;
            overflow: hidden;
            margin: 15px 0;
        }
        .video-container iframe {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }
        footer {
            text-align: center;
            padding: 20px;
            color: #7f8c8d;
            margin-top: 40px;
        }
        @media (max-width: 600px) {
            body {
                padding: 10px;
            }
            .section {
                padding: 15px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>📖 My Creative Corner</h1>
            <p>Short Stories | Word of the Day | Lyrics & Videos</p>
            <nav>
                <button onclick="showSection('stories')">📚 Stories</button>
                <button onclick="showSection('word')">📝 Word of Day</button>
                <button onclick="showSection('music')">🎵 Music & Lyrics</button>
                <button onclick="showSection('videos')">🎬 Videos</button>
                <button onclick="showSection('admin')">⚙️ Admin Panel</button>
            </nav>
        </header>

        <!-- Stories Section -->
        <div id="stories" class="section">
            <h2>📚 Short Stories</h2>
            <div id="stories-list"></div>
        </div>

        <!-- Word of the Day Section -->
        <div id="word" class="section" style="display:none;">
            <h2>📝 Word of the Day</h2>
            <div id="word-card" class="word-card"></div>
        </div>

        <!-- Music & Lyrics Section -->
        <div id="music" class="section" style="display:none;">
            <h2>🎵 Music & Lyrics</h2>
            <div id="lyrics-list"></div>
        </div>

        <!-- Videos Section -->
        <div id="videos" class="section" style="display:none;">
            <h2>🎬 Videos</h2>
            <div id="videos-list"></div>
        </div>

        <!-- Admin Panel -->
        <div id="admin" class="section" style="display:none;">
            <h2>⚙️ Admin Panel - Add New Content</h2>
            
            <h3>Add Short Story</h3>
            <input type="text" id="story-title" placeholder="Story Title">
            <textarea id="story-content" rows="5" placeholder="Write your story here..."></textarea>
            <button onclick="addStory()">Publish Story</button>
            
            <h3 style="margin-top:20px;">Set Word of the Day</h3>
            <input type="text" id="word-title" placeholder="Word">
            <textarea id="word-definition" rows="3" placeholder="Definition and example..."></textarea>
            <button onclick="setWordOfDay()">Set Word of Day</button>
            
            <h3 style="margin-top:20px;">Add Lyrics</h3>
            <input type="text" id="song-title" placeholder="Song Title & Artist">
            <textarea id="lyrics-content" rows="5" placeholder="Lyrics..."></textarea>
            <button onclick="addLyrics()">Add Lyrics</button>
            
            <h3 style="margin-top:20px;">Add Video (YouTube)</h3>
            <input type="text" id="video-title" placeholder="Video Title">
            <input type="text" id="video-url" placeholder="YouTube URL (e.g., https://youtube.com/watch?v=...)">
            <button onclick="addVideo()">Add Video</button>
        </div>
        
        <footer>
            <p>✨ Created with love for sharing creativity | All content is original ✨</p>
        </footer>
    </div>

    <script>
        // Load saved data from localStorage
        let stories = JSON.parse(localStorage.getItem('stories')) || [];
        let wordOfDay = JSON.parse(localStorage.getItem('wordOfDay')) || { word: "Inspire", definition: "To fill someone with the urge to do something creative." };
        let lyrics = JSON.parse(localStorage.getItem('lyrics')) || [];
        let videos = JSON.parse(localStorage.getItem('videos')) || [];
        
        // Save functions
        function saveAll() {
            localStorage.setItem('stories', JSON.stringify(stories));
            localStorage.setItem('wordOfDay', JSON.stringify(wordOfDay));
            localStorage.setItem('lyrics', JSON.stringify(lyrics));
            localStorage.setItem('videos', JSON.stringify(videos));
        }
        
        // Display functions
        function displayStories() {
            const container = document.getElementById('stories-list');
            if(stories.length === 0) {
                container.innerHTML = '<p>No stories yet. Go to Admin Panel to add your first story!</p>';
                return;
            }
            container.innerHTML = stories.map((story, index) => `
                <div class="post">
                    <h3>${escapeHtml(story.title)}</h3>
                    <div class="post-date">${story.date}</div>
                    <p style="white-space: pre-wrap;">${escapeHtml(story.content)}</p>
                    <button onclick="deleteStory(${index})" style="background:#e74c3c; margin-top:10px;">Delete</button>
                </div>
            `).join('');
        }
        
        function displayWordOfDay() {
            const container = document.getElementById('word-card');
            container.innerHTML = `
                <h3>✨ ${escapeHtml(wordOfDay.word)} ✨</h3>
                <p>${escapeHtml(wordOfDay.definition)}</p>
                <small>Refresh daily for new inspiration!</small>
            `;
        }
        
        function displayLyrics() {
            const container = document.getElementById('lyrics-list');
            if(lyrics.length === 0) {
                container.innerHTML = '<p>No lyrics added yet.</p>';
                return;
            }
            container.innerHTML = lyrics.map((lyric, index) => `
                <div class="post">
                    <h3>🎵 ${escapeHtml(lyric.song)}</h3>
                    <div class="post-date">${lyric.date}</div>
                    <p style="white-space: pre-wrap;">${escapeHtml(lyric.lyrics)}</p>
                    <button onclick="deleteLyrics(${index})" style="background:#e74c3c; margin-top:10px;">Delete</button>
                </div>
            `).join('');
        }
        
        function displayVideos() {
            const container = document.getElementById('videos-list');
            if(videos.length === 0) {
                container.innerHTML = '<p>No videos added yet.</p>';
                return;
            }
            container.innerHTML = videos.map((video, index) => {
                const videoId = extractYouTubeId(video.url);
                return `
                    <div class="post">
                        <h3>🎬 ${escapeHtml(video.title)}</h3>
                        <div class="post-date">${video.date}</div>
                        ${videoId ? `<div class="video-container"><iframe src="https://www.youtube.com/embed/${videoId}" frameborder="0" allowfullscreen></iframe></div>` : `<p>Invalid YouTube URL</p>`}
                        <button onclick="deleteVideo(${index})" style="background:#e74c3c; margin-top:10px;">Delete</button>
                    </div>
                `;
            }).join('');
        }
        
        function extractYouTubeId(url) {
            const regExp = /^.*(youtu.be\/|v\/|u\/\w\/|embed\/|watch\?v=|\&v=)([^#\&\?]*).*/;
            const match = url.match(regExp);
            return (match && match[2].length === 11) ? match[2] : null;
        }
        
        // Add/Delete functions
        function addStory() {
            const title = document.getElementById('story-title').value;
            const content = document.getElementById('story-content').value;
            if(title && content) {
                stories.unshift({ title, content, date: new Date().toLocaleDateString() });
                saveAll();
                displayStories();
                document.getElementById('story-title').value = '';
                document.getElementById('story-content').value = '';
                alert('Story published!');
            } else {
                alert('Please fill in both title and content.');
            }
        }
        
        function deleteStory(index) {
            if(confirm('Delete this story?')) {
                stories.splice(index, 1);
                saveAll();
                displayStories();
            }
        }
        
        function setWordOfDay() {
            const word = document.getElementById('word-title').value;
            const definition = document.getElementById('word-definition').value;
            if(word && definition) {
                wordOfDay = { word, definition };
                saveAll();
                displayWordOfDay();
                document.getElementById('word-title').value = '';
                document.getElementById('word-definition').value = '';
                alert('Word of the day updated!');
            } else {
                alert('Please fill in both word and definition.');
            }
        }
        
        function addLyrics() {
            const song = document.getElementById('song-title').value;
            const lyricsContent = document.getElementById('lyrics-content').value;
            if(song && lyricsContent) {
                lyrics.unshift({ song, lyrics: lyricsContent, date: new Date().toLocaleDateString() });
                saveAll();
                displayLyrics();
                document.getElementById('song-title').value = '';
                document.getElementById('lyrics-content').value = '';
                alert('Lyrics added!');
            } else {
                alert('Please fill in both song title and lyrics.');
            }
        }
        
        function deleteLyrics(index) {
            if(confirm('Delete these lyrics?')) {
                lyrics.splice(index, 1);
                saveAll();
                displayLyrics();
            }
        }
        
        function addVideo() {
            const title = document.getElementById('video-title').value;
            const url = document.getElementById('video-url').value;
            if(title && url) {
                videos.unshift({ title, url, date: new Date().toLocaleDateString() });
                saveAll();
                displayVideos();
                document.getElementById('video-title').value = '';
                document.getElementById('video-url').value = '';
                alert('Video added!');
            } else {
                alert('Please fill in both title and URL.');
            }
        }
        
        function deleteVideo(index) {
            if(confirm('Delete this video?')) {
                videos.splice(index, 1);
                saveAll();
                displayVideos();
            }
        }
        
        function showSection(section) {
            const sections = ['stories', 'word', 'music', 'videos', 'admin'];
            sections.forEach(s => {
                document.getElementById(s).style.display = 'none';
            });
            document.getElementById(section).style.display = 'block';
            
            // Refresh content when switching views
            if(section === 'stories') displayStories();
            if(section === 'word') displayWordOfDay();
            if(section === 'music') displayLyrics();
            if(section === 'videos') displayVideos();
        }
        
        function escapeHtml(text) {
            const div = document.createElement('div');
            div.textContent = text;
            return div.innerHTML;
        }
        
        // Initial display
        displayStories();
        displayWordOfDay();
        displayLyrics();
        displayVideos();
    </script>
</body>
</html>