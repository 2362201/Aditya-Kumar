# Aditya-Kumar
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Netflix Clone</title>
    <link rel="icon" href="http://pngimg.com/uploads/netflix/small/netflix_PNG15.png" />
    <script src="https://www.gstatic.com/firebasejs/10.0.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.0.0/firebase-auth.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.0.0/firebase-firestore.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: Arial, Helvetica, sans-serif;
        }

        body {
            background-color: black;
            color: white;
        }

        .background {
            position: fixed;
            top: 0;
            left: 0;
            height: 100vh;
            width: 100vw;
            background: url('https://wallpapers.com/images/hd/netflix-background-gs7hjuwvv2g0e9fj.jpg') no-repeat center center/cover;
            z-index: -1;
            filter: brightness(0.4);
        }

        .navbar {
            display: flex;
            align-items: center;
            padding: 20px;
        }

        .brand_logo {
            height: 40px;
        }

        .top-navbar {
            display: flex;
            justify-content: flex-end;
            padding: 10px 30px;
            gap: 10px;
        }

        .nav-buttons button {
            padding: 8px 16px;
            font-size: 14px;
            background-color: #e50914;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        .nav-buttons button:hover {
            background-color: #b81d12;
        }

        .banner {
            height: 100vh;
            background-size: contain;
            background-position: center center;
            background-image: url('https://image.tmdb.org/t/p/original//lsHFo42x6B5gVBNxyzQF4Oh6e9V.jpg');
            display: flex;
            flex-direction: column;
            justify-content: center;
            padding: 60px 30px;
        }

        .banner__contents h1 {
            font-size: 4rem;
        }

        .banner__description {
            margin-top: 20px;
            font-size: 1.3rem;
            max-width: 600px;
        }

        .container {
            text-align: center;
            max-width: 800px;
            margin: 100px auto;
            padding: 0 20px;
        }

        .container h2 {
            font-size: 3rem;
            margin-bottom: 20px;
        }

        .container h3 {
            font-size: 1.5rem;
            margin-bottom: 15px;
        }

        .email-form {
            margin-top: 20px;
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            gap: 10px;
        }

        .email-form input {
            padding: 15px;
            width: 300px;
            font-size: 16px;
            border: none;
            border-radius: 2px;
            outline: none;
        }

        .email-form button {
            padding: 15px 25px;
            font-size: 16px;
            background-color: #e50914;
            color: white;
            border: none;
            border-radius: 2px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }

        .email-form button:hover {
            background-color: #b81d12;
        }

        .carousel {
            margin: 60px auto;
            padding: 20px;
            max-width: 1000px;
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            justify-content: center;
            overflow-x: scroll;
            scrollbar-width: none;
        }

        .carousel::-webkit-scrollbar {
            display: none;
            /* for Chrome, Safari */
        }

        .carousel img {
            width: 180px;
            border-radius: 10px;
            transition: transform 0.3s ease;
        }

        .carousel img:hover {
            transform: scale(1.1);
        }

        .visually-hidden {
            position: absolute;
            left: -9999px;
        }

        @media (max-width: 768px) {
            .container h2 {
                font-size: 2.5rem;
            }

            .container h3 {
                font-size: 1.2rem;
            }

            .email-form input,
            .email-form button {
                width: 80%;
            }
        }

        .top-navbar {
            position: sticky;
            top: 0;
            background: rgba(0, 0, 0, 0.7);
            z-index: 10;
        }

        section h3 {
            font-size: 1.5rem;
            margin: 20px 0 10px 30px;
        }

        #movie-modal {
            transition: all 0.3s ease;
        }

        .movie-card {
            position: relative;
            width: 200px;
            height: 300px;
            border-radius: 10px;
            overflow: hidden;
            transition: transform 0.3s ease;
        }

        .movie-card:hover {
            transform: scale(1.05);
        }

        .movie-img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            color: white;
            display: none;
            align-items: center;
            justify-content: center;
            text-align: center;
        }

        /* For smooth animations and hover effects */
        .movie-card:hover {
            transform: scale(1.1);
            transition: transform 0.3s ease;
        }

        .movie-card:hover .overlay {
            display: flex;
            opacity: 1;
            transition: opacity 0.3s ease-in-out;
        }

        .overlay {
            display: none;
            opacity: 0;
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            color: white;
            align-items: center;
            justify-content: center;
            text-align: center;
            padding: 10px;
        }

        .movie-img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        /* Navbar Styling - Sticky effect */
        .top-navbar {
            position: sticky;
            top: 0;
            background: rgba(0, 0, 0, 0.8);
            z-index: 10;
            padding: 15px;
        }
 
        /* Banner styling with fade-in */
        .banner {
            height: 100vh;
            background-size: cover;
            background-position: center center;
            background-image: url('https://image.tmdb.org/t/p/original/3NTAbAiao4JLzFQw6YxP1YZppM8.jpg');
            display: flex;
            flex-direction: column;
            justify-content: center;
            padding: 60px 30px;
            opacity: 0;
            animation: fadeIn 2s forwards;
        }

        @keyframes fadeIn {
            0% {
                opacity: 0;
            }

            100% {
                opacity: 1;
            }
        }

        .nav-buttons button:hover {
            background-color: #b81d12;
        }

        .carousel img {
            width: 180px;
            height: 270px;
            border-radius: 10px;
            transition: transform 0.3s ease;
        }

        .carousel img:hover {
            transform: scale(1.05);
        }

        /* Mobile responsiveness for smaller screens */
        @media (max-width: 768px) {
            .carousel {
                flex-direction: column;
                align-items: center;
                gap: 10px;
            }

            .carousel img {
                width: 100%;
                height: auto;
            }
        }

        /* Ensuring the movies are displayed in a row */
        /* Ensuring the movies are displayed in a row */
        .movie-row {
            display: flex;
            justify-content: space-between;
            gap: 10px;
            margin-top: 20px;
            flex-wrap: nowrap;
            /* Prevent wrapping */
        }

        /* Styling individual movie cards */
        .movie-card {
            display: flex;
            justify-content: center;
            align-items: center;
            flex: 0 0 auto;
            /* Prevent expansion and compression */
            border-radius: 10px;
            overflow: hidden;
            transition: transform 0.3s ease;
            height: 300px;
        }

        /* Image styling */
        .movie-img {
            width: 100%;
            height: auto;
            border-radius: 10px;
            object-fit: cover;
        }

        /* Hover effect */
        .movie-card:hover {
            transform: scale(1.05);
        }
    </style>
