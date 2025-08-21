Code Documentation - US Naturalization Quiz
Screenshot of the Quiz Game

About the Game
This is a web-based quiz that helps users practice the US Naturalization Test. Players answer 10 random questions from the official USCIS question and need 6 correct answers to pass, just like the real test.

I built this game because I'm currently studying for US Natrualization test to become US citizen on August 22nd. I wanted to build a fun way to study for the test, intead of

How to Play:
Answer 10 questions randomly selected from the official USCIS question bank.
Click on an answer choice to select it.
Click "Submit Answer" to check your selection.
Click "Next Question" to continue to the next question.
After 10 questions, your score and pass/fail status will be displayed.
Click "Take Quiz Again" to restart the game.
Technologies Used
HTML
CSS
JavaScript
DOM manipulation
Event Listeners
Flexbox
Resources
Questions: USCIS Official Question
Check Mark and Cross Mark Icon: HTML-CSS-JSS
American Flag: Emojipedia
Code Reference: MDN and Google
File Structure Overview
US-Naturalization-Quiz/
├── index.html # Main HTML structure
├── style.css # Styling and responsive design
├── data.js # Question database and shuffle logic
├── app.js # Game logic and DOM manipulation
└── README.md # Project documentation
HTML Structure (index.html)
Key HTML Elements
Container: .container - Main wrapper for the entire application
Header: .header - Title and description section
Instructions: .instructions - Game rules and how-to-play
Game Stats: .game-stats - Real-time progress tracking
Question Container: .question-container - Active quiz area
Results: .results - Final score and pass/fail status
Interactive Elements
<button class="btn" id="submit-btn">Submit Answer</button>
<button class="btn" id="next-btn">Next Question</button>
<button class="btn" id="restart-btn">Restart Quiz</button>
CSS Styling (style.css)
Key CSS Classes and Their Functions
Layout Classes:

.container - Centers content with max-width and shadow
.game-stats - Flexbox layout for score display
.question-container - Main quiz interaction area
Interactive States:

.option - Base styling for answer buttons
.option.selected - Highlighted selected answer (blue background)
.option.correct - Correct answer display (green background)
.option.incorrect - Wrong answer display (red background)
Responsive Design:

@media (max-width: 600px) {
.game-stats { flex-direction: column; }
.controls { flex-direction: column; }
}
Data Management (data.js)
Main Data Structure
const quizQuestionsRaw = [
{
question: "What is the supreme law of the land?",
correctAnswer: "The Constitution",
incorrectAnswers: ["The Declaration of Independence", "The Bill of Rights", "The Articles of Confederation"]
}
// ... 99 more questions
];
Key Functions
shuffleArray(array)
Purpose: Randomizes the order of array elements How it works:

function shuffleArray(array) {
let newArray = [...array]; // Create copy to avoid mutation

    for (let i = 0; i < newArray.length; i++) {
        let randomIndex = Math.floor(Math.random() * newArray.length);
        // Swap current element with random element
        let temp = newArray[i];
        newArray[i] = newArray[randomIndex];
        newArray[randomIndex] = temp;
    }
    return newArray;

}
prepareQuestions()
Purpose: Converts raw question data into game-ready format Process:

Loop through all raw questions using for (let i = 0; i < quizQuestionsRaw.length; i++)
Combine answers: Creates array with correct answer + 3 incorrect answers
Shuffle answers: Randomizes answer positions so correct isn't always first
Find correct index: Locates where correct answer ended up after shuffle
Create game object: Returns formatted question with shuffled options
for (let i = 0; i < quizQuestionsRaw.length; i++) {
let q = quizQuestionsRaw[i];
let allAnswers = [q.correctAnswer, ...q.incorrectAnswers];
let shuffledAnswers = shuffleArray(allAnswers);
let correctIndex = shuffledAnswers.indexOf(q.correctAnswer);

    gameQuestions.push({
        question: q.question,
        options: shuffledAnswers,
        correct: correctIndex
    });

}
Game Logic (app.js)
Global Variables
let currentQuestionIndex = 0; // Tracks current question (0-9)
let selectedQuestions = []; // 10 randomly selected questions
let userAnswers = []; // Stores all user responses
let correctCount = 0; // Number of correct answers
let selectedAnswer = null; // Currently selected answer index
Core Game Functions
initQuiz()
Purpose: Initializes/resets the entire quiz Actions:

