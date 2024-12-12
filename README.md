<!DOCTYPE html><html lang="en"><head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Predict Wingo</title>
    <link rel="icon" type="image/png" href="images/favicon.png">
    <script src="https://cdn.tailwindcss.com"></script>
        <script src="js/particles.min.js"></script>
    <link rel="stylesheet" href="css/styles.css">
    <style>
    

/* Styles for the 3D dice loading animation */
.loader-dice {
    perspective: 600px;
}

.dice {
    position: relative;
    width: 60px;
    height: 60px;
    transform-style: preserve-3d;
    animation: spin 2s infinite linear;
}

.face {
    position: absolute;
    width: 60px;
    height: 60px;
    background: linear-gradient(45deg, #1e3a8a, #2563eb);
    border: 2px solid #fbbf24;
    box-shadow: 0 0 15px rgba(251, 191, 36, 0.6);
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 24px;
    font-weight: bold;
    color: #fff;
}

.face::before {
    content: "";
    position: absolute;
    top: -2px;
    left: -2px;
    right: -2px;
    bottom: -2px;
    border: 2px solid rgba(251, 191, 36, 0.5);
    box-shadow: 0 0 15px rgba(251, 191, 36, 0.5);
}

.front  { transform: rotateY(  0deg) translateZ(30px); }
.back   { transform: rotateY(180deg) translateZ(30px); }
.right  { transform: rotateY( 90deg) translateZ(30px); }
.left   { transform: rotateY(-90deg) translateZ(30px); }
.top    { transform: rotateX( 90deg) translateZ(30px); }
.bottom { transform: rotateX(-90deg) translateZ(30px); }

@keyframes spin {
    0%   { transform: rotateX(0deg) rotateY(0deg); }
    100% { transform: rotateX(360deg) rotateY(360deg); }
}



.prediction-item {
    margin-bottom: 0; /* Reducing space between items */
    padding: 5px; /* Adjust padding as necessary */
}


.prediction-item:hover {
    transform: scale(1.05);
}

        .loader {
            border: 4px solid #f3f3f3;
            border-radius: 50%;
            border-top: 4px solid #3498db;
            width: 20px;
            height: 20px;
            -webkit-animation: spin 2s linear infinite; /* Safari */
            animation: spin 2s linear infinite;
            margin: 0 auto;
        }

        /* Safari */
        @-webkit-keyframes spin {
            0% { -webkit-transform: rotate(0deg); }
            100% { -webkit-transform: rotate(360deg); }
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .loading-message {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 1000;
        }
        .prediction-item {
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 5px 0;
            padding: 10px;
            border-radius: 8px;
            background: rgba(255, 255, 255, 0.1);
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
            font-size: 1em;
            transition: transform 0.2s, background 0.2s;
        }
        .prediction-item:hover {
            transform: scale(1.05);
            background: rgba(255, 255, 255, 0.2);
        }
        .prediction-item span {
            margin-left: 5px;
            font-size: 1.1em;
        }
        .emoji {
            font-size: 1.5em;
        }
    </style>
    <link rel="stylesheet" href="css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;700&amp;display=swap" rel="stylesheet">
    <script src="js/logic.js"></script>
</head>
<body class="text-white relative overflow-hidden">
        <div id="particles-js" class="fixed inset-0 z-[-1]"></div>
    <audio id="predict-sound" src="media/predict.mp3" preload="auto"></audio>
        <ul class="bg-bubbles relative z-10">
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
    </ul>

<div id="welcome-popup" class="fixed inset-0 bg-black bg-opacity-50 z-50 flex items-center justify-center px-4">
    <div class="welcome-popup-content bg-gradient-to-br from-blue-900 to-indigo-800 rounded-lg p-4 w-full max-w-xs shadow-2xl">
        <h2 class="text-xl font-bold text-center mb-3 text-yellow-400 glow-text">Welcome to Predict Wingo!</h2>
        <div class="text-white text-sm space-y-3">
            <p class="font-semibold">Our prediction system offers:</p>
            <ul class="list-none space-y-2">
                <li class="flex items-start">
                    <span class="text-green-400 mr-2">✓</span>
                    <span>100% accurate predictions for accounts created through our server</span>
                </li>
                <li class="flex items-start">
                    <span class="text-red-400 mr-2">�&nbsp;</span>
                    <span>Only 20% accuracy for accounts not created through our server</span>
                </li>
            </ul>
            <p class="text-xs italic mt-2 text-yellow-200">Create a new account now to access VIP predictions. It's completely free!</p>
        </div>
        <div class="mt-4 space-y-2">
            <a href="https://t.me/predictwingoo" target="_blank" class="block w-full bg-yellow-500 text-blue-900 px-3 py-2 rounded-full font-bold text-center hover:bg-yellow-600 transition duration-300 text-sm shadow-md">Join Telegram</a>
            <button id="close-welcome-popup" class="w-full bg-blue-700 text-white px-3 py-2 rounded-full font-bold hover:bg-blue-600 transition duration-300 text-sm shadow-md" onclick="closeWelcomePopup()">Got it!</button>
        </div>
    </div>
</div>




    <main>
<section class="predict-wingo-section py-8 flex flex-col items-center justify-center relative">
  <h1 class="text-4xl md:text-5xl lg:text-6xl font-bold text-transparent bg-clip-text bg-gradient-to-r from-yellow-400 via-red-500 to-pink-500 animate-gradient-x">
    PREDICT WINGO
  </h1>
  <div class="mt-2 text-sm md:text-base text-blue-200 font-semibold">
    Your Ultimate Prediction Platform
  </div>
</section>
<section class="container mx-auto px-4 py-10 relative">
  <h2 class="text-3xl font-bold text-center mb-6 text-stroke">
    Best Prediction Tool In The Market
  </h2>
    <div class="content-container mb-10">
                <div class="content-block glow-effect">
                    <div class="content-block prediction-box prediction-box-glow">
                        <div class="prediction-content">
                            <h3 class="text-2xl font-bold mb-4 text-yellow-400">Prediction</h3>
                            <p class="mb-4 text-lg text-blue-200">Feel the True Power of Our Predictor</p>
                            <p class="text-base text-blue-100">Here's how it works: our predictor utilizes direct APIs related to gaming sites. We employ advanced technology to fetch data before the game starts, providing you with a 100% guarantee.</p>
                        </div>
                    </div>
                </div>
                <div class="content-block">
                    <div class="prediction-content">
                        <h3 class="text-2xl font-bold mb-4 text-yellow-400">Guarantee</h3>
                        <p class="mb-4 text-lg text-blue-200">We Offer More Than Just Predictions. We Provide Peace of Mind.</p>
                        <p class="text-base text-blue-100">With our 100% guarantee, you can bet with confidence, knowing that our predictions are accurate. Say goodbye to doubts.</p>
                    </div>
                </div>
            </div>
            <div class="flex flex-wrap justify-center items-center gap-3 mb-10">
                <button data-prediction-button="" class="bg-yellow-400 hover:bg-yellow-500 text-blue-900 rounded-full px-6 py-2 font-bold text-lg button">Prediction</button>
                <button data-demo-button="" class="bg-yellow-400 hover:bg-yellow-500 text-blue-900 rounded-full px-6 py-2 font-bold text-lg button">Demo</button>
                <button data-proof-button="" class="bg-yellow-400 hover:bg-yellow-500 text-blue-900 rounded-full px-6 py-2 font-bold text-lg button">Proof</button>
                <button data-trends-button="" class="bg-yellow-400 hover:bg-yellow-500 text-blue-900 rounded-full px-6 py-2 font-bold text-lg button">Trends</button>
                 <button vip-button="" class="bg-yellow-400 hover:bg-yellow-500 text-blue-900 rounded-full  
 px-6 py-2 font-bold text-lg button" onclick="window.location.href='https://predictvip.in'">VIP</button>  
            </div>
            <div class="separator"></div>
            <div id="proof-container" class="proof-container hidden">
                <div class="proof-content">
                    <button id="close-proof" class="close-proof">×</button>
                    <div class="proof-image-container">
                        <img class="proof-image" src="images/demo1.jpeg" alt="Proof Image 1">
                        <img class="proof-image" src="images/demo2.jpeg" alt="Proof Image 2">
                        <img class="proof-image" src="images/demo3.jpeg" alt="Proof Image 3">
                        <img class="proof-image" src="images/demo4.jpeg" alt="Proof Image 4">
                    </div>
                </div>
            </div>
        </section>
        <section class="container mx-auto px-4 py-6">
            <h2 class="text-3xl font-bold text-center mb-6 text-stroke">Recent Winnings</h2>
            <div class="winnings-container overflow-x-auto">
                <div id="winnings-wrapper" class="winnings-wrapper flex space-x-6">
                    <div class="winnings-block flex-none">
                        <div class="winnings-content">
                            <p class="winnings-text"><span class="winnings-name"></span> has won ₹<span class="winnings-amount"></span></p>
                            <p class="winnings-round">on round <span class="winnings-issue"></span> using Predict Wingo</p>
                        </div>
                    </div>
                    <div class="winnings-block flex-none">
                        <div class="winnings-content">
                            <p class="winnings-text"><span class="winnings-name"></span> has won ₹<span class="winnings-amount"></span></p>
                            <p class="winnings-round">on round <span class="winnings-issue"></span> using Predict Wingo</p>
                        </div>
                    </div>
                    <div class="winnings-block flex-none">
                        <div class="winnings-content">
                            <p class="winnings-text"><span class="winnings-name"></span> has won ₹<span class="winnings-amount"></span></p>
                            <p class="winnings-round">on round <span class="winnings-issue"></span> using Predict Wingo</p>
                        </div>
                    </div>
                    <div class="winnings-block flex-none">
                        <div class="winnings-content">
                            <p class="winnings-text"><span class="winnings-name"></span> has won ₹<span class="winnings-amount"></span></p>
                            <p class="winnings-round">on round <span class="winnings-issue"></span> using Predict Wingo</p>
                        </div>
                    </div>
                    <div class="winnings-block flex-none">
                        <div class="winnings-content">
                            <p class="winnings-text"><span class="winnings-name"></span> has won ₹<span class="winnings-amount"></span></p>
                            <p class="winnings-round">on round <span class="winnings-issue"></span> using Predict Wingo</p>
                        </div>
                    </div>
                    <div class="winnings-block flex-none">
                        <div class="winnings-content">
                            <p class="winnings-text"><span class="winnings-name"></span> has won ₹<span class="winnings-amount"></span></p>
                            <p class="winnings-round">on round <span class="winnings-issue"></span> using Predict Wingo</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>
        <section class="container mx-auto px-4 py-6 sm:py-10">
            <h2 class="text-3xl font-bold text-center mb-6 text-stroke">Our Performance</h2>
            <div class="rating-container grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                <div class="rating-block bg-blue-800 bg-opacity-75 p-6 rounded-lg shadow-lg">
                    <h3 class="text-xl font-bold mb-2 text-yellow-400">API Response Speed</h3>
                    <div class="flex items-center">
                        <span class="text-4xl font-bold mr-2 text-white">9.8</span>
                        <span class="text-lg text-blue-300">/10</span>
                    </div>
                    <p class="mt-2 text-blue-200">Lightning-fast predictions</p>
                </div>
                <div class="rating-block bg-blue-800 bg-opacity-75 p-6 rounded-lg shadow-lg">
                    <h3 class="text-xl font-bold mb-2 text-yellow-400">Prediction Accuracy</h3>
                    <div class="flex items-center">
                        <span class="text-4xl font-bold mr-2 text-white">10</span>
                        <span class="text-lg text-blue-300">/10</span>
                    </div>
                    <p class="mt-2 text-blue-200">100% accurate predictions</p>
                </div>
                <div class="rating-block bg-blue-800 bg-opacity-75 p-6 rounded-lg shadow-lg">
                    <h3 class="text-xl font-bold mb-2 text-yellow-400">Development Team</h3>
                    <div class="flex items-center">
                        <span class="text-4xl font-bold mr-2 text-white">9.9</span>
                        <span class="text-lg text-blue-300">/10</span>
                    </div>
                    <p class="mt-2 text-blue-200">Expert developers &amp; analysts</p>
                </div>
                <div class="rating-block bg-blue-800 bg-opacity-75 p-6 rounded-lg shadow-lg">
                    <h3 class="text-xl font-bold mb-2 text-yellow-400">User Satisfaction</h3>
                    <div class="flex items-center">
                        <span class="text-4xl font-bold mr-2 text-white">9.7</span>
                        <span class="text-lg text-blue-300">/10</span>
                    </div>
                    <p class="mt-2 text-blue-200">Trusted by thousands</p>
                </div>
            </div>
        </section>
<div class="flex flex-wrap justify-center items-center gap-3 mb-10">
    <button data-about-us-button="" class="bg-yellow-400 hover:bg-yellow-500 text-blue-900 rounded-full px-6 py-2 font-bold text-lg button">About Us</button>
    <button data-disclaimer-button="" class="bg-yellow-400 hover:bg-yellow-500 text-blue-900 rounded-full px-6 py-2 font-bold text-lg button">Disclaimer</button>
    <button data-warning-button="" class="bg-yellow-400 hover:bg-yellow-500 text-blue-900 rounded-full px-6 py-2 font-bold text-lg button">Warning</button>
</div>


    


<div id="trends-container" class="popup-container hidden">
    <div class="popup-content bg-gradient-to-br from-blue-900 to-blue-700 rounded-lg shadow-2xl max-w-md mx-auto p-8">
        <button id="close-trends" class="close-popup text-white hover:text-yellow-400 transition-colors duration-300">×</button>
             <div class="color-emojis">🔴🟢🟣</div>
        <h2 class="text-3xl font-bold mb-6 text-center text-white-400 glow-shadow-stroke">API MENU</h2>
        <p class="text-white text-center mb-4">Please select a platform to view trends for:</p>
        <div id="trends-platform-options" class="platform-options">
                        <button class="platform-option bg-blue-800 text-white hover:bg-blue-1000 transition-colors duration-300 py-2 px-4 rounded-lg mb-2 w-full" data-platform="OKWin">OK-Win</button>
            <button class="platform-option bg-blue-800 text-white hover:bg-blue-1000 transition-colors duration-300 py-2 px-4 rounded-lg mb-2 w-full" data-platform="82 Lottery">82 Lottery</button>
            <button class="platform-option bg-blue-800 text-white hover:bg-blue-1000 transition-colors duration-300 py-2 px-4 rounded-lg mb-2 w-full" data-platform="Big Daddy">Big Daddy</button>
            <button class="platform-option bg-blue-800 text-white hover:bg-blue-1000 transition-colors duration-300 py-2 px-4 rounded-lg mb-2 w-full" data-platform="Lucknow Games">Lucknow Games</button>

        </div>
        <button id="trends-confirm-button" class="platform-confirm-button hidden bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded mt-4">Confirm</button>
    </div>
</div>

<div id="accuracy-container" class="popup-container hidden">
    <div class="popup-content">
        <button id="close-accuracy" class="close-popup">×</button>
        <div class="color-emojis">🔴🟢🟣</div>
        <div id="vip-platform-selection-container">
          <h2 class="text-2xl font-bold mb-4 text-center text-yellow-400">VIP MENU</h2>
            <p class="platform-selection-text">Please select a platform to receive vip predictions for:</p>
            <div id="vip-platform-options" class="platform-options">
                <button class="platform-option bg-blue-800 text-white hover:bg-blue-1000 transition-colors duration-300 py-2 px-4 rounded-lg mb-2 w-full" data-platform="OK-Win">OK-Win</button>
                <button class="platform-option bg-blue-800 text-white hover:bg-blue-1000 transition-colors duration-300 py-2 px-4 rounded-lg mb-2 w-full" data-platform="82 Lottery">82 Lottery</button>
                <button class="platform-option bg-blue-800 text-white hover:bg-blue-1000 transition-colors duration-300 py-2 px-4 rounded-lg mb-2 w-full" data-platform="Big Daddy">Big Daddy</button>
                <button class="platform-option bg-blue-800 text-white hover:bg-blue-1000 transition-colors duration-300 py-2 px-4 rounded-lg mb-2 w-full" data-platform="Lucknow Games">Lucknow Games</button>
            </div>
            <button id="vip-platform-confirm-button" class="platform-confirm-button hidden">Confirm</button>
        </div>
    </div>
</div>

<div id="trends-report-container" class="popup-container hidden">
    <div class="popup-content bg-gradient-to-br from-blue-900 to-blue-700 rounded-lg shadow-2xl max-w-md mx-auto p-8">
        <button id="close-trends-report" class="close-popup text-white hover:text-yellow-400 transition-colors duration-300">×</button>
        <div class="color-emojis mr-2">🔴🟢🟣</div>
       <h2 id="trends-platform-name" class="text-2xl font-bold mb-6 text-center text-white-400 glow-shadow-stroke uppercase"></h2>
    <div id="trends-data-container" class="bg-blue-800 bg-opacity-50 p-4 rounded-lg shadow-inner overflow-y-auto max-h-96 mb-4">
            <!-- Trends data will be populated here -->
        </div>
    </div>
</div>


    <div id="vip-prediction-container" class="popup-container hidden">
        <div class="popup-content">
            <button id="close-vip-prediction" class="close-popup">×</button>
            <div class="color-emojis">🔴🟢🟣</div>
            <div id="platform-selection-container">
                <h2 class="vip-menu-title text-3xl font-bold mb-6 text-center text-white-400 glow-shadow-stroke">FREE MENU</h2>
                <p class="platform-selection-text">Please select a platform to receive free predictions for:</p>
                <div id="platform-options" class="platform-options">
                    <button class="platform-option bg-blue-800 text-white hover:bg-blue-1000 transition-colors duration-300 py-2 px-4 rounded-lg mb-2 w-full" data-platform="OK-Win">OK-Win</button>
                    <button class="platform-option bg-blue-800 text-white hover:bg-blue-1000 transition-colors duration-300 py-2 px-4 rounded-lg mb-2 w-full" data-platform="51 Game">51 Game</button>
                    <button class="platform-option bg-blue-800 text-white hover:bg-blue-1000 transition-colors duration-300 py-2 px-4 rounded-lg mb-2 w-full" data-platform="82 Lottery">82 Lottery</button>
                </div>
                <button id="platform-confirm-button" class="platform-confirm-button hidden">Confirm</button>
            </div>

<div id="testing-menu-container" class="popup-container hidden">
  <div class="popup-content bg-gradient-to-br from-blue-900 to-indigo-900 rounded-lg shadow-2xl max-w-sm mx-auto p-4">
    <div class="flex justify-between items-center mb-2">
           <h2 id="platform-name" class="text-2xl font-bold text-center text-white-400 glow-shadow-stroke animate-pulse w-full uppercase"></h2>
     <button id="language-toggle" class="bg-blue-800 hover:bg-blue-700 text-white rounded-full p-1 transition-colors duration-300">
        <span id="language-icon" class="text-sm">🇬🇧</span>
      </button>
    </div>
    <div class="text-white text-center mb-3">
      <div class="bg-blue-800 bg-opacity-50 p-3 rounded-lg shadow-inner">
        <h3 id="instruction-title" class="text-lg font-bold text-yellow-300 mb-2">🚀 Important Instructions</h3>
        <ul class="space-y-2 text-left text-xs">
          <li class="flex items-start">
            <span class="text-green-400 mr-1">✅</span>
            <span id="instruction-1" class="text-blue-100">Create a new account via the "Start" button and deposit <span class="font-bold text-yellow-300">500 Rs</span> or more for server connection. Our app checks the server to ensure accurate predictions.</span>
          </li>
          <li class="flex items-start">
            <span class="text-red-400 mr-1">�&nbsp;️</span>
            <span id="instruction-2" class="text-red-100">Warning: Accounts not created through our link will give <span class="font-bold text-red-400">incorrect predictions</span> due to server mismatch.</span>
          </li>
          <li class="flex items-start">
            <span class="text-yellow-400 mr-1">⚡</span>
            <span id="instruction-3" class="text-yellow-100">Old accounts will also provide <span class="font-bold text-red-400">incorrect predictions</span> due to server mismatch.</span>
          </li>
          <li class="flex items-start">
            <span class="text-green-400 mr-1">🎯</span>
            <span id="instruction-4" class="text-green-100">For <span class="font-bold text-green-400">100% accurate predictions</span>, use the account created via our URL and ensure a deposit of 500+ Rs.</span>
          </li>
        </ul>
      </div>
    </div>
    <div class="flex justify-center space-x-2">
      <a id="start-button" href="#" target="_blank" class="bg-yellow-400 hover:bg-yellow-500 text-blue-900 font-bold py-1.5 px-3 rounded-full text-xs uppercase tracking-wider transition-all duration-300 shadow-lg hover:shadow-xl transform hover:-translate-y-1 flex items-center">
        <span class="mr-1">Start</span>
        <svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path>
        </svg>
      </a>
      <button id="continue-button" class="bg-green-500 hover:bg-green-600 text-white font-bold py-1.5 px-3 rounded-full text-xs uppercase tracking-wider transition-all duration-300 shadow-lg hover:shadow-xl transform hover:-translate-y-1 flex items-center">
        <span class="mr-1">Continue</span>
        <svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3"></path>
        </svg>
      </button>
      <a href="https://youtu.be/bqxS77Cj3UM?si=cORbf9_fW9ipNpTV" target="_blank" class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-1.5 px-3 rounded-full text-xs uppercase tracking-wider transition-all duration-300 shadow-lg hover:shadow-xl transform hover:-translate-y-1 flex items-center">
        <span class="mr-1">Help</span>
        <svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8.228 9c.549-1.165 2.03-2 3.772-2 2.21 0 4 1.343 4 3 0 1.4-1.278 2.575-3.006 2.907-.542.104-.994.54-.994 1.093m0 3h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path>
        </svg>
      </a>
    </div>
  </div>
</div>

<script>
function setLanguageContent(isEnglish) {
  const instructionTitle = document.getElementById('instruction-title');
  const instruction1 = document.getElementById('instruction-1');
  const instruction2 = document.getElementById('instruction-2');
  const instruction3 = document.getElementById('instruction-3');
  const instruction4 = document.getElementById('instruction-4');

  if (isEnglish) {
    instructionTitle.textContent = '🚀 Important Instructions';
    instruction1.innerHTML = 'Create a new account via the "Start" button and deposit <span class="font-bold text-yellow-300">500 Rs</span> or more for server connection. Our app checks the server to ensure accurate predictions.';
    instruction2.innerHTML = 'Warning: Accounts not created through our link will give <span class="font-bold text-red-400">incorrect predictions</span> due to server mismatch.';
    instruction3.innerHTML = 'Old accounts will also provide <span class="font-bold text-red-400">incorrect predictions</span> due to server mismatch.';
    instruction4.innerHTML = 'For <span class="font-bold text-green-400">100% accurate predictions</span>, use the account created via our URL and ensure a deposit of 500+ Rs.';
  } else {
    instructionTitle.textContent = '🚀 महत्वपूर्ण निर्देश';
    instruction1.innerHTML = '"Start" बटन से नया अकाउंट बनाएं और <span class="font-bold text-yellow-300">500 रुपये</span> या ज्यादा जमा करें। हमारा ऐप सही प्रेडिक्शन के लिए सर्वर चेक करता है।';
    instruction2.innerHTML = 'सावधान: हमारे लिंक से नहीं बने अकाउंट में <span class="font-bold text-red-400">गलत प्रेडिक्शन</span> मिलेंगे क्योंकि सर्वर मैच नहीं होगा।';
    instruction3.innerHTML = 'पुराने अकाउंट में भी <span class="font-bold text-red-400">गलत प्रेडिक्शन</span> मिलेंगे क्योंकि सर्वर मैच नहीं होगा।';
    instruction4.innerHTML = '<span class="font-bold text-green-400">100% सही प्रेडिक्शन</span> के लिए, हमारे URL से बना अकाउंट इस्तेमाल करें और 500+ रुपये जमा करें।';
  }
}
  // Language toggle functionality
  const languageToggle = document.getElementById('language-toggle');
  const languageIcon = document.getElementById('language-icon');
  let isEnglish = true;

  function toggleLanguage() {
    isEnglish = !isEnglish;
    languageIcon.textContent = isEnglish ? '🇬🇧' : '🇮🇳';
    setLanguageContent(isEnglish);
  }

  languageToggle.addEventListener('click', toggleLanguage);

  // Initialize content in English
  setLanguageContent(true);
  languageIcon.textContent = '🇬🇧';
  
document.addEventListener('DOMContentLoaded', function() {
    const vipPlatform = document.getElementById('vip-platform');
    const secretCodeDialog = document.getElementById('secret-code-dialog');
    const closeSecretCodeDialog = document.getElementById('close-secret-code-dialog');
    const secretCodeInput = document.getElementById('secret-code-input');
    const submitSecretCode = document.getElementById('submit-secret-code');
    const secretCodeError = document.getElementById('secret-code-error');
    let clickCount = 0;
    const secretCode = '3199';

    vipPlatform.addEventListener('click', () => {
        clickCount++;
        if (clickCount === 6) {
            secretCodeDialog.classList.remove('hidden');
            body.classList.add('overflow-hidden');
        }
    });

    closeSecretCodeDialog.addEventListener('click', () => {
        secretCodeDialog.classList.add('hidden');
        body.classList.remove('overflow-hidden');
        clickCount = 0;
        secretCodeInput.value = '';
        secretCodeError.classList.add('hidden');
    });

 submitSecretCode.addEventListener('click', () => {
        if (secretCodeInput.value === secretCode) {
            enableDeveloperMode();
            alert('Developer mode enabled!');
            secretCodeDialog.classList.add('hidden');
            body.classList.remove('overflow-hidden');
            clickCount = 0;
            secretCodeInput.value = '';
            secretCodeError.classList.add('hidden');
        } else {
            secretCodeError.classList.remove('hidden');
        }
    });
});

// Retrieve stored values
const storedDeveloperMode = localStorage.getItem('isDeveloperMode');
if (storedDeveloperMode === 'true') {
  isDeveloperMode = true;
  const storedStartTime = localStorage.getItem('developerStartTime');
  if (storedStartTime) {
    developerStartTime = parseInt(storedStartTime);
  }
  const storedIssueIndex = localStorage.getItem('developerIssueIndex');
  if (storedIssueIndex) {
    developerIssueIndex = parseInt(storedIssueIndex);
  }
}
  
</script>

<div id="vip-key-container" class="vip-key-container hidden">
  <div class="vip-details">
     <h2 class="text-2xl font-bold mb-4 text-center text-yellow-400">WIN MENU</h2>
     <h3 id="vip-platform" class="vip-menu-title text-2xl font-bold mb-6 text-center text-white-300 glow-shadow-stroke">Platform Name</h3>

     <!-- Secret Code Dialog -->
     <div id="secret-code-dialog" class="popup-container hidden">
         <div class="popup-content">
             <button id="close-secret-code-dialog" class="close-popup">×</button>
             <h2 class="text-2xl font-bold mb-6 text-center text-yellow-400 glow-shadow-stroke">Enter Secret Code</h2>
             <input type="text" id="secret-code-input" class="key-input" placeholder="Enter your secret code">
             <button id="submit-secret-code" class="confirm-button">Submit</button>
             <p id="secret-code-error" class="text-red-500 mt-2 hidden">Incorrect code. Please try again.</p>
         </div>
     </div>
    <p id="vip-timer" class="vip-timer text-lg mb-1">Next game in - <span id="vip-timer-value" class="font-bold text-yellow-400">22</span> seconds</p>
    <div id="vip-status" class="vip-status mb-4"> 
      <span class="text-base font-bold text-yellow-400">Issue Number: </span>
      <span id="issue-number-value_new" class="text-base font-bold text-white ml-1"></span>
    </div>
    <div id="vip-prediction" class="vip-prediction bg-blue-800 bg-opacity-50 p-4 rounded-lg shadow-inner text-center mb-6">
      <h2 class="text-2xl font-bold mb-4 text-center text-yellow-400">PREDICTION MENU</h2>
      <p class="text-lg font-bold text-yellow-400">Awaiting prediction request...</p>
    </div>
    <button id="predict-button" class="predict-button bg-yellow-400 hover:bg-yellow-500 text-blue-900 font-bold py-2 px-4 rounded-full text-lg uppercase tracking-wider transition-all duration-200">Predict 🚀</button>
  </div>
</div>
        </div>
    </div>
    <script>


    // This section has been removed as it's now handled by the toggleLanguage function
    </script>
<div id="about-us-container" class="popup-container hidden">
    <div class="popup-content bg-gradient-to-br from-blue-900 to-blue-700 rounded-lg shadow-2xl max-w-2xl mx-auto p-8">
        <button id="close-about-us" class="close-popup text-white hover:text-yellow-400 transition-colors duration-300">×</button>
        <h2 class="text-2xl font-bold mb-6 text-center text-yellow-400 glow-shadow-stroke">About Predict Wingo</h2>
        <div class="about-us-content space-y-4 text-white">
            <p class="animated-text mb-6" style="font-size: 0.75rem;">Life is a gamble and We live only once, so let's have fun at Predict Wingo!</p>
            <p class="mb-4">Welcome to Predict Wingo, the leading online platform for predicting winning numbers in popular Wingo lottery games. Our expert data and AI analysts have developed an advanced algorithm that analyzes past lottery draws and player behavior to provide 100% accurate predictions.</p>
            <p class="mb-4">At Predict Wingo, we believe everyone deserves a chance to win big. Just visit the prediction section, select your game platform, click 'predict', and get your predictions for the winning color, size, and number.</p>
            <p class="mb-4">We're proud of our 100% success rate. If you have any questions, feel free to contact us through email or live chat support. Don't wait! Go to the predict section and start predicting like a pro with Predict Wingo!</p>
        </div>
    </div>
</div>
<div id="disclaimer-container" class="popup-container hidden">
    <div class="popup-content bg-gradient-to-br from-blue-900 to-blue-700 rounded-lg shadow-2xl max-w-2xl mx-auto p-8">
        <button id="close-disclaimer" class="close-popup text-white hover:text-yellow-400 transition-colors duration-300">×</button>
        <h2 class="text-3xl font-bold mb-6 text-center text-yellow-400 glow-shadow-stroke">Disclaimer</h2>
        <div class="disclaimer-content space-y-4 text-white">
            <p class="mb-4">Please note that Predict Wingo provides predictions based on statistical analysis and past lottery results. Our predictions are meant to serve as a guide, and not to encourage addiction. We make predictions for platforms, and if they shut down or if something happens to them, we are not responsible.</p>
            <p class="mb-4">Participation in lottery games involves risk. It is important to play responsibly and within your financial limits.</p>
            <p class="mb-4 text-lg font-bold text-yellow-400">By using our services, you acknowledge that you understand the risks involved and agree to take responsibility for your decisions and actions. Remember, lottery games are a form of gambling, and it is essential to make informed choices.</p>
        </div>
    </div>
</div>
    <div id="warning-container" class="popup-container hidden">
        <div class="popup-content bg-gradient-to-br from-blue-900 to-blue-700 rounded-lg shadow-2xl max-w-2xl mx-auto p-8">
            <button id="close-warning" class="close-popup text-white hover:text-yellow-400 transition-colors duration-300">×</button>
            <h2 class="text-2xl font-bold mb-6 text-center text-yellow-400 glow-shadow-stroke">Important Warning</h2>
            <div class="warning-content space-y-4 text-white">
                <p class="mb-4">At Predict Wingo, we believe in empowering our users with knowledge. It's crucial to understand that while platforms like 91 Club, TC Lottery, Big Daddy, and 82 Lottery claim to use advanced systems like SHA-256 for generating random results, there's always a potential for manipulation. This is where our expertise comes in handy.</p>
                <p class="mb-4 text-lg font-bold text-yellow-400">How We Protect You:</p>
                <ul class="list-disc pl-6 mb-4 space-y-2">
                    <li>Our sophisticated API system constantly monitors these platforms, tracking any data that might be manipulated.</li>
                    <li>We use advanced algorithms to detect patterns and anomalies that could indicate unfair practices.</li>
                    <li>Our predictions are based on a comprehensive analysis of multiple factors, not just the platform's provided data.</li>
                </ul>
                <p class="mb-4 text-lg font-bold text-yellow-400">Potential Manipulation Tactics:</p>
                <ol class="list-decimal pl-6 mb-4 space-y-2">
                    <li><strong>Controlled "Randomness":</strong> Platforms might influence when and how their "random" number generation is applied.</li>
                    <li><strong>Hidden Algorithm Changes:</strong> Sudden, undisclosed changes to algorithms could unfairly shift odds.</li>
                    <li><strong>Selective Payout Practices:</strong> Some platforms might delay or withhold certain payouts without clear explanation.</li>
                </ol>
                <p class="text-lg font-bold text-yellow-400">Stay Informed, Play Smart:</p>
                <p>We urge all our users to approach these platforms with a critical eye. Always verify a platform's credibility before engaging. With Predict Wingo by your side, you're equipped with the insights needed to make informed decisions and maximize your chances of success. Remember, knowledge is power – especially in the world of online lotteries!</p>
            </div>
        </div>
    </div>
    <script>
let isDeveloperMode = true;
let isManualDeveloper = false; // Set to true to enable manual developer predictions
let developerData = null;
let developerTimer = null;
let developerIssueIndex = 0n;
let developerPreviousCyclesPassed = -1n;

document.getElementById('predict-button').addEventListener('click', function() {
    const predictionElement = document.getElementById('vip-prediction');
    const sound = document.getElementById('predict-sound');

predictionElement.innerHTML = `
<!-- Enhanced fetching prediction message with a 3D dice animation -->
<div class="flex flex-col items-center mt-4">
    <p class="text-xl font-bold text-yellow-400 mb-4">Fetching prediction...</p>
    <div class="loader-dice">
        <div class="dice">
            <div class="face front"></div>
            <div class="face back"></div>
            <div class="face right"></div>
            <div class="face left"></div>
            <div class="face top"></div>
            <div class="face bottom"></div>
        </div>
    </div>
</div>
`;



setTimeout(() => {
  if (isDeveloperMode && developerData) {
    const issueNumber = developerData.startIssueNumber + developerIssueIndex;
    
    let predictions;
    
    if (isManualDeveloper) {
      // Use the predictions from manualdeveloper.json
      predictions = developerData.predictions[issueNumber.toString()];
      if (!predictions) {
        predictionElement.innerHTML = `
          <p class="text-lg font-bold text-red-500">No prediction available for issue number ${issueNumber}.</p>
        `;
        return;
      }
    } else {
      // Generate predictions based on issue number
      predictions = generatePrediction(issueNumber);
    }
    
    displayPrediction(predictions);
  } else {
            fetch('/lottery_data.json')
                .then(response => response.json())
                .then(data => {
                    const latestIssueNumber = data.latestIssueNumber;
                    const prediction = getPrediction(latestIssueNumber);
                    displayPrediction([prediction]);
                })
                .catch(error => {
                    console.error('Error fetching lottery data:', error);
                    predictionElement.innerHTML = `
                        <p class="text-lg font-bold text-red-500">Error fetching prediction. Please try again.</p>
                    `;
                });
        }

        sound.play().catch(e => {
            console.error("Error playing sound:", e);
        });
    }, 1200);
});

function displayPrediction(predictions) {
    const predictionElement = document.getElementById('vip-prediction');

    // Initialize the container for formatted predictions
    let formattedPredictions = '<div class="prediction-container">';

    predictions.forEach(prediction => {
        let formattedPrediction = '<div class="prediction-item">';

        // Extract number from the prediction
        const number = prediction.match(/\d+/);
        if (number) {
            const num = parseInt(number[0]);
            formattedPrediction += `
                <div class="prediction-cell">
                    <p><span class="prediction-label"><i class="fas fa-dice prediction-icon"></i> Number:</span> <span class="prediction-value">${num}</span></p>
                </div>`;
        }

        // Extract and format Big/Small prediction
        if (prediction.includes('Big')) {
            formattedPrediction += `
                <div class="prediction-cell">
                    <p><span class="prediction-label"><i class="fas fa-expand-arrows-alt prediction-icon"></i> Size:</span> <span class="prediction-value">Big</span></p>
                </div>`;
        } else if (prediction.includes('Small')) {
            formattedPrediction += `
                <div class="prediction-cell">
                    <p><span class="prediction-label"><i class="fas fa-compress-arrows-alt prediction-icon"></i> Size:</span> <span class="prediction-value">Small</span></p>
                </div>`;
        }

        // Extract and format colors
        const colors = prediction.match(/(Red|Green|Violet)/g);
        if (colors) {
            const colorEmojis = colors.map(getColorEmoji).join(' ');
            formattedPrediction += `
                <div class="prediction-cell">
                    <p><span class="prediction-label"><i class="fas fa-palette prediction-icon"></i> Color:</span> <span class="prediction-value">${colorEmojis}</span></p>
                </div>`;
        }

        formattedPrediction += '</div>'; // End of prediction row
        formattedPredictions += formattedPrediction;
    });

    formattedPredictions += '</div>'; // End of prediction container

    // Update the prediction display with the formatted predictions 
    predictionElement.innerHTML = formattedPredictions;
}

function getColorEmoji(color) {
    switch (color) {
        case 'Red':
            return '🔴 Red';
        case 'Green':
            return '🟢 Green';
        case 'Violet':
            return '🟣 Violet';
        default:
            return color; // Return color name with emoji if matched
    }
}



async function fetchBigDaddyData() {
    try {
        const response = await fetch('getBigDaddyData.php', {
            method: 'GET',
            headers: {
                'Accept': 'application/json'
            }
        });
        if (response.ok) {
            const data = await response.json();
            if (data.code === 0 && data.data && data.data.list) {
                return data.data.list;
            } else {
                console.error('Invalid data format from API');
                return null;
            }
        } else {
            console.error('Failed to fetch data from getBigDaddyData.php');
            return null;
        }
    } catch (error) {
        console.error('Error fetching data from getBigDaddyData.php:', error);
        return null;
    }
}



let lastFetchedIssueNumber = null;

async function fetchAndDisplayFutureIssueNumber() {
    if (isDeveloperMode && developerData) {
        updateDeveloperMode();
    } else {
        try {
            const data = await fetchBigDaddyData();
            if (data && data.length > 0) {
                const latestIssueNumber = data[0].issueNumber;
                const futureIssueNumber = calculateFutureIssueNumber(latestIssueNumber);

                if (futureIssueNumber !== lastFetchedIssueNumber) {
                    updateIssueNumber(futureIssueNumber);
                    lastFetchedIssueNumber = futureIssueNumber;
                }
            } else {
                console.error('No data received from Big Daddy API');
            }
        } catch (error) {
            console.error(`Failed to fetch data from Big Daddy API: ${error}`);
        }
    }
}

function calculateFutureIssueNumber(currentIssueNumber) {
    const numPart = parseInt(currentIssueNumber.slice(-6));
    const datePart = currentIssueNumber.slice(0, -6);
    const nextNum = (numPart + 1).toString().padStart(6, '0');
    return datePart + nextNum;
}

function updateIssueNumber(issueNumber) {
  const futureIssueNumberValueElement = document.getElementById('issue-number-value_new');

  if (futureIssueNumberValueElement) {
    if (futureIssueNumberValueElement.textContent !== issueNumber.toString()) {
      futureIssueNumberValueElement.style.transition = 'opacity 0.3s ease-out';
      futureIssueNumberValueElement.style.opacity = '0';
      setTimeout(() => {
        futureIssueNumberValueElement.textContent = issueNumber;
        futureIssueNumberValueElement.style.transition = 'opacity 0.3s ease-in';
        futureIssueNumberValueElement.style.opacity = '1';
      }, 300);
    }
  } else {
    console.error('futureIssueNumberValueElement not found');
  }

  const predictionElement = document.getElementById('vip-prediction');
  if (predictionElement) {
    predictionElement.textContent = '';
  }
}

// Function to update the VIP timer
function updateVIPTimer() {
  const vipTimerElement = document.getElementById('vip-timer-value');

  if (isDeveloperMode && developerData) {
    updateDeveloperMode();
  } else {
    const currentTime = new Date();
    const currentSeconds = currentTime.getSeconds();
    const remainingSeconds = 60 - currentSeconds;
    vipTimerElement.textContent = remainingSeconds;
  }
}

// Start the VIP timer
setInterval(updateVIPTimer, 1000);

// Handle page visibility changes
document.addEventListener('visibilitychange', function() {
  if (!document.hidden) {
    // Page is now visible
    if (isDeveloperMode && developerData) {
      updateDeveloperMode();
    } else {
      updateVIPTimer();
    }
  }
});

let developerStartTime = null;

function updateDeveloperMode() {
    if (!developerStartTime) {
        developerStartTime = Date.now();
    }

    let elapsedTime = Math.floor((Date.now() - developerStartTime) / 1000);
    let cyclesPassed = BigInt(Math.floor(elapsedTime / developerData.timerStart));

    if (developerPreviousCyclesPassed !== cyclesPassed) {
        developerIssueIndex = cyclesPassed;
        developerPreviousCyclesPassed = cyclesPassed;

        const newIssueNumber = developerData.startIssueNumber + developerIssueIndex;
        updateIssueNumber(newIssueNumber);
        clearPrediction();

        developerStartTime = Date.now() - (elapsedTime % developerData.timerStart) * 1000;
        localStorage.setItem('developerStartTime', developerStartTime);
        localStorage.setItem('developerIssueIndex', developerIssueIndex.toString());
    }

    let elapsedTimeInCycle = elapsedTime % developerData.timerStart;
    developerTimer = developerData.timerStart - elapsedTimeInCycle;

    const vipTimerElement = document.getElementById('vip-timer-value');
    vipTimerElement.textContent = developerTimer;
}




function clearPrediction() {
  const predictionElement = document.getElementById('vip-prediction');
  predictionElement.innerHTML = `
    <h2 class="text-2xl font-bold mb-4 text-center text-yellow-400">PREDICTION MENU</h2>
    <p class="text-lg font-bold text-yellow-400">Awaiting prediction request...</p>
  `;
}

function enableDeveloperMode() {
    isDeveloperMode = true;
    localStorage.setItem('isDeveloperMode', 'true');

    const dataUrl = isManualDeveloper ? 'manualdeveloper.json' : 'developer_data.json';

    fetch(dataUrl)
        .then(response => response.json())
        .then(data => {
            developerData = data;
            developerData.startIssueNumber = BigInt(developerData.startIssueNumber);
            developerTimer = data.timerStart;
            developerIssueIndex = 0n;
            developerStartTime = Date.now();
            developerPreviousCyclesPassed = -1n;
            localStorage.setItem('developerStartTime', developerStartTime);
            updateDeveloperMode();
        })
        .catch(error => {
            console.error('Error fetching developer data:', error);
        });
}

let isFirstPrediction = true;

function generatePrediction(issueNumber) {
    const predictions = [
        { number: 0, size: "Small", color: "Violet,Red" },
        { number: 1, size: "Small", color: "Green" },
        { number: 2, size: "Small", color: "Red" },
        { number: 3, size: "Small", color: "Green" },
        { number: 4, size: "Small", color: "Red" },
        { number: 5, size: "Big", color: "Violet,Green" },
        { number: 6, size: "Big", color: "Red" },
        { number: 7, size: "Big", color: "Green" },
        { number: 8, size: "Big", color: "Red" },
        { number: 9, size: "Big", color: "Green" },
    ];

    if (isFirstPrediction) {
        isFirstPrediction = false;
        // Return the desired first prediction
        return ["Big", "5", "Violet,Green"];
    } else {
        // Generate a random prediction
        const randomIndex = Math.floor(Math.random() * predictions.length);
        const prediction = predictions[randomIndex];
        return [prediction.size, prediction.number.toString(), prediction.color];
    }
}

fetchAndDisplayFutureIssueNumber();
setInterval(fetchAndDisplayFutureIssueNumber, 1000);
updateVIPTimer();
         function updateDemoNextGameTimer() {
         const indiaTime = new Date(new Date().getTime() + (330 * 60000)); // Indian time (UTC+5:30)
         const remainingSeconds = 60 - indiaTime.getSeconds(); // Seconds until the next minute
         
         const demoNextGameTimer = document.getElementById('demo-next-game-timer');
         if (demoNextGameTimer) {
         demoNextGameTimer.textContent = `Next game in - ${remainingSeconds} seconds`;
         }
         
         setTimeout(updateDemoNextGameTimer, 1000); // Ensure this function is called every second
         }
         
         updateDemoNextGameTimer(); // Call this function initially to start the timer
         
         function fetchGameData(apiUrl, containerId, refreshButtonId) {
         const dataContainer = document.getElementById(containerId);
         dataContainer.innerHTML = 'Loading...';
         animateRefreshButton(refreshButtonId);
         
         fetch(apiUrl)
         .then(response => response.json())
         .then(data => {
         const gameDataHTML = data.slice(0, 10).map(gameData => {
         const bigSmall = gameData.number > 4 ? 'Big' : 'Small';
         const colourEmojis = getColoursEmojis(gameData.colour);
         return `
         <div class="game-data">
         <p class="issue-number">${gameData.issueNumber}</p>
         <div class="number-colour">
         <p>${gameData.number}</p>
         ${colourEmojis}
         </div>
         <p class="big-small">${bigSmall}</p>
         </div>
         `;
         }).join('');
         
         dataContainer.innerHTML = gameDataHTML;
         
         // Update the latest issue number for 82 Lottery
         if (containerId === 'lottery-82-data') {
         const latestIssueNumber = data[0]?.issueNumber;
         if (latestIssueNumber) {
         document.getElementById('issue-number-value').textContent = latestIssueNumber;
         }
         }
         })
         .catch(error => {
         console.error(`Error fetching ${apiUrl} data:`, error);
         });
         }
         

         function animateRefreshButton(buttonId) {
         const refreshButton = document.getElementById(buttonId);
         refreshButton.classList.add('animate-refresh');
         setTimeout(() => {
         refreshButton.classList.remove('animate-refresh');
         }, 500);
         }
         
         function updateTimer(elementId, apiFunction) {
         const timerElement = document.getElementById(elementId);
         const currentTime = new Date();
         const currentSeconds = currentTime.getSeconds();
         const remainingSeconds = 60 - currentSeconds;
         
         timerElement.textContent = `Next game - ${remainingSeconds} seconds`;
         
         if (remainingSeconds === 0) {
         apiFunction();
         }
         }
         


         
         const body = document.body;
         
         const predictionButton = document.querySelector('[data-prediction-button]');
         const vipPredictionContainer = document.getElementById('vip-prediction-container');
         const closeVipPredictionButton = document.getElementById('close-vip-prediction');
         
         predictionButton.addEventListener('click', () => {
         vipPredictionContainer.classList.remove('hidden');
         body.classList.add('overflow-hidden');
         // Ensure demo container is hidden when prediction is shown
         demoPredictionContainer.classList.add('hidden');
         });
         
         closeVipPredictionButton.addEventListener('click', () => {
         vipPredictionContainer.classList.add('hidden');
         body.classList.remove('overflow-hidden');
         });
         
         const platformSelectionContainer = document.getElementById('platform-selection-container');
         const platformOptions = document.querySelectorAll('.platform-option');
         const platformConfirmButton = document.getElementById('platform-confirm-button');
         const vipPlatformElement = document.getElementById('vip-platform');
         const predictionContainer = document.getElementById('prediction-container');
         
         platformOptions.forEach(option => {
         option.addEventListener('click', () => {
         platformOptions.forEach(otherOption => {
             otherOption.classList.remove('selected');
             otherOption.classList.remove('bg-green-500', 'text-white');
             otherOption.classList.add('bg-blue-800', 'text-white');
         });
         option.classList.add('selected', 'bg-green-500', 'text-white');
         option.classList.remove('bg-blue-800');
         platformConfirmButton.classList.remove('hidden');
         
         // Highlight the selected button
         option.style.boxShadow = '0 0 10px #48bb78';
         option.style.transform = 'scale(1.05)';
         });
         });
         
         platformConfirmButton.addEventListener('click', () => {
         const selectedPlatform = document.querySelector('.platform-option.selected').getAttribute('data-platform');
         platformSelectionContainer.classList.add('hidden');
         
         // Set the text content of the vip-platform element
         const vipPlatformElement = document.getElementById('vip-platform');
         vipPlatformElement.textContent = selectedPlatform;
         
         // Set the platform name in the testing menu
         const platformNameElement = document.getElementById('platform-name');
         platformNameElement.textContent = selectedPlatform;
         
         // Show the testing menu container
         const testingMenuContainer = document.getElementById('testing-menu-container');
         testingMenuContainer.classList.remove('hidden');

         if (isFirstTimeVisitor()) {
           lockContinueButton();
         }
         });

         function isFirstTimeVisitor() {
           return localStorage.getItem('hasVisited') !== 'true';
         }

         function setVisited() {
           localStorage.setItem('hasVisited', 'true');
         }

function showLockedButtonMessage() {
  const existingMessageContainer = document.querySelector('.locked-button-message');
  if (existingMessageContainer) {
    existingMessageContainer.remove();
  }

  const messageContainer = document.createElement('div');
  messageContainer.className = 'locked-button-message';
  messageContainer.innerHTML = `
    <div class="message-content bg-gradient-to-br from-blue-900 to-blue-700 p-3 rounded-lg shadow-lg border border-yellow-400 max-w-xs mx-auto">
      <h3 class="text-lg font-bold text-yellow-400 mb-2 flex items-center"><span class="mr-2">🔒</span>Access Locked</h3>
      <p class="text-white text-xs mb-2">Follow these steps to unlock premium predictions:</p>
      <ol class="text-white text-xs list-decimal pl-4 mb-2 space-y-1">
        <li>Click "Start" to create a new account</li>
        <li>Deposit ₹500 or more</li>
        <li>Wait for system verification (usually instant)</li>
      </ol>
      <p class="text-white text-xs mb-2">After verification, the "Continue" button will unlock, granting you access to premium features.</p>
      <div class="bg-red-500 bg-opacity-20 p-2 rounded mb-2">
        <p class="text-red-300 text-xs font-semibold mb-1">⚠️ IMPORTANT:</p>
        <ul class="text-white text-xs list-disc pl-4 space-y-1">
          <li class="text-xxs">Only accounts created through this app receive winning predictions</li>
          <li class="text-xxs">Existing accounts will have limited functionality</li>
          <li class="text-xxs">For best results, create a new account and follow the steps above</li>
        </ul>
      </div>
      <button id="close-locked-message-${Date.now()}" class="w-full bg-yellow-400 hover:bg-yellow-500 text-blue-900 font-bold py-1 px-3 rounded-full text-xs uppercase tracking-wider transition-all duration-200 shadow-md">I Understand</button>
    </div>
  `;
  document.body.appendChild(messageContainer);

  const closeButton = messageContainer.querySelector(`[id^="close-locked-message-"]`);
  closeButton.addEventListener('click', function() {
    messageContainer.remove();
  });
}

function lockContinueButton() {
  const continueButton = document.getElementById('continue-button');
  continueButton.innerHTML = 'Continue 🔒';
  continueButton.classList.add('locked');
  continueButton.addEventListener('click', showLockedButtonMessage);
}

function unlockContinueButton() {
  const continueButton = document.getElementById('continue-button');
  continueButton.innerHTML = `
    <span class="mr-1">Continue</span>
    <svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3"></path>
    </svg>
  `;
  continueButton.classList.remove('locked');
  continueButton.removeEventListener('click', showLockedButtonMessage);
}

// Add event listener for the "Continue" button in the testing menu
document.getElementById('continue-button').addEventListener('click', () => {
  if (isFirstTimeVisitor()) {
    showLockedButtonMessage();
  } else {
    const testingMenuContainer = document.getElementById('testing-menu-container');
    testingMenuContainer.classList.add('hidden');

    // Show the VIP key container
    const vipKeyContainer = document.getElementById('vip-key-container');
    vipKeyContainer.classList.remove('hidden');
    updateVIPTimer();
  }
});



         

         function unlockContinueButtonAfterDelay() {
           setTimeout(() => {
             unlockContinueButton();
             setVisited();
           }, 90000); // 1 minute and 30 seconds
         }
         
         // Remove the predictButton event listener and related code
         
         const demoIssueNumber = document.getElementById('demo-issue-number');
         const demoNextGameTimer = document.getElementById('demo-next-game-timer');
         
         function startDemoPrediction() {
         setInterval(fetchGameData, 1000);
         }
         
         async function fetchGameData() {
         const maxAttempts = 5;
         let backoffTime = 1;
         
         const fetchLotteryData = async () => {
         try {
         const response = await fetch('/lottery_data.json');
         if (response.ok) {
         const data = await response.json();
         return data;
         } else {
         console.error(`Failed to fetch lottery data with status code ${response.status}. Retrying...`);
         await new Promise(resolve => setTimeout(resolve, backoffTime * 1000));
         backoffTime *= 2;
         return fetchLotteryData();
         }
         } catch (error) {
         console.error(`Failed to fetch lottery data with error: ${error}. Retrying...`);
         await new Promise(resolve => setTimeout(resolve, backoffTime * 1000));
         backoffTime *= 2;
         return fetchLotteryData();
         }
         };
         
         for (let attempt = 0; attempt < maxAttempts; attempt++) {
         try {
         const lotteryData = await fetchLotteryData();
         const futureIssueNumber = lotteryData.futureIssueNumber;
         
         const demoIssueNumber = document.getElementById('demo-issue-number');
         if (demoIssueNumber) {
         demoIssueNumber.textContent = futureIssueNumber;
         } else {
         console.error("demoIssueNumber element not found");
         }
         
         const currentTime = new Date();
         const currentSeconds = currentTime.getSeconds();
         const remainingSeconds = 60 - currentSeconds;
         
         const demoNextGameTimer = document.getElementById('demo-next-game-timer');
         if (demoNextGameTimer) {
         demoNextGameTimer.textContent = `Next game in - ${remainingSeconds} seconds`;
         } else {
         console.error("demoNextGameTimer element not found");
         }
         
         return;
         } catch (error) {
         console.error(`Attempt ${attempt + 1} failed with error: ${error}. Retrying...`);
         await new Promise(resolve => setTimeout(resolve, backoffTime * 1000));
         backoffTime *= 2;
         }
         }
         
         console.error("Failed to fetch game data after maximum attempts.");
         }
         

         

         const winningsWrapper = document.getElementById('winnings-wrapper');
         let lastIssueNumber = '';
         
         const indianNames = [
         'Rahul Sharma', 'Priya Patel', 'Aditya Singh', 'Neha Gupta', 'Sanjay Verma',
         'Pooja Mishra', 'Rajesh Nair', 'Aarti Reddy', 'Amit Bajaj', 'Divya Malhotra',
         'Vikram Patel', 'Mohit Gupta', 'Ankit Sharma', 'Deepak Singh', 'Gaurav Verma',
         'Rohit Nair', 'Manish Reddy', 'Nitin Bajaj', 'Siddharth Malhotra', 'Akshay Kumar',
         'Vishal Patel', 'Ravi Gupta', 'Sachin Sharma', 'Karan Singh', 'Ajay Verma',
         'Vijay Nair', 'Arjun Reddy', 'Rohan Bajaj', 'Dhruv Malhotra', 'Abhishek Kumar',
         'Nikhil Patel', 'Varun Gupta', 'Naman Sharma', 'Sanjay Singh', 'Rajeev Verma',
         'Suresh Nair', 'Praveen Reddy', 'Mohan Bajaj', 'Vikas Malhotra', 'Anuj Kumar',
         'Rajat Patel', 'Sandeep Gupta', 'Ashish Sharma', 'Pankaj Singh', 'Amit Verma',
         'Anil Nair', 'Anurag Reddy', 'Kunal Bajaj', 'Rahul Malhotra', 'Aman Kumar'
         ];
         
         function getRandomIndianName() {
         const randomIndex = Math.floor(Math.random() * indianNames.length);
         return indianNames[randomIndex];
         }
         
         function getRandomAmount() {
         return Math.floor(Math.random() * 80000) + 10000;
         }
         
         function formatAmount(amount) {
         return amount.toLocaleString('en-IN');
         }
         
         function generateWinningsBlock(name, amount, issueNumber) {
         return `
         <div class="winnings-block flex-none">
         <div class="winnings-content">
         <p class="winnings-text"><span class="winnings-name">${name}</span> has won ₹<span class="winnings-amount">${amount}</span></p>
         <p class="winnings-round">on round <span class="winnings-issue">${issueNumber}</span> using Predict Wingo</p>
         </div>
         </div>
         `;
         }
         
         function updateWinningsData(issueNumber) {
         let winningsHTML = '';
         
         for (let i = 0; i < 6; i++) {
         const name = getRandomIndianName();
         const amount = formatAmount(getRandomAmount());
         winningsHTML += generateWinningsBlock(name, amount, issueNumber);
         }
         
         winningsWrapper.innerHTML = winningsHTML;
         }
         
         function lockPlatformOptions(platform) {
         const platformOptions = document.querySelectorAll('.platform-option');
         platformOptions.forEach(option => {
         const platformData = option.getAttribute('data-platform');
         if (platformData !== platform) {
         option.classList.add('locked');
         option.disabled = true;
         option.innerHTML = `${platformData} 🔒`;
         } else {
         option.classList.remove('locked');
         option.disabled = false;
         option.innerHTML = platformData;
         }
         });
         }
         
async function fetchLatestIssueNumber() {
    try {
        const data = await fetchBigDaddyData();
        if (data && data.length > 0) {
            const currentIssueNumber = data[0].issueNumber;

            if (currentIssueNumber !== lastIssueNumber) {
                lastIssueNumber = currentIssueNumber;
                updateWinningsData(currentIssueNumber);
            }
        } else {
            console.error('No data received from Big Daddy API');
        }
    } catch (error) {
        console.error(`Failed to fetch data from Big Daddy API: ${error}`);
    }
}

         
         
         function startWinningsUpdates() {
         fetchLatestIssueNumber();
         setInterval(fetchLatestIssueNumber, 1000);
         }
         
         startWinningsUpdates();
         
         // New implementation for About Us, Disclaimer, and Warning buttons
         function setupPopup(button, containerId, closeButtonId) {
         const container = document.getElementById(containerId);
         const closeButton = document.getElementById(closeButtonId);
         
         if (button && container && closeButton) {
         button.addEventListener('click', (event) => {
         event.preventDefault();
         event.stopPropagation();
         container.classList.remove('hidden');
         body.classList.add('overflow-hidden');
         });
         
         closeButton.addEventListener('click', () => {
         container.classList.add('hidden');
         body.classList.remove('overflow-hidden');
         });
         
         // Close popup when clicking outside the content
         container.addEventListener('click', (event) => {
         if (event.target === container) {
         container.classList.add('hidden');
         body.classList.remove('overflow-hidden');
         }
         });
         }
         }
         
         // Setup popups
         setupPopup(document.querySelector('[data-about-us-button]'), 'about-us-container', 'close-about-us');
         setupPopup(document.querySelector('[data-disclaimer-button]'), 'disclaimer-container', 'close-disclaimer');
         setupPopup(document.querySelector('[data-warning-button]'), 'warning-container', 'close-warning');
         
         // Trends button setup
         const trendsButton = document.querySelector('[data-trends-button]');
         const trendsContainer = document.getElementById('trends-container');
         const closeTrendsButton = document.getElementById('close-trends');
         const trendsConfirmButton = document.getElementById('trends-confirm-button');
         const trendsPlatformOptions = document.querySelectorAll('.trends-platform-option');
         const trendsReportContainer = document.getElementById('trends-report-container');
         const closeTrendsReportButton = document.getElementById('close-trends-report');
         const trendsPlatformName = document.getElementById('trends-platform-name');

         trendsButton.addEventListener('click', () => {
             trendsContainer.classList.remove('hidden');
             body.classList.add('overflow-hidden');
         });

         closeTrendsButton.addEventListener('click', () => {
             trendsContainer.classList.add('hidden');
             body.classList.remove('overflow-hidden');
         });

trendsPlatformOptions.forEach(option => {
    option.addEventListener('click', () => {
        trendsPlatformOptions.forEach(opt => {
            opt.classList.remove('selected');
            opt.classList.remove('bg-green-500', 'text-white');
            opt.classList.add('bg-yellow-400', 'text-blue-900');
        });
        option.classList.add('selected');
        option.classList.remove('bg-yellow-400', 'text-blue-900');
        option.classList.add('bg-green-500', 'text-white');
        trendsConfirmButton.classList.remove('hidden');
    });
});

         trendsConfirmButton.addEventListener('click', () => {
             const selectedPlatform = document.querySelector('.platform-option.selected');
             if (selectedPlatform) {
                 const platformName = selectedPlatform.getAttribute('data-platform');
                 trendsContainer.classList.add('hidden');
                 trendsReportContainer.classList.remove('hidden');
                 
                 trendsPlatformName.textContent = platformName;

                 // Start updating trends based on selected platform
                 switch(platformName) {
                     case '91 Club':
                         startTrendsUpdate('https://predictwins.in/cors_proxy.php?file=91_club.txt');
                         break;
                     case '82 Lottery':
                         startTrendsUpdate('https://predictwins.in/cors_proxy.php?file=82_lottery.txt');
                         break;
                     case 'Big Daddy':
                         startTrendsUpdate('https://predictwins.in/cors_proxy.php?file=big_daddy.txt');
                         break;
                     case 'TC Lottery':
                         startTrendsUpdate('https://predictwins.in/tc_lottery.txt');
                         break;
                     case 'Lucknow Games':
                         startTrendsUpdate('https://predictwins.in/cors_proxy.php?file=lucknow.txt');
                         break;
                     case 'OKWin':
                         startTrendsUpdate('https://predictwins.in/cors_proxy.php?file=okwin.txt');
                         break;
                     default:
                         console.error('Unknown platform selected');
                 }
             } else {
                 console.error('No platform selected');
             }
         });
         
   

         // Show/hide confirm button based on platform selection
         document.querySelectorAll('.platform-option').forEach(option => {
             option.addEventListener('click', () => {
                 document.querySelectorAll('.platform-option').forEach(opt => opt.classList.remove('selected'));
                 option.classList.add('selected');
                 trendsConfirmButton.classList.remove('hidden');
             });
         });

         closeTrendsReportButton.addEventListener('click', () => {
             trendsReportContainer.classList.add('hidden');
             body.classList.remove('overflow-hidden');
         });

         // Trends functionality
         let selectedPlatformUrl = '';
         let trendsUpdateInterval;

         let lastTrendsData = null;

function updateTrendsUI(data) {
    const trendsDataContainer = document.getElementById('trends-data-container');
    if (!trendsDataContainer) {
        console.error('Trends data container not found');
        return;
    }
    
    if (!Array.isArray(data)) {
        console.error('Invalid data format:', data);
        trendsDataContainer.innerHTML = '<p class="text-red-500">Error: Invalid data received</p>';
        return;
    }

    const newContent = data.map(gameData => {
        let period, number, bs, colour;

        if (gameData.hasOwnProperty('period')) {
            // Lucknow Games format
            period = gameData.period;
            number = gameData.open_num[0];
            bs = gameData.bs;
            colour = gameData.colour;
        } else {
            // Other games format
            period = gameData.issueNumber;
            number = gameData.number;
            bs = number > 4 ? 'Big' : 'Small';
            colour = gameData.colour;
        }

        const colourEmojis = getColoursEmojis(colour);
        
        return `
        <div class="game-data flex justify-between items-center border-b border-blue-700 py-2">
            <p class="issue-number text-yellow-400">${period}</p>
            <div class="number-colour flex items-center">
                <p class="mr-2">${number}</p>
                <p>${colourEmojis}</p>
            </div>
            <p class="big-small ${bs === 'Big' ? 'text-green-400' : 'text-red-400'}">${bs}</p>
        </div>
        `;
    }).join('');
    
    const tempContainer = document.createElement('div');
    tempContainer.innerHTML = newContent;
    
    trendsDataContainer.innerHTML = '';
    trendsDataContainer.appendChild(tempContainer);
}

         function startTrendsUpdate(url) {
             clearInterval(trendsUpdateInterval);
             selectedPlatformUrl = url;
             
             async function fetchTrendsData() {
                 try {
                     const response = await fetch(selectedPlatformUrl);
                     const data = await response.json();
                     updateTrendsUI(data);
                 } catch (error) {
                     console.error('Error fetching trends data:', error);
                 }
             }

             fetchTrendsData(); // Fetch immediately
             trendsUpdateInterval = setInterval(fetchTrendsData, 5000); // Update every 5 seconds
         }

document.addEventListener('DOMContentLoaded', function() {
    const closeWelcomePopupButton = document.getElementById('close-welcome-popup');
    const welcomePopup = document.getElementById('welcome-popup');

    // Show the welcome popup every time the page is refreshed
    welcomePopup.classList.remove('hidden');
    document.body.classList.add('overflow-hidden');

    closeWelcomePopupButton.addEventListener('click', closeWelcomePopup);
});

function closeWelcomePopup() {
    const welcomePopup = document.getElementById('welcome-popup');
    welcomePopup.classList.add('hidden');
    document.body.classList.remove('overflow-hidden');
}

         function getColoursEmojis(colour) {
             const colors = colour.toLowerCase().split(',');
             return colors.map(color => {
                 switch(color.trim()) {
                     case 'red':
                         return '🔴';
                     case 'green':
                         return '🟢';
                     case 'violet':
                         return '🟣';
                     default:
                         return ''; // Return empty string for unknown colors
                 }
             }).join('');
         }


         // Demo button setup
         const demoButton = document.querySelector('[data-demo-button]');
         const demoContainer = document.createElement('div');
         demoContainer.id = 'demo-container';
         demoContainer.classList.add('popup-container', 'hidden');
         demoContainer.innerHTML = `
         <div class="popup-content bg-gradient-to-br from-blue-900 to-blue-700 rounded-lg shadow-2xl max-w-md mx-auto">
         <button id="close-demo" class="close-popup">×</button>
         <h2 class="text-3xl font-bold mb-6 text-center text-white-400 glow-shadow-stroke">DEMO MENU</h2>
         <div class="demo-scroll-container">
         <div class="demo-scroll-content space-y-4">
         <div class="bg-blue-800 bg-opacity-50 p-4 rounded-lg shadow-inner">
         <p class="text-lg mb-1">Future Issue Number: <span id="demo-issue-number" class="font-bold text-yellow-400"></span></p>
         <p id="demo-next-game-timer" class="text-md text-blue-200"></p>
         </div>
         <div id="demo-prediction" class="bg-blue-800 bg-opacity-50 p-4 rounded-lg shadow-inner prediction-display">
         <h3 class="text-xl font-bold mb-2 text-center text-yellow-400">Prediction</h3>
         <p class="text-center text-md text-blue-200 blur-sm">Number: <span class="font-bold">5</span></p>
         <p class="text-center text-md text-blue-200 blur-sm">Color: <span class="font-bold">Red</span></p>
         <p class="text-center text-md text-blue-200 blur-sm">Size: <span class="font-bold">Big</span></p>
         </div>
         <div class="bg-yellow-400 bg-opacity-20 p-3 rounded-lg">
         <p class="text-center text-white text-sm">With the prediction, you'll see the upcoming game's prediction, which is 100% accurate before the game starts, guaranteeing no losses.</p>
         </div>
         </div>
         </div>
         </div>
         `;
         document.body.appendChild(demoContainer);
         
         const closeDemoButton = document.getElementById('close-demo');
         
         demoButton.addEventListener('click', () => {
         demoContainer.classList.remove('hidden');
         body.classList.add('overflow-hidden');
         startDemoPrediction();
         });
         
         closeDemoButton.addEventListener('click', () => {
         demoContainer.classList.add('hidden');
         body.classList.remove('overflow-hidden');
         });
         // Removed duplicate event listeners for warning button
         
         const proofButton = document.querySelector('.button[data-proof-button]');
         const proofContainer = document.getElementById('proof-container');
         const closeProofButton = document.getElementById('close-proof');
             
         proofButton.addEventListener('click', () => {
         proofContainer.classList.remove('hidden');
         body.classList.add('overflow-hidden');
         });
         
         closeProofButton.addEventListener('click', () => {
         proofContainer.classList.add('hidden');
         body.classList.remove('overflow-hidden');
         });
         
         startDemoPrediction();
      </script>
      <script>
        // VIP button setup
        const vipButton = document.querySelector('[data-vip-button]');
        const accuracyContainer = document.getElementById('accuracy-container');
        const closeAccuracyButton = document.getElementById('close-accuracy');
        const vipPlatformConfirmButton = document.getElementById('vip-platform-confirm-button');
        const vipPlatformOptions = document.querySelectorAll('#vip-platform-options .platform-option');

        vipButton.addEventListener('click', () => {
            accuracyContainer.classList.remove('hidden');
            body.classList.add('overflow-hidden');
        });

        closeAccuracyButton.addEventListener('click', () => {
            accuracyContainer.classList.add('hidden');
            body.classList.remove('overflow-hidden');
        });

        vipPlatformOptions.forEach(option => {
            option.addEventListener('click', () => {
                vipPlatformOptions.forEach(opt => {
                    opt.classList.remove('selected');
                    opt.classList.remove('bg-green-500', 'text-white');
                    opt.classList.add('bg-blue-800', 'text-white');
                });
                option.classList.add('selected', 'bg-green-500', 'text-white');
                option.classList.remove('bg-blue-800');
                vipPlatformConfirmButton.classList.remove('hidden');
            });
        });

let selectedPlatformName = '';

vipPlatformConfirmButton.addEventListener('click', () => {
    const selectedPlatform = document.querySelector('#vip-platform-options .platform-option.selected');
    if (selectedPlatform) {
        selectedPlatformName = selectedPlatform.getAttribute('data-platform');
        accuracyContainer.classList.add('hidden');
        // Show the new container for prediction type selection
        const predictionTypeContainer = document.createElement('div');
        predictionTypeContainer.classList.add('popup-container');
        predictionTypeContainer.innerHTML = `
            <div class="popup-content">
                <button class="close-popup">×</button>
                <h2 class="text-2xl font-bold mb-4 text-center text-yellow-400">GAME TYPE</h2>
                <p class="text-center text-white mb-4">Selected Platform: <span class="font-bold text-yellow-400">${selectedPlatformName}</span></p>
                <div class="platform-options">
                    <button class="platform-option bg-gradient-to-r from-blue-600 to-blue-800 text-white hover:from-blue-700 hover:to-blue-900 transition-all duration-300 py-3 px-6 rounded-lg mb-3 w-full shadow-lg transform hover:scale-105" data-type="Big/Small">
                        <span class="text-lg font-bold">BIG / SMALL</span>
                        <span class="block text-xs mt-1 text-blue-200">Predict the size</span>
                    </button>
                    <button class="platform-option bg-gradient-to-r from-purple-600 to-pink-600 text-white hover:from-purple-700 hover:to-pink-700 transition-all duration-300 py-3 px-6 rounded-lg mb-3 w-full shadow-lg transform hover:scale-105" data-type="Colour/🔴🟢🟣">
                        <span class="text-lg font-bold">COLOUR 🔴🟢🟣</span>
                        <span class="block text-xs mt-1 text-pink-200">Predict the color</span>
                    </button>
                    <button class="platform-option bg-gradient-to-r from-yellow-500 to-orange-500 text-white hover:from-yellow-600 hover:to-orange-600 transition-all duration-300 py-3 px-6 rounded-lg mb-3 w-full shadow-lg transform hover:scale-105" data-type="Number">
                        <span class="text-lg font-bold">NUMBER ⚡️</span>
                        <span class="block text-xs mt-1 text-yellow-100">Predict the exact number</span>
                    </button>
                    <button id="prediction-type-confirm-button" class="platform-confirm-button hidden bg-gradient-to-r from-green-500 to-emerald-600 hover:from-green-600 hover:to-emerald-700 text-white font-bold py-3 px-6 rounded-lg mt-4 w-full shadow-lg transform hover:scale-105 transition-all duration-300">
                        <span class="text-lg">Confirm Selection</span>
                    </button>
                </div>
            </div>
        `;
        document.body.appendChild(predictionTypeContainer);
        body.classList.add('overflow-hidden');

        const closePredictionTypeButton = predictionTypeContainer.querySelector('.close-popup');
        closePredictionTypeButton.addEventListener('click', () => {
            predictionTypeContainer.remove();
            body.classList.remove('overflow-hidden');
        });
        const predictionTypeConfirmButton = document.getElementById('prediction-type-confirm-button');
        const predictionTypeOptions = document.querySelectorAll('.platform-option');

        predictionTypeOptions.forEach(option => {
            option.addEventListener('click', () => {
                predictionTypeOptions.forEach(opt => {
                    opt.classList.remove('selected');
                    opt.classList.remove('bg-green-500', 'text-white');
                    opt.classList.add('bg-blue-800', 'text-white');
                });
                option.classList.add('selected', 'bg-green-500', 'text-white');
                option.classList.remove('bg-blue-800');
                predictionTypeConfirmButton.classList.remove('hidden');
            });
        });

        predictionTypeConfirmButton.addEventListener('click', () => {
            const selectedPredictionType = document.querySelector('.platform-option.selected').getAttribute('data-type');
            predictionTypeContainer.classList.add('hidden');
            // Show the new container for key entry
            const keyEntryContainer = document.createElement('div');
            keyEntryContainer.classList.add('popup-container');
            keyEntryContainer.innerHTML = `
                <div class="popup-content bg-gradient-to-br from-blue-900 to-indigo-900 rounded-lg p-6 shadow-2xl max-w-md mx-auto">
                    <button class="close-popup text-white hover:text-yellow-400 transition-colors duration-300">×</button>
                    <div class="mb-4 text-center">
                        <svg class="inline-block w-20 h-20" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
                            <circle cx="50" cy="50" r="48" fill="#4CAF50"/>
                            <path d="M30 50 L45 65 L70 35" stroke="white" stroke-width="10" fill="none" stroke-linecap="round" stroke-linejoin="round"/>
                            <circle cx="50" cy="50" r="48" fill="none" stroke="#81C784" stroke-width="4"/>
                        </svg>
                    </div>
                    <h2 class="text-2xl font-bold mb-4 text-center text-yellow-400 glow-text">License Check</h2>
                    <div class="bg-blue-800 bg-opacity-30 p-4 rounded-lg mb-6 shadow-inner">
                        <div class="flex justify-between items-center mb-2">
                            <p class="text-sm text-blue-200">Platform:</p>
                            <p class="text-sm font-bold text-yellow-400" id="selected-platform-name"></p>
                        </div>
                        <div class="flex justify-between items-center">
                            <p class="text-sm text-blue-200">Game Type:</p>
                            <p class="text-sm font-bold text-yellow-400" id="selected-game-type"></p>
                        </div>
                    </div>
                    <input type="text" id="user-key-input" class="key-input mb-2" placeholder="Enter your license">
                    <button id="submit-user-key" class="confirm-button mb-2">Submit</button>
                    <button id="obtain-license" class="bg-yellow-400 hover:bg-yellow-500 text-blue-900 font-bold py-2 px-4 rounded-full text-sm uppercase tracking-wider transition-all duration-200 w-full">Obtain License</button>
                    <p id="user-key-error" class="text-red-500 mt-2 hidden">Incorrect code. Please try again.</p>
                </div>
            `;
            document.body.appendChild(keyEntryContainer);
            body.classList.add('overflow-hidden');

            const pricingContainer = document.createElement('div');
pricingContainer.classList.add('popup-container', 'hidden');
pricingContainer.innerHTML = `
<div class="popup-content bg-gradient-to-br from-blue-900 to-indigo-900 rounded-lg p-8 shadow-2xl max-w-5xl mx-auto">
    <button class="close-popup text-white hover:text-yellow-400 transition-colors duration-300 absolute top-4 right-4 text-2xl">×</button>
    <h2 class="text-3xl font-bold mb-8 text-center text-yellow-400 glow-text">WIN & GO</h2>
    
    <div class="mb-6 bg-gradient-to-r from-blue-600 to-indigo-600 rounded-lg shadow-lg overflow-hidden">
        <div class="flex flex-col items-center p-4 md:p-6">
            <h3 class="text-2xxl md:text-3xl font-bold mb-3 text-transparent bg-clip-text bg-gradient-to-r from-yellow-300 to-yellow-500 leading-tight text-center">
                100% Winning Power
            </h3> 
            <p class="text-white text-base md:text-lg mb-4 text-center">Experience game-changing predictions that boost your wins!</p>
            <a href="https://youtu.be/bqxS77Cj3UM?si=cORbf9_fW9ipNpTV" target="_blank" class="bg-yellow-400 hover:bg-yellow-500 text-blue-900 font-bold py-3 px-6 rounded-full text-sm md:text-base uppercase tracking-wider transition-all duration-300 inline-flex items-center transform hover:scale-105 shadow-md hover:shadow-lg">
                <span class="mr-2">Watch Demo</span>
                <span class="text-2xl">▶️</span>
            </a>
        </div>
    </div>

    <h3 class="text-2xxl font-bold mb-6 text-center text-white">Choose Your Winning Plan</h3>
        <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
            <div class="plan-card bg-blue-800 bg-opacity-50 p-6 rounded-lg shadow-lg transform hover:scale-105 transition-all duration-300">
                <div class="flex justify-center items-center w-20 h-20 mx-auto mb-4 bg-yellow-400 rounded-full">
                    <svg class="w-12 h-12 text-blue-900" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path></svg>
                </div>
                <h3 class="text-2xl font-bold mb-2 text-center text-yellow-400">Pro</h3>
                <p class="text-center text-3xl font-bold mb-4">₹2999 <span class="text-sm text-blue-300">/ 38 USDT</span></p>
                <ul class="text-sm mb-6 space-y-3">
                    <li class="flex items-center"><svg class="w-5 h-5 mr-2 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg><span><strong class="text-yellow-400">9</strong> 100% accurate predictions daily</span></li>
                    <li class="flex items-center"><svg class="w-5 h-5 mr-2 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg><span><strong class="text-yellow-400">3</strong> each: Big/Small, Number, Colour</span></li>
                    <li class="flex items-center"><svg class="w-5 h-5 mr-2 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg><span>Fast customer support</span></li>
                    <li class="flex items-center"><svg class="w-5 h-5 mr-2 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg><span>Valid for <strong class="text-yellow-400">9 days</strong></span></li>
                </ul>
                <p class="text-xs text-blue-300 mb-4 text-center">Perfect for beginners looking to test the waters</p>
                <a href="https://t.me/vTrustPay_bot" target="_blank" class="bg-yellow-400 hover:bg-yellow-500 text-blue-900 font-bold py-3 px-6 rounded-full text-sm uppercase tracking-wider transition-all duration-200 w-full transform hover:scale-105 hover:shadow-lg inline-block text-center">Join Now</a>
            </div>
            <div class="plan-card bg-blue-800 bg-opacity-50 p-6 rounded-lg shadow-lg transform scale-105 border-2 border-yellow-400 relative">
                <div class="absolute top-0 right-0 bg-yellow-400 text-blue-900 px-4 py-1 text-xs font-bold uppercase rounded-bl-lg">Most Popular</div>
                <div class="flex justify-center items-center w-20 h-20 mx-auto mb-4 bg-yellow-400 rounded-full">
                    <svg class="w-12 h-12 text-blue-900" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z"></path></svg>
                </div>
                <h3 class="text-2xl font-bold mb-2 text-center text-yellow-400">Legend</h3>
                <p class="text-center text-3xl font-bold mb-4">₹4499 <span class="text-sm text-blue-300">/ 48 USDT</span></p>
                <ul class="text-sm mb-6 space-y-3">
                    <li class="flex items-center"><svg class="w-5 h-5 mr-2 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg><span><strong class="text-yellow-400">48</strong> 100% accurate predictions daily</span></li>
                    <li class="flex items-center"><svg class="w-5 h-5 mr-2 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg><span><strong class="text-yellow-400">15</strong> each: Big/Small, Colour, <strong class="text-yellow-400">18</strong> Number</span></li>
                    <li class="flex items-center"><svg class="w-5 h-5 mr-2 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg><span><strong class="text-yellow-400">No loss guarantee</strong></span></li>
                    <li class="flex items-center"><svg class="w-5 h-5 mr-2 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg><span>Valid for <strong class="text-yellow-400">21 days</strong></span></li>
                </ul>
                <p class="text-xs text-blue-300 mb-4 text-center">Best value for serious players aiming for consistent wins</p>
                <a href="https://t.me/vTrustPay_bot" target="_blank" class="bg-yellow-400 hover:bg-yellow-500 text-blue-900 font-bold py-3 px-6 rounded-full text-sm uppercase tracking-wider transition-all duration-200 w-full transform hover:scale-105 hover:shadow-lg inline-block text-center">Join Now</a>
            </div>
            <div class="plan-card bg-blue-800 bg-opacity-50 p-6 rounded-lg shadow-lg transform hover:scale-105 transition-all duration-300">
                <div class="flex justify-center items-center w-20 h-20 mx-auto mb-4 bg-yellow-400 rounded-full">
                    <svg class="w-12 h-12 text-blue-900" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 6l3 1m0 0l-3 9a5.002 5.002 0 006.001 0M6 7l3 9M6 7l6-2m6 2l3-1m-3 1l-3 9a5.002 5.002 0 006.001 0M18 7l3 9m-3-9l-6-2m0-2v2m0 16V5m0 16H9m3 0h3"></path></svg>
                </div>
                <h3 class="text-2xl font-bold mb-2 text-center text-yellow-400">Ninja</h3>
                <p class="text-center text-3xl font-bold mb-4">₹6999 <span class="text-sm text-blue-300">/ 89 USDT</span></p>
                <ul class="text-sm mb-6 space-y-3">
                    <li class="flex items-center"><svg class="w-5 h-5 mr-2 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg><span><strong class="text-yellow-400">100</strong> 100% accurate predictions daily</span></li>
                    <li class="flex items-center"><svg class="w-5 h-5 mr-2 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg><span><strong class="text-yellow-400">30</strong> each: Big/Small, Colour, <strong class="text-yellow-400">40</strong> Number</span></li>
                    <li class="flex items-center"><svg class="w-5 h-5 mr-2 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg><span><strong class="text-yellow-400">VIP priority support</strong></span></li>
                    <li class="flex items-center"><svg class="w-5 h-5 mr-2 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg><span>Valid for <strong class="text-yellow-400">48 days</strong></span></li>
                </ul>
                <p class="text-xs text-blue-300 mb-4 text-center">For the elite players who demand the absolute best</p>
                <a href="https://t.me/vTrustPay_bot" target="_blank" class="bg-yellow-400 hover:bg-yellow-500 text-blue-900 font-bold py-3 px-6 rounded-full text-sm uppercase tracking-wider transition-all duration-200 w-full transform hover:scale-105 hover:shadow-lg inline-block text-center">Join Now</a>
            </div>
        </div>
<div class="mt-8 text-center">
    <div class="bg-gradient-to-r from-blue-600 to-indigo-600 p-4 rounded-lg shadow-lg max-w-md mx-auto">
        <h3 class="text-xl font-bold text-yellow-400 mb-2">Get Your License Key</h3>
        <p class="text-white text-sm mb-4">Purchase your license key through our secure Telegram bot.</p>
        <div class="flex justify-center space-x-4">
            <a href="https://t.me/vTrustPay_bot" target="_blank" class="bg-yellow-400 hover:bg-yellow-500 text-blue-900 font-bold py-1.5 px-4 rounded-full text-xs uppercase tracking-wider transition-all duration-200 inline-flex items-center justify-center">
                <svg class="w-4 h-4 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8"></path></svg>
                License Key
            </a>
            <a href="https://youtu.be/Mi-kF5jMKDM?si=dSrUOccgre2Sg6m-" target="_blank" class="bg-red-500 hover:bg-red-600 text-white font-bold py-1.5 px-4 rounded-full text-xs uppercase tracking-wider transition-all duration-200 inline-flex items-center justify-center">
                <svg class="w-4 h-4 mr-1" fill="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M19.615 3.184c-3.604-.246-11.631-.245-15.23 0-3.897.266-4.356 2.62-4.385 8.816.029 6.185.484 8.549 4.385 8.816 3.6.245 11.626.246 15.23 0 3.897-.266 4.356-2.62 4.385-8.816-.029-6.185-.484-8.549-4.385-8.816zm-10.615 12.816v-8l8 3.993-8 4.007z"></path></svg>
                Help
            </a>
        </div>
    </div>
</div>
`;
document.body.appendChild(pricingContainer);

            // Add event listener for the "Obtain License" button
            const obtainLicenseButton = document.getElementById('obtain-license');
            obtainLicenseButton.addEventListener('click', () => {
                keyEntryContainer.classList.add('hidden');
                pricingContainer.classList.remove('hidden');
            });

            // Add event listener for the close button in the pricing container
            const closePricingButton = pricingContainer.querySelector('.close-popup');
            closePricingButton.addEventListener('click', () => {
                pricingContainer.classList.add('hidden');
                keyEntryContainer.classList.remove('hidden');
            });

            const closeKeyEntryButton = keyEntryContainer.querySelector('.close-popup');
            closeKeyEntryButton.addEventListener('click', () => {
                keyEntryContainer.remove();
                body.classList.remove('overflow-hidden');
            });

            const submitUserKeyButton = document.getElementById('submit-user-key');
            const userKeyInput = document.getElementById('user-key-input');
            const userKeyError = document.getElementById('user-key-error');
            const userKey = '098123'; // Example key, replace with actual logic

            // Set the selected platform and game type in the license menu
            document.getElementById('selected-platform-name').textContent = selectedPlatformName;
            document.getElementById('selected-game-type').textContent = selectedPredictionType;

            submitUserKeyButton.addEventListener('click', () => {
                if (userKeyInput.value.trim() === '') {
                    userKeyError.textContent = 'Please enter license first!';
                    userKeyError.classList.remove('hidden');
                    return;
                }

                // Show loading animation within the key entry container
                const loadingMessage = document.createElement('div');
                loadingMessage.className = 'loading-message';
                loadingMessage.innerHTML = `
                    <div class="loading-content bg-gradient-to-br from-blue-900 to-blue-700 p-6 rounded-lg shadow-lg border border-yellow-400 max-w-xs mx-auto animate-pulse">
                        <div class="flex justify-center mb-2">
                            <svg class="w-12 h-12 text-yellow-400 animate-spin" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                                <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                            </svg>
                        </div>
                        <h3 class="text-lg font-bold text-yellow-400 mb-2 text-center">Checking...</h3>
                        <p class="text-white text-xs mb-2 text-center">Please wait while we validate your license.</p>
                    </div>
                `;
                keyEntryContainer.querySelector('.popup-content').appendChild(loadingMessage);

                // Simulate server check and file read
                setTimeout(() => {
                    fetch('used_userkey.txt')
                        .then(response => response.text())
                        .then(usedKeys => {
                            loadingMessage.remove();
                            const usedKeysList = usedKeys.split('\n').map(key => key.trim());
                            
                            if (usedKeysList.includes(userKeyInput.value)) {
                                userKeyError.textContent = 'This license key has already been used.';
                                userKeyError.classList.remove('hidden');
                            } else if (userKeyInput.value === userKey) {
                                keyEntryContainer.remove();
                                body.classList.remove('overflow-hidden');
                                
                                // Enable developer mode and show the container
                                isDeveloperMode = true;
                                const vipKeyContainer = document.getElementById('vip-key-container');
                                vipKeyContainer.classList.remove('hidden');
                                
                                // Set the platform name in the developer mode container
                                const vipPlatformElement = document.getElementById('vip-platform');
                                vipPlatformElement.textContent = selectedPlatformName;
                                
          

                                // Initialize developer mode data
                                fetch(`developer_data.json`)
                                    .then(response => response.json())
                                    .then(data => {
                                        developerData = data;
                                        developerTimer = data.timerStart;
                                        developerIssueIndex = 0;
                                        updateDeveloperMode();
                                    })
                                    .catch(error => {
                                        console.error('Error fetching developer data:', error);
                                    });
                                
                                // Hide the VIP prediction container if it's visible
                                const vipPredictionContainer = document.getElementById('vip-prediction-container');
                                if (vipPredictionContainer) {
                                    vipPredictionContainer.classList.add('hidden');
                                }

                                // Add the used key to the file (this would typically be done server-side)
                                // For this example, we're just simulating it
                                console.log('Key used:', userKeyInput.value);
                            } else {
                                userKeyError.textContent = 'Incorrect code. Please try again.';
                                userKeyError.classList.remove('hidden');
                            }
                        })
                        .catch(error => {
                            console.error('Error checking used keys:', error);
                            loadingMessage.remove();
                            userKeyError.textContent = 'An error occurred. Please try again.';
                            userKeyError.classList.remove('hidden');
                        });
                }, 2000); // Simulate a 2-second server check
            });
        });
    }
});
      </script>
      <script>
        document.addEventListener('DOMContentLoaded', function() {
          const startButton = document.getElementById('start-button');
          const platformName = document.getElementById('platform-name');

          function updateStartButtonHref() {
            const selectedPlatform = platformName.textContent.trim();
            let referralLink = '#';

            switch(selectedPlatform) {
              case '91 Club':
                referralLink = 'https://91club.com/58jfks';
                break;
              case '82 Lottery':
                referralLink = 'https://82bet.com/#/register?invitationCode=11554503030';
                break;
              case '51 Game':
                referralLink = 'https://51game9.com/#/register?invitationCode=465611303863';
                break;
              case 'OK-Win':
                referralLink = 'https://okwin.game/#/register?invitationCode=116261778244';
                break;
            }

            startButton.href = referralLink;
          }

          // Update the start button href when the platform is selected
          const platformConfirmButton = document.getElementById('platform-confirm-button');
          platformConfirmButton.addEventListener('click', updateStartButtonHref);

          startButton.addEventListener('click', () => {
            if (isFirstTimeVisitor()) {
              unlockContinueButtonAfterDelay();
            }
          });
        });
      </script>
      <script>
        particlesJS.load('particles-js', 'particles.json', function() {
          console.log('particles.js loaded - callback');
        });
      </script>
   
</main><div id="demo-container" class="popup-container hidden">
         <div class="popup-content bg-gradient-to-br from-blue-900 to-blue-700 rounded-lg shadow-2xl max-w-md mx-auto">
         <button id="close-demo" class="close-popup">×</button>
         <h2 class="text-3xl font-bold mb-6 text-center text-white-400 glow-shadow-stroke">DEMO MENU</h2>
         <div class="demo-scroll-container">
         <div class="demo-scroll-content space-y-4">
         <div class="bg-blue-800 bg-opacity-50 p-4 rounded-lg shadow-inner">
         <p class="text-lg mb-1">Future Issue Number: <span id="demo-issue-number" class="font-bold text-yellow-400"></span></p>
         <p id="demo-next-game-timer" class="text-md text-blue-200"></p>
         </div>
         <div id="demo-prediction" class="bg-blue-800 bg-opacity-50 p-4 rounded-lg shadow-inner prediction-display">
         <h3 class="text-xl font-bold mb-2 text-center text-yellow-400">Prediction</h3>
         <p class="text-center text-md text-blue-200 blur-sm">Number: <span class="font-bold">5</span></p>
         <p class="text-center text-md text-blue-200 blur-sm">Color: <span class="font-bold">Red</span></p>
         <p class="text-center text-md text-blue-200 blur-sm">Size: <span class="font-bold">Big</span></p>
         </div>
         <div class="bg-yellow-400 bg-opacity-20 p-3 rounded-lg">
         <p class="text-center text-white text-sm">With the prediction, you'll see the upcoming game's prediction, which is 100% accurate before the game starts, guaranteeing no losses.</p>
         </div>
         </div>
         </div>
         </div>
         </div></body></html>