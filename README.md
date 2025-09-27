<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ইন্টারেক্টিভ কুইজ প্ল্যাটফর্ম</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+Bengali:wght@400;500;600;700&display=swap');
        body {
            font-family: 'Noto Sans Bengali', sans-serif;
            background-color: #f0f2f5;
            color: #333;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 1rem;
        }
        .container {
            background-color: #ffffff;
            border-radius: 1.5rem;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
            max-width: 600px;
            width: 100%;
            padding: 2rem;
            text-align: center;
            border: 1px solid #e0e0e0;
        }
        .quiz-header {
            font-size: 1.5rem;
            font-weight: 700;
            color: #333;
            margin-bottom: 1.5rem;
            border-bottom: 2px solid #e0e0e0;
            padding-bottom: 0.75rem;
        }
        .quiz-progress {
            font-size: 1rem;
            color: #888;
            margin-bottom: 1rem;
        }
        .question-card {
            background-color: #f7f9fc;
            border-radius: 1.2rem;
            padding: 1.5rem;
            margin-bottom: 1rem;
            border: 1px solid #e0e0e0;
        }
        .question-text {
            font-size: 1.1rem;
            font-weight: 600;
            color: #333;
            line-height: 1.6;
            margin-bottom: 1.2rem;
        }
        .option-button {
            width: 100%;
            padding: 0.8rem 1rem;
            margin-bottom: 0.6rem;
            font-size: 1rem;
            font-weight: bold;
            color: #555;
            background-color: #f0f2f5;
            border-radius: 0.75rem;
            /* বর্ডার কালার কালো করা হয়েছে */
            border: 1px solid #333; 
            cursor: pointer;
            transition: all 0.3s ease;
            text-align: left;
        }
        .option-button:hover:not(.correct):not(.wrong) {
            background-color: #e6e9ed;
            transform: translateY(-2px);
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
            border-color: #1a1a1a;
        }
        .option-button.correct {
            background-color: #c8e6c9;
            border-color: #388E3C;
            color: #1b5e20;
            font-weight: 600;
        }
        .option-button.wrong {
            background-color: #ffcdd2;
            border-color: #B71C1C;
            color: #b71c1c;
            font-weight: 600;
        }
        .feedback {
            font-size: 1rem;
            font-weight: 500;
            margin-top: 0.8rem;
            min-height: 24px;
        }
        .navigation-buttons {
            display: flex;
            justify-content: space-between;
            gap: 0.8rem;
        }
        .nav-button {
            flex: 1;
            padding: 0.7rem;
            border-radius: 0.75rem;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        .next-button, .back-button {
            background-color: #1976D2;
            color: white;
            border: 1px solid #1976D2;
        }
        .next-button:hover, .back-button:hover {
            background-color: #1565C0;
        }
        .restart-button {
            background-color: #4CAF50;
            color: white;
            padding: 0.8rem 2rem;
            border-radius: 0.75rem;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        .restart-button:hover {
            background-color: #388E3C;
        }
        .hidden {
            display: none;
        }
        .analysis-section {
            text-align: left;
            margin-top: 2rem;
            padding-top: 1.5rem;
            border-top: 1px solid #e0e0e0;
        }
        .analysis-section h3 {
            font-size: 1.2rem;
            font-weight: 700;
            color: #333;
            margin-bottom: 1rem;
            text-align: center;
        }
        .analysis-section p {
            font-size: 0.9rem;
            margin-bottom: 0.8rem;
        }
        .wrong-answers-list {
            margin-top: 1.5rem;
        }
        .wrong-answers-list h4 {
            font-size: 1.1rem;
            font-weight: 600;
            margin-bottom: 0.8rem;
            color: #B71C1C;
        }
        .wrong-answer-item {
            background-color: #fce4ec;
            border: 1px solid #F48FB1;
            border-left: 5px solid #F48FB1;
            padding: 1rem;
            border-radius: 0.75rem;
            margin-bottom: 0.8rem;
        }
        .grade-text {
            font-size: 1.5rem;
            font-weight: bold;
            margin-top: 1rem;
        }

        @keyframes shake {
            0%, 100% {
                transform: translateX(0);
            }
            10%, 30%, 50%, 70%, 90% {
                transform: translateX(-3px);
            }
            20%, 40%, 60%, 80% {
                transform: translateX(3px);
            }
        }
        .shake-animation {
            animation: shake 0.5s;
        }
    </style>
</head>
<body>
    <div class="container">
        <div id="quiz-screen">
            <h2 class="quiz-header">কুইজ</h2>
            <p class="quiz-progress" id="progress-info">প্রশ্ন ১ / ৫</p>

            <div class="question-card">
                <p id="question-text" class="question-text">প্রশ্ন লোড হচ্ছে...</p>
                <div id="options-container" class="space-y-4">
                    <!-- Options will be injected here by JavaScript -->
                </div>
            </div>

            <div id="feedback" class="feedback"></div>

            <div class="navigation-buttons mt-4">
                <button id="back-button" class="nav-button back-button hidden">পূর্ববর্তী</button>
                <button id="next-button" class="nav-button next-button hidden">পরবর্তী</button>
            </div>
        </div>

        <div id="result-screen" class="hidden">
            <h2 class="quiz-header">কুইজ শেষ!</h2>
            <div class="text-center my-4">
                <p class="text-2xl font-bold text-gray-700">তোমার স্কোর: <span id="final-score">0</span> / <span id="total-questions">0</span></p>
                <p class="text-xl font-semibold mt-2 text-green-700">সঠিক উত্তরের শতাংশ: <span id="percentage">0</span>%</p>
                <p id="grade-text" class="grade-text"></p>
            </div>
            
            <div id="analysis-section" class="analysis-section hidden">
                <div id="wrong-answers-list" class="wrong-answers-list">
                    <!-- Wrong answers will be injected here -->
                </div>
            </div>

            <button id="restart-button" class="restart-button mt-4">আবার শুরু কর</button>
        </div>
    </div>
    <!-- BGM/Sound Audio Tags সরিয়ে দেওয়া হয়েছে -->

    <script>
        const questions = [
            {
                 [
    {
        "question": "১. আলোর প্রতিফলন কী ধরনের ঘটনা?",
        "options": ["শোষণ", "বিচ্ছুরণ", "পুনরায় ফিরে আসা", "প্রতিসরণ"],
        "answer": "পুনরায় ফিরে আসা",
        "topic": "পদার্থবিজ্ঞান: আলো",
        "solution": "আলোর প্রতিফলন হলো কোনো তলে বাধা পেয়ে আলোর গতিপথ পরিবর্তন করে পুনরায় প্রথম মাধ্যমে ফিরে আসা।"
    },
    {
        "question": "২. দ্বি-বীজপত্রী উদ্ভিদের মূলের প্রধান বৈশিষ্ট্য কী?",
        "options": ["গৌণ বৃদ্ধি থাকে না", "সাধারণত গুচ্ছ মূল থাকে", "কান্ডের ভেতরের দিকে জাইলেম থাকে", "সাধারণত প্রধান মূল থাকে"],
        "answer": "সাধারণত প্রধান মূল থাকে",
        "topic": "জীববিজ্ঞান: উদ্ভিদ পরিচিতি",
        "solution": "দ্বি-বীজপত্রী উদ্ভিদের প্রধান বৈশিষ্ট্য হলো এদের সাধারণত প্রধান মূল (Tap root) থাকে, যা মাটির গভীরে প্রবেশ করে।"
    },
    // তোর বাকি ১০০+ প্রশ্ন এই ফরম্যাটে যোগ কর...
]

            }
        ];

        let currentQuestionIndex = 0;
        let score = 0;
        let answeredQuestions = [];

        const quizScreen = document.getElementById('quiz-screen');
        const resultScreen = document.getElementById('result-screen');
        const progressInfo = document.getElementById('progress-info');
        const questionText = document.getElementById('question-text');
        const optionsContainer = document.getElementById('options-container');
        const nextButton = document.getElementById('next-button');
        const backButton = document.getElementById('back-button');
        const restartButton = document.getElementById('restart-button');
        const feedbackElement = document.getElementById('feedback');
        const finalScoreElement = document.getElementById('final-score');
        const totalQuestionsElement = document.getElementById('total-questions');
        const percentageElement = document.getElementById('percentage');
        const gradeTextElement = document.getElementById('grade-text');
        const analysisSection = document.getElementById('analysis-section');
        const wrongAnswersList = document.getElementById('wrong-answers-list');
        // সাউন্ড ভেরিয়েবলগুলো সরিয়ে দেওয়া হয়েছে

        // সাউন্ড প্লে করার ফাংশনটিও সরিয়ে দেওয়া হয়েছে

        function showQuestion() {
            if (currentQuestionIndex < 0) {
                currentQuestionIndex = 0;
            }

            if (currentQuestionIndex >= questions.length) {
                showResults();
                return;
            }

            const currentQuestion = questions[currentQuestionIndex];
            
            progressInfo.textContent = `প্রশ্ন ${currentQuestionIndex + 1} / ${questions.length}`;

            feedbackElement.textContent = '';
            optionsContainer.innerHTML = '';
            nextButton.classList.add('hidden');
            
            if (currentQuestionIndex > 0) {
                backButton.classList.remove('hidden');
            } else {
                backButton.classList.add('hidden');
            }

            questionText.textContent = currentQuestion.question;
            currentQuestion.options.forEach(option => {
                const button = document.createElement('button');
                button.textContent = option;
                button.classList.add('option-button');
                button.onclick = () => checkAnswer(option, currentQuestion.answer, button);
                optionsContainer.appendChild(button);
            });

            if (answeredQuestions[currentQuestionIndex]) {
                const answeredData = answeredQuestions[currentQuestionIndex];
                const selectedButton = Array.from(optionsContainer.children).find(btn => btn.textContent === answeredData.selectedOption);
                checkAnswer(answeredData.selectedOption, currentQuestion.answer, selectedButton, true);
                if (currentQuestionIndex < questions.length -1) {
                    nextButton.textContent = "পরবর্তী প্রশ্ন";
                } else {
                    nextButton.textContent = "ফলাফল দেখুন";
                }
                nextButton.classList.remove('hidden');
            }
        }

        function checkAnswer(selectedOption, correctAnswer, button, isRevisited = false) {
            if (!isRevisited && answeredQuestions[currentQuestionIndex]) {
                return;
            }

            if (!isRevisited && !answeredQuestions[currentQuestionIndex]) {
                const isCorrect = selectedOption === correctAnswer;
                answeredQuestions[currentQuestionIndex] = {
                    selectedOption,
                    correct: isCorrect,
                    question: questions[currentQuestionIndex].question,
                    correctAnswer: questions[currentQuestionIndex].answer,
                    topic: questions[currentQuestionIndex].topic,
                    solution: questions[currentQuestionIndex].solution
                };
                if (isCorrect) {
                    score++;
                    // সাউন্ড প্লে করার কল সরিয়ে দেওয়া হয়েছে
                } else {
                    // সাউন্ড প্লে করার কল সরিয়ে দেওয়া হয়েছে
                    button.classList.add('shake-animation');
                    setTimeout(() => {
                        button.classList.remove('shake-animation');
                    }, 500);
                }
            }

            const allButtons = optionsContainer.querySelectorAll('.option-button');
            allButtons.forEach(btn => {
                btn.style.pointerEvents = 'none';
                if (btn.textContent === correctAnswer) {
                    btn.classList.add('correct');
                } else if (btn.textContent === selectedOption && selectedOption !== correctAnswer) {
                    btn.classList.add('wrong');
                }
            });

            // ফিডব্যাক মেসেজ দেওয়া
            if (selectedOption === correctAnswer) {
                feedbackElement.textContent = 'সঠিক উত্তর! এগিয়ে চলো!';
                feedbackElement.style.color = '#1b5e20';
            } else {
                feedbackElement.textContent = `ভুল হয়েছে। সঠিক উত্তর হলো: ${correctAnswer}`;
                feedbackElement.style.color = '#b71c1c';
            }
            
            if (currentQuestionIndex < questions.length - 1) {
                 nextButton.textContent = "পরবর্তী প্রশ্ন";
            } else {
                nextButton.textContent = "ফলাফল দেখুন";
            }
            nextButton.classList.remove('hidden');
            backButton.classList.remove('hidden');
        }

        function getGrade(percentage) {
            if (percentage >= 80) return "A+";
            if (percentage >= 71) return "A";
            if (percentage >= 61) return "A-";
            if (percentage >= 51) return "B";
            if (percentage >= 41) return "C";
            if (percentage >= 34) return "D";
            return "Fail";
        }

        function showResults() {
            quizScreen.classList.add('hidden');
            resultScreen.classList.remove('hidden');
            
            const finalScore = answeredQuestions.filter(q => q.correct).length;
            finalScoreElement.textContent = finalScore;
            totalQuestionsElement.textContent = questions.length;
            
            const percentage = ((finalScore / questions.length) * 100).toFixed(2);
            percentageElement.textContent = percentage;

            const grade = getGrade(parseFloat(percentage));
            gradeTextElement.textContent = `তোমার গ্রেড: ${grade}`;

            const wrongAnswers = answeredQuestions.filter(q => !q.correct);
            if (wrongAnswers.length > 0) {
                analysisSection.classList.remove('hidden');
                wrongAnswersList.innerHTML = '<h4>ভুল উত্তর দেওয়া প্রশ্নগুলো:</h4>';
                
                wrongAnswers.forEach(item => {
                    const analysisItem = document.createElement('div');
                    analysisItem.classList.add('wrong-answer-item');
                    analysisItem.innerHTML = `
                        <p><strong>প্রশ্ন:</strong> ${item.question}</p>
                        <p><strong>তোমার উত্তর:</strong> <span class="text-red-700">${item.selectedOption}</span></p>
                        <p><strong>সঠিক উত্তর:</strong> <span class="text-green-700">${item.correctAnswer}</span></p>
                        <p class="mt-2"><strong>বিষয়:</strong> ${item.topic}</p>
                        <p><strong>সমাধান:</strong> ${item.solution}</p>
                    `;
                    wrongAnswersList.appendChild(analysisItem);
                });
            } else {
                analysisSection.classList.remove('hidden');
                analysisSection.innerHTML = '<h3 class="text-green-700">অভিনন্দন! তুমি সব প্রশ্নের সঠিক উত্তর দিয়েছ!</h3>';
            }
        }

        function restartQuiz() {
            currentQuestionIndex = 0;
            score = 0;
            answeredQuestions = [];
            quizScreen.classList.remove('hidden');
            resultScreen.classList.add('hidden');
            showQuestion();
        }

        nextButton.addEventListener('click', () => {
            currentQuestionIndex++;
            showQuestion();
        });

        backButton.addEventListener('click', () => {
            currentQuestionIndex--;
            showQuestion();
        });

        restartButton.addEventListener('click', restartQuiz);

        showQuestion();
    </script>
</body>
</html>

