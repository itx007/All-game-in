<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Redirect to Poki Games</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        .glass-panel {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
        }
        .countdown-number {
            font-variant-numeric: tabular-nums;
        }
        /* Smooth pulse animation for the countdown */
        @keyframes pulse-soft {
            0%, 100% { transform: scale(1); opacity: 1; }
            50% { transform: scale(1.05); opacity: 0.8; }
        }
        .animate-pulse-soft {
            animation: pulse-soft 1s infinite;
        }
        .progress-bar {
            transition: width 1s linear;
        }
    </style>
</head>
<body class="bg-gradient-to-br from-indigo-500 via-purple-500 to-pink-500 min-h-screen flex items-center justify-center p-4">

    <!-- Main Container -->
    <div class="glass-panel w-full max-w-md rounded-2xl p-8 text-center border border-white/20 relative overflow-hidden">
        
        <!-- Decorative Background Elements inside card -->
        <div class="absolute top-0 left-0 w-full h-2 bg-gradient-to-r from-blue-500 via-purple-500 to-pink-500"></div>
        
        <!-- Icon -->
        <div class="mb-6 flex justify-center">
            <div class="w-16 h-16 bg-indigo-100 rounded-full flex items-center justify-center text-indigo-600 shadow-inner">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M13 10V3L4 14h7v7l9-11h-7z" />
                </svg>
            </div>
        </div>

        <!-- Heading -->
        <h1 class="text-2xl font-bold text-gray-800 mb-2">Leaving Site</h1>
        <p class="text-gray-500 text-sm mb-8">You are being redirected to an external destination.</p>

        <!-- Countdown Display -->
        <div id="countdown-container" class="mb-8">
            <div class="text-5xl font-bold text-indigo-600 mb-2 countdown-number animate-pulse-soft" id="timer-display">5</div>
            <p class="text-gray-600 font-medium" id="timer-text">Redirecting in 5 seconds</p>
            
            <!-- Progress Bar -->
            <div class="w-full bg-gray-200 rounded-full h-1.5 mt-4 overflow-hidden">
                <div id="progress-bar" class="bg-indigo-600 h-1.5 rounded-full progress-bar" style="width: 100%"></div>
            </div>
        </div>

        <!-- Cancelled Message (Hidden by default) -->
        <div id="cancelled-message" class="hidden mb-8 py-4">
            <div class="inline-flex items-center justify-center w-12 h-12 rounded-full bg-red-100 mb-3">
                <svg class="w-6 h-6 text-red-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                </svg>
            </div>
            <h2 class="text-xl font-bold text-gray-800">Redirect Cancelled</h2>
            <p class="text-gray-500 text-sm mt-1">You have chosen to stay on this page.</p>
        </div>

        <!-- Action Buttons -->
        <div class="grid grid-cols-2 gap-4" id="button-container">
            <button id="cancel-btn" class="px-4 py-3 rounded-xl border border-gray-200 text-gray-700 font-semibold hover:bg-gray-50 hover:border-gray-300 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-gray-200">
                Cancel
            </button>
            <button id="redirect-btn" class="px-4 py-3 rounded-xl bg-indigo-600 text-white font-semibold shadow-lg shadow-indigo-200 hover:bg-indigo-700 hover:shadow-indigo-300 transition-all duration-200 transform hover:-translate-y-0.5 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2">
                Redirect Now
            </button>
        </div>

        <!-- Reset Button (Hidden by default) -->
        <button id="reset-btn" class="hidden w-full px-4 py-3 rounded-xl bg-gray-800 text-white font-semibold hover:bg-gray-900 transition-all duration-200">
            Restart Countdown
        </button>

    </div>

    <script>
        // Configuration
        const TARGET_URL = "https://poki.com";
        const INITIAL_SECONDS = 5;
        
        // State
        let secondsLeft = INITIAL_SECONDS;
        let timerInterval = null;
        let isCancelled = false;

        // DOM Elements
        const timerDisplay = document.getElementById('timer-display');
        const timerText = document.getElementById('timer-text');
        const progressBar = document.getElementById('progress-bar');
        const countdownContainer = document.getElementById('countdown-container');
        const cancelledMessage = document.getElementById('cancelled-message');
        const buttonContainer = document.getElementById('button-container');
        const cancelBtn = document.getElementById('cancel-btn');
        const redirectBtn = document.getElementById('redirect-btn');
        const resetBtn = document.getElementById('reset-btn');

        // Initialize
        function init() {
            startTimer();
            
            // Event Listeners
            cancelBtn.addEventListener('click', handleCancel);
            redirectBtn.addEventListener('click', handleRedirect);
            resetBtn.addEventListener('click', resetTimer);
        }

        // Timer Logic
        function startTimer() {
            // Clear any existing interval just in case
            if (timerInterval) clearInterval(timerInterval);
            
            secondsLeft = INITIAL_SECONDS;
            isCancelled = false;
            updateUIState();

            timerInterval = setInterval(() => {
                if (isCancelled) return;

                secondsLeft--;
                updateDisplay();

                if (secondsLeft <= 0) {
                    clearInterval(timerInterval);
                    performRedirect();
                }
            }, 1000);
        }

        // Update Visuals
        function updateDisplay() {
            timerDisplay.textContent = secondsLeft;
            timerText.textContent = `Redirecting in ${secondsLeft} second${secondsLeft !== 1 ? 's' : ''}`;
            
            // Update progress bar width (percentage)
            const percentage = (secondsLeft / INITIAL_SECONDS) * 100;
            progressBar.style.width = `${percentage}%`;
        }

        // Actions
        function handleCancel() {
            if (isCancelled) return;
            
            isCancelled = true;
            clearInterval(timerInterval);
            
            // UI Updates for Cancelled State
            countdownContainer.classList.add('hidden');
            buttonContainer.classList.add('hidden');
            
            cancelledMessage.classList.remove('hidden');
            cancelledMessage.classList.add('animate-fade-in');
            
            resetBtn.classList.remove('hidden');
        }

        function handleRedirect() {
            clearInterval(timerInterval);
            performRedirect();
        }

        function performRedirect() {
            // Visual feedback before leaving
            document.body.style.cursor = 'wait';
            timerText.textContent = "Redirecting now...";
            
            // Small delay to allow UI update
            setTimeout(() => {
                window.location.href = TARGET_URL;
            }, 300);
        }

        function resetTimer() {
            // Reset UI
            cancelledMessage.classList.add('hidden');
            resetBtn.classList.add('hidden');
            
            countdownContainer.classList.remove('hidden');
            buttonContainer.classList.remove('hidden');
            
            // Restart
            startTimer();
        }

        function updateUIState() {
            updateDisplay();
        }

        // Run

    </script>
</body>
</html>

Yah batao yah kas level ki coding hai yah coding kar leta hai mera beta