</head>

<body>
    <div class="background"></div>

    <header>
        <nav class="navbar">
            <div class="navbar_brand">
                <img src="https://www.freepnglogos.com/uploads/netflix-logo-0.png" alt="Netflix Logo"
                    class="brand_logo" />
            </div>
        </nav>
    </header>
    <div class="movie-row">
        <div class="movie-card">
            <img src="https://pics.filmaffinity.com/2025_the_world_enslaved_by_a_virus-659669776-large.jpg"
                alt="Movie Name" class="movie-img" />
        </div>

        <div class="movie-card">
            <img src="https://pics.filmaffinity.com/2025_the_world_enslaved_by_a_virus-659669776-large.jpg"
                alt="Movie Name" class="movie-img" />
        </div>

        <div class="movie-card">
            <img src="https://pics.filmaffinity.com/2025_the_world_enslaved_by_a_virus-659669776-large.jpg"
                alt="Movie Name" class="movie-img" />
        </div>
    </div>

    <div class="top-navbar">
        <div class="nav-buttons">
            <button type="button" class="signin-btn">Sign In</button>
            <button type="button" class="lang-btn">Hindi / English</button>
        </div>
    </div>
    <header class="banner">
        <div class="banner__contents">
            <h1>Money Heist</h1>
            <div class="banner__description">
                A criminal mastermind who goes by "The Professor" has a plan to pull off the biggest heist in recorded
                history.
            </div>
        </div>
    </header>

    <main class="container">
        <h2>Unlimited Movies, TV Shows, and More</h2>
        <h3>Watch anywhere. Cancel anytime.</h3>
        <h3>Ready to watch? Enter your email to create or restart your membership.</h3>

        <form class="email-form">
            <label for="email" class="visually-hidden">Email Address</label>
            <input type="email" id="email" placeholder="Email Address" required />
            <button type="submit">Get Started</button>
        </form>
    </main>

    <section class="carousel">
        <img src="https://image.tmdb.org/t/p/w300/qNBAXBIQlnOThrVvA6mA2B5ggV6.jpg" alt="Movie 1" />
        <img src="https://image.tmdb.org/t/p/w300/rXTqhpkpj6E0YilQ49PK1SSqLhm.jpg" alt="Movie 2" />
        <img src="https://image.tmdb.org/t/p/w300/7qop80YfuO0BwJa1uXk1DXUUEwv.jpg" alt="Movie 3" />
        <img src="https://image.tmdb.org/t/p/w300/hTExot1sfn7dHZjGrk0Aiwpntxt.jpg" alt="Movie 4" />
        <img src="https://image.tmdb.org/t/p/w300/kGzFbGhp99zva6oZODW5atUtnqi.jpg" alt="Movie 5" />
    </section>

    <div id="search-results"></div>

    <script>

        const API_KEY = 'YOUR_TMDB_API_KEY'; // Replace with your real TMDB API key
        const API_URL = 'https://api.themoviedb.org/3';
        const IMAGE_BASE = 'https://image.tmdb.org/t/p/w300';

        const firebaseConfig = {
            apiKey: "YOUR_API_KEY",
            authDomain: "YOUR_AUTH_DOMAIN",
            projectId: "YOUR_PROJECT_ID",
            storageBucket: "YOUR_STORAGE_BUCKET",
            messagingSenderId: "YOUR_SENDER_ID",
            appId: "YOUR_APP_ID"
        };

        // Initialize Firebase
        firebase.initializeApp(firebaseConfig);
        const auth = firebase.auth();
        const db = firebase.firestore();

        // Sign In / Sign Out
        document.querySelector('.signin-btn').addEventListener('click', () => {
            if (auth.currentUser) {
                auth.signOut().then(() => {
                    console.log("Signed out successfully");
                }).catch(error => {
                    console.error("Error during sign out:", error);
                });
            } else {
                const provider = new firebase.auth.GoogleAuthProvider();
                auth.signInWithPopup(provider).then(result => {
                    console.log(result.user);
                }).catch(error => {
                    console.error("Error during sign in:", error);
                });
            }
        });

        auth.onAuthStateChanged(user => {
            if (user) {
                document.querySelector('.signin-btn').innerText = "Sign Out";
            } else {
                document.querySelector('.signin-btn').innerText = "Sign In";
            }
        });

        // Search functionality
        document.getElementById('email').addEventListener('input', async (e) => {
            const query = e.target.value;
            if (query.length < 2) return;

            const res = await fetch(`${API_URL}/search/movie?api_key=${API_KEY}&query=${query}`);
            const data = await res.json();
            displaySearchResults(data.results);
        });

        function displaySearchResults(results) {
            const container = document.getElementById('search-results');
            container.innerHTML = '';
            results.forEach(movie => {
                const div = document.createElement('div');
                div.innerHTML = `<img src="${IMAGE_BASE}${movie.poster_path}" alt="${movie.title}" title="${movie.title}" />`;
                container.appendChild(div);
            });
        }

        // Add to Watchlist
        function saveToWatchlist(movie) {
            const user = firebase.auth().currentUser;
            if (!user) return alert("Please sign in first.");

            db.collection("users").doc(user.uid).collection("watchlist").add(movie);
        }

        // Infinite Scroll
        let currentPage = 1;
        let isLoading = false;

        window.addEventListener('scroll', () => {
            if (window.innerHeight + window.scrollY >= document.body.offsetHeight - 500 && !isLoading) {
                isLoading = true;
                fetchMoreMovies();
            }
        });

        async function fetchMoreMovies() {
            const res = await fetch(`${API_URL}/trending/movie/week?api_key=${API_KEY}&page=${++currentPage}`);
            const data = await res.json();
            createRow("More Trending", data.results);
            isLoading = false;
        }

        function createRow(title, movies) {
            const section = document.createElement('section');
            section.innerHTML = `<h3>${title}</h3>`;
            const carousel = document.createElement('div');
            carousel.classList.add('carousel');

            movies.forEach(movie => {
                const img = document.createElement('img');
                img.src = `${IMAGE_BASE}${movie.poster_path}`;
                img.alt = movie.title;
                img.loading = "lazy";
                img.addEventListener('click', () => showMovieDetails(movie));

                const addToWatchlistButton = document.createElement('button');
                addToWatchlistButton.innerText = "Add to Watchlist";
                addToWatchlistButton.addEventListener('click', () => {
                    saveToWatchlist(movie);
                });

                carousel.appendChild(img);
                carousel.appendChild(addToWatchlistButton);
            });

            section.appendChild(carousel);
            document.body.appendChild(section);
        }

        function showMovieDetails(movie) {
            const modal = document.createElement('div');
            modal.innerHTML = `
                <span onclick="document.body.removeChild(this.parentElement)">✖</span>
                <h2>${movie.title}</h2>
                <p>${movie.overview || 'No description available.'}</p>
            `;
            document.body.appendChild(modal);
        }

        // Initial fetch for trending movies
        fetchMoreMovies();
    </script>
</body>

</html>
