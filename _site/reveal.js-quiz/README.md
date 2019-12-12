# A quiz-plugin for reveal.js

This plugin provides the functionality of dynamic single- and multiple-choice quizzes for reveal.js-presentations. It is basically a reveal.js-friendly wrapper for [SlickQuiz](https://www.github.com/jewlofthelotus/SlickQuiz). SlickQuiz is a generic jQuery plugin for quizzes, which was originally created by Julie Cameron while previously employed at Quicken Loans. Some default values were changed to be more friendly for presentations and reveal.js-hooks for layouting were implemented. Also, quizzes can be added to presentations very easily by just providing a JSON-String that defines the quiz.

## Usage

First, download this repository and include the files in the plugin folder in your reveal.js-configuration. Then include the plugin in the dependencies section when initializing Reveal, including the callback:

```js
Reveal.initialize({
    dependencies: [
        // other dependencies
        { src: 'plugin/quiz/js/quiz.js', async: true, callback: function() { prepareQuizzes({}); } }
        // other dependencies
    ]
});
```

For each quiz you want to use, just include a ```<script data-quiz>```-Tag with a quiz object as follows:

```html
<script data-quiz>
    quiz = {
            "info": {
                    "name":    "Test Your Knowledge!!",
                    "main":    "Think you're smart enough to be on Jeopardy? Find out with this super crazy knowledge quiz!",
                    "level1":  "Jeopardy Ready",
                    "level2":  "Jeopardy Contender",
                    "level3":  "Jeopardy Amateur",
                    "level4":  "Jeopardy Newb",
                    "level5":  "Stay in school, kid..." // no comma here
            },
            "questions": [
                    { // Question 1 - Multiple Choice, Single True Answer
                            "q": "What number is the letter A in the English alphabet?",
                            "a": [
                                    {"option": "8",      "correct": false},
                                    {"option": "14",     "correct": false},
                                    {"option": "1",      "correct": true},
                                    {"option": "23",     "correct": false} // no comma here
                            ],
                            "correct": "<p><span>That's right!</span> The letter A is the first letter in the alphabet!</p>",
                            "incorrect": "<p><span>Uhh no.</span> It's the first letter of the alphabet. Did you actually <em>go</em> to kindergarden?</p>" // no comma here
                    },
                    // more questions here
            ]
    }
</script>
```

The quiz-name will appear as the title of the slide. It is advised to include just the quiz on the slide and no further content.

Further documentation on the various formats and options supported by SlickQuiz can be found in the SlickQuiz-Documentation found below.

# SlickQuiz Documentation – Options

## Base Config Options

```js
var quizJSON = {
    "info": {
        "name":    "The Quiz Header",
        "main":    "The Quiz Description Text",
        "results": "The Quiz Results Copy",
        "level1":  "The highest ranking",
        "level2":  "The almost highest ranking",
        "level3":  "The middle ranking",
        "level4":  "The almost lowest ranking",
        "level5":  "The lowest ranking"
    },
    "questions": [
        {
            "q": "The Question?",
            "a": [
                {"option": "an incorrect answer",       "correct": false},
                {"option": "a correct answer",          "correct": true},
                {"option": "another correct answer",    "correct": true}
            ],
            "correct": "The Correct Response Message",
            "incorrect": "The Incorrect Response Message",
            "select_any": false, // optional, see "Question Options" above
            "force_checkbox": false // optional, see "Question Options" above
        },
        // more questions
    ]
}
```

## Adding HTML to Questions and Answers

Standard HTML elements like images, videos embeds, headers, paragraphs, etc., can be used within text values like `q` and `a` options.

```js
"q": "The Question? <img src='path/to/image.png' />",
"a": [
    {"option": "an <b>incorrect</b> answer", "correct": false},
    {"option": "a <b>correct</b> answer",    "correct": true},
]
```

## Configuration Options

Many variables can be adjusted by adding custom configurations to the ```prepareQuizzes({});``` callback in the ```Reveal.initialize``` function. For example, the following configuration would set the ```preventUnanswered``` parameter to true and change the ```checkAnswerText``` variable:

```js
callback: function() { prepareQuizzes({preventUnanswered: true, checkAnswerText: 'Check Answer'}); }
```

All possible configuration are listed below.

#### Text Options

**`checkAnswerText`** (String) *Default: 'Check My Answer!';* - the text to use on the check answer button

**`nextQuestionText`** (String) *Default: 'Next &raquo;';* - the text to use on the next question button

**`completeQuizText`** (String) *Default: '';* - the text to use for the last button the user will click before getting results; if left null / blank (default) - <code>nextQuestionText</code> will be used. Example: "Get Your Results!"

**`backButtonText`** (String) *Default: 'Back';* - the text to use on the back button; if left null / blank (default) - no back button will be displayed

**`tryAgainText`** (String) *Default: 'Try Again';* - the text to use on the try again button; if left null / blank - no try again button will be displayed

**`preventUnansweredText`** (String) *Default: 'You must select at least one answer.';* - the text to display if a user submits a blank answer while <code>preventUnanswered</code> is enabled

**`questionCountText`** (String) *Default: 'Question %current of %total';* - if <code>displayQuestionCount</code> is enabled, this will format that text using the string provided. <code>%current</code> and <code>%total</code> are placeholders that will output the appropriate values. Note: <code>displayQuestionCount</code> may eventually be deprecated in favor of this option

**`questionTemplateText`** (String) *Default:  '%count. %text';* - if <code>displayQuestionNumber</code> is enabled, this will format that question number and question using the string provided. <code>%count</code> and <code>%text</code> are placeholders that will output the appropriate values. Note: <code>displayQuestionNumber</code> may eventually be deprecated in favor of this option

**`scoreTemplateText`** (String) *Default: '%score / %total';* - the format of the final score text. <code>%score</code> and <code>%total</code> are placeholders that will output the appropriate values

**`nameTemplateText`** (String) *Default:  '&lt;span&gt;Quiz: &lt;/span&gt;%name';* - the format of the quiz name; <code>%name</code> is a placeholder that will output the quiz name. Note: the "Quiz" span in the default value is used to enhance accessibility, it will not display on the screen.


#### Functionality Options

**`skipStartButton`** (Boolean) *Default: false;* - whether or not to skip the quiz "start" button

**`numberOfQuestions`** (Integer) *Default: null;* - the number of questions to load from the question set in the JSON object, defaults to null (all questions); Note: If you set this to an integer, you'll probably also want to set <code>randomSortQuestions</code> to **true** to ensure that you get a mixed set of questions each page load.

**`randomSortQuestions`** (Boolean) *Default: false;* - whether or not to randomly sort questions ONLY

**`randomSortAnswers`** (Boolean) *Default: false;* - whether or not to randomly sort answers ONLY

**`preventUnanswered`** (Boolean) *Default: false;* - prevents submitting a question with zero answers

**`perQuestionResponseMessaging`** (Boolean) *Default: true;* - Displays correct / incorrect response messages after each question is submitted.

**`perQuestionResponseAnswers`** (Boolean) *Default: false;* - Keeps the answer options in display after the question is submitted. Note: this should be used in tandem with <code>perQuestionResponseMessaging</code>

**`completionResponseMessaging`** (Boolean) *Default: false;* - Displays all questions and answers with correct or incorrect response messages when the quiz is completed.

**`displayQuestionCount`** (Boolean) *Default: true;* - whether or not to display the number of questions and which question the user is on, for example "Question 3 of 10". Note: this may eventually be deprecated in favor of <code>questionCountText</code>

**`displayQuestionNumber`** (Boolean) *Default: true;* - whether or not to display the number of the question along side the question itself, for example, the "1." in "1. What is the first letter of the alphabet?" Note: this may eventually be deprecated in favor of <code>questionTemplateText</code>

**`disableScore`** (Boolean) *Default: false;* - Removes the score from the final results display. Eliminates the need for an element with class <code>quizScore</code> in the markup.

**`disableRanking`** (Boolean) *Default: false;* - Removes the ranking leve from the final results display. Eliminates the need for an element with class <code>quizLevel</code> in the markup, as well as the need for JSON values for <code>level1</code> through <code>level5</code>.

**`scoreAsPercentage`** (Boolean) *Default: false;* - Returns the score as a percentage rather than the number of correct responses. If enabled, you'll also want to adjust <code>scoreTemplateText</code> to something like *'%score'*


#### Question Options

*See "Base Config Options" below for examples*

**`select_any`** (Boolean) *Optional*  - Use if there is more than one true answer and when submitting any single true answer should be considered correct.  (Select ANY that apply vs. Select ALL that apply)

**`force_checkbox`** (Boolean) *Optional* - Set this to `true` if you want to render checkboxes instead of radios even if the question only has one true answer.


#### Event Options

**`events.onStartQuiz`** (function) *Default: empty;* - a function to be executed once the quiz has started.

**`events.onCompleteQuiz`** (function) *Default: empty;* - a function to be executed the quiz has completed; the function will be passed two arguments in an object: <code>options.questionCount</code>, <code>options.score</code>


#### Animation Callback Options

**`animationCallbacks.setupQuiz`** (function) *Default: empty;* - a function to be executed once all jQuery animations have completed in the <code>setupQuiz</code> method

**`animationCallbacks.startQuiz`** (function) *Default: empty;* - a function to be executed once all jQuery animations have completed in the <code>startQuiz</code> method; note that <code>events.onStartQuiz()</code> would execute before this callback method due to durations of jQuery animations

**`animationCallbacks.resetQuiz`** (function) *Default: empty;* - a function to be executed once all jQuery animations have completed in the <code>resetQuiz</code> method

**`animationCallbacks.checkAnswer`** (function) *Default: empty;* - a function to be executed once all jQuery animations have completed in the <code>checkAnswer</code> method

**`animationCallbacks.nextQuestion`** (function) *Default: empty;* - a function to be executed once all jQuery animations have completed in the <code>nextQuestion</code> method

**`animationCallbacks.backToQuestion`** (function) *Default: empty;* - a function to be executed once all jQuery animations have completed in the <code>backToQuestion</code> method

**`animationCallbacks.completeQuiz`** (function) *Default: empty;* - a function to be executed once all jQuery animations have completed in the <code>completeQuiz</code> method; note that <code>events.onCompleteQuiz()</code> would execute before this callback method due to durations of jQuery animations


#### Deprecated Options

**`disableNext`** - Prevents submitting a question with zero answers. You should now use <code>preventUnanswered</code> instead.

**`disableResponseMessaging`** - Hides all correct / incorrect response messages. You should now use <code>perQuestionResponseMessaging</code> and <code>completionResponseMessaging</code> instead.

**`randomSort`** - Randomly sort all questions AND their answers. You should now use <code>randomSortQuestions</code> and <code>randomSortAnswers</code> instead.


## Base HTML Structure

The slickQuiz ID and class names are what are important here:

```html
<body id="slickQuiz">
    <h1 class="quizName"></h1>
    <div class="quizArea">
        <div class="quizHeader">
            <a class="startQuiz" href="">Get Started!</a>
        </div>
    </div>
    <div class="quizResults">
        <h3 class="quizScore">You Scored: <span></span></h3>
        <h3 class="quizLevel"><strong>Ranking:</strong> <span></span></h3>
        <div class="quizResultsCopy"></div>
    </div>
</body>
```

SlickQuiz was originally created by [Julie Cameron](http://juliecameron.com).