Resets all counters to 0
Randomly selects 10 questions from the 100 available
Shows question container, hides results
Loads first question
Updates display stats
function initQuiz() {
currentQuestionIndex = 0;
correctCount = 0;
userAnswers = [];
selectedAnswer = null;
selectedQuestions = shuffleArray(quizQuestions).slice(0, 10);
// Show/hide UI elements and load first question
}
loadQuestion()
Purpose: Displays current question and answer options Process:

Gets current question from selectedQuestions[currentQuestionIndex]
Updates question number display (e.g., "Question 1 of 10")
Sets question text
Creates answer buttons using forEach loop:
question.options.forEach((option, index) => {
const button = document.createElement('button');
button.className = 'option';
button.textContent = option;
button.onclick = () => selectAnswer(index, button);
optionsContainer.appendChild(button);
});
Resets UI state (disable submit, hide feedback)
selectAnswer(index, button)
Purpose: Handles user answer selection Actions:

Removes previous selections using querySelectorAll loop:
document.querySelectorAll('.option').forEach(opt => {
opt.classList.remove('selected');
});
Highlights clicked button
Stores selected answer index
Enables submit button
submitAnswer()
Purpose: Processes submitted answer and shows feedback Logic Flow:

Validation: Returns early if no answer selected
Scoring: Compares selectedAnswer with question.correct
Data Storage: Saves answer details to userAnswers array
UI Updates: Loops through all options to show correct/incorrect:
document.querySelectorAll('.option').forEach((option, index) => {
option.disabled = true;
if (index === question.correct) option.classList.add('correct');
if (index === selectedAnswer && !isCorrect) option.classList.add('incorrect');
});
Feedback Display: Shows checkmark or correct answer
Button State: Hides submit, shows next button
nextQuestion()
Purpose: Advances to next question or shows results Logic:

function nextQuestion() {
currentQuestionIndex++;
if (currentQuestionIndex < 10) {
loadQuestion(); // More questions remaining
} else {
showResults(); // Quiz complete
}
}
updateStats()
Purpose: Updates real-time statistics display Calculations:

currentQuestionStat.textContent = currentQuestionIndex + 1;
correctCountStat.textContent = correctCount;

let percentage = 0;
if (userAnswers.length > 0) {
percentage = Math.round((correctCount / userAnswers.length) \* 100);
}
scoreStat.textContent = percentage + "%";
showResults()
Purpose: Displays final quiz results Logic:

Calculate final score: Math.round((correctCount / 10) \* 100)
Determine pass/fail: let passed = correctCount >= 6
Conditional display: Shows different messages based on pass status
Smooth scroll: Brings results into view
resultsTitle.innerHTML = passed ? '🎉 Congratulations!' : '📚 Keep Studying!';
passStatus.className = passed ? 'pass-status pass' : 'pass-status fail';
Event Handling System
setupEventListeners()
Purpose: Attaches click handlers to control buttons

submitBtn.addEventListener('click', submitAnswer);
nextBtn.addEventListener('click', nextQuestion);
restartBtn.addEventListener('click', restartQuiz);
startQuiz()
Purpose: Initializes the application when page loads DOM Ready Check:

if (document.readyState === 'loading') {
document.addEventListener('DOMContentLoaded', startQuiz);
} else {
startQuiz(); // DOM already ready
}
Key Programming Concepts Used

1. Array Manipulation
   Spread Operator: [...array] for creating copies
   Slice: array.slice(0, 10) for selecting subsets
   forEach: For iterating through options
   indexOf: Finding correct answer position after shuffle
2. DOM Manipulation
   Element Creation: document.createElement('button')
   Class Management: classList.add(), classList.remove()
   Event Binding: addEventListener() and onclick
   Content Updates: textContent, innerHTML
3. State Management
   Global Variables: Track game progress and user selections
   Conditional Logic: if/else statements for game flow
   Data Persistence: Arrays store user answers and question data
4. User Interface Control
   Button States: Enable/disable based on user actions
   Visual Feedback: CSS classes for correct/incorrect answers
   Progressive Disclosure: Show/hide elements based on game state
5. Mathematical Operations
   Random Generation: Math.random() for shuffling
   Percentage Calculations: Score conversion to percentages
   Rounding: Math.round() for clean percentage display
   Game Flow Summary
   Page Load → startQuiz() → setupEventListeners() → initQuiz()
   Quiz Start → shuffleArray() selects 10 random questions → loadQuestion()
   User Interaction → selectAnswer() → submitAnswer() → nextQuestion()
   Repeat Steps 2-3 for 10 questions
   Quiz End → showResults() → Display pass/fail → Option to restartQuiz()
   This architecture creates a smooth, interactive quiz experience with proper separation of concerns between data, logic, and presentation.
