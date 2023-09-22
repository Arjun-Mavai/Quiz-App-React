function Quiz() {
  // ... existing state variables
  const [selectedOptionIndex, setSelectedOptionIndex] = useState(null); // New state variable

  const handleAnswerSubmit = () => {
    // ... existing logic

    // Reset selected option index
    setSelectedOptionIndex(null);
  };

  return (
    <div className="bg-gray-200 min-h-screen flex items-center justify-center">
      {/* ... existing JSX */}
      <ul className="space-y-2">
        {questions[currentQuestion].options.map((option, index) => (
          <li
            key={index}
            className={`p-2 cursor-pointer rounded-lg hover:bg-gray-100 ${
              isAnswered
                ? index === selectedOptionIndex && userAnswer !== questions[currentQuestion].answer
                  ? "bg-red-200" // Red for incorrect answer
                  : index === questions[currentQuestion].options.indexOf(questions[currentQuestion].answer)
                  ? "bg-green-200" // Green for correct answer
                  : ""
                : ""
            }`}
            onClick={() => {
              if (!isAnswered) {
                setUserAnswer(option);
                setSelectedOptionIndex(index); // Set selected option index
              }
            }}
          >
            {option}
          </li>
        ))}
      </ul>
      {/* ... existing JSX */}
    </div>
  );
}


### Deeper Explanation

#### Real-Life Analogy

Imagine you're a teacher grading multiple-choice questions on an exam. Each student circles an answer for each question. After all exams are submitted, you go through each question for each student:

- If the student's answer is correct, you mark it with a green checkmark.
- If the answer is incorrect, you mark it with a red "X" and also mark the correct answer with a green checkmark.

This is similar to what's happening in the code. The `selectedOptionIndex` is like the answer the student circled, and `isAnswered` is like the flag that tells you the student has submitted the answer and you can now grade it.

#### User Perspective

From the user's point of view, they are presented with multiple-choice questions. They click on an option, and upon submission:

- If they are correct, the option they clicked turns green.
- If they are incorrect, the option they clicked turns red, and the correct answer turns green.

#### Developer Perspective

As a developer, you need to keep track of several things:

1. **Which question is currently displayed?** - Managed by `currentQuestion`.
2. **What option did the user click on?** - Managed by `userAnswer`.
3. **What is the index of the option the user clicked on?** - Managed by `selectedOptionIndex`.
4. **Has the user submitted an answer for grading?** - Managed by `isAnswered`.

#### Code Explanation

Let's break down this part of the code:

```jsx
isAnswered
  ? index === selectedOptionIndex && userAnswer !== questions[currentQuestion].answer
    ? "bg-red-200" // Red for incorrect answer
    : index === questions[currentQuestion].options.indexOf(questions[currentQuestion].answer)
    ? "bg-green-200" // Green for correct answer
    : ""
  : ""
```

1. **`isAnswered`**: This checks if the user has submitted an answer. If not, no color changes are made (`""`).
  
2. **`index === selectedOptionIndex && userAnswer !== questions[currentQuestion].answer`**: This checks if the currently iterated option (in the `.map()` function) is the one the user selected and if it's incorrect. If both conditions are true, it sets the background color to red (`"bg-red-200"`).

3. **`index === questions[currentQuestion].options.indexOf(questions[currentQuestion].answer)`**: This checks if the currently iterated option is the correct answer. If true, it sets the background color to green (`"bg-green-200"`).

#### State Changes

- `currentQuestion`: Changes when moving to the next question.
- `userAnswer`: Changes when the user clicks on an option.
- `selectedOptionIndex`: Changes to the index of the option the user clicked on.
- `isAnswered`: Changes to `true` when the user submits an answer, allowing for the background color to change based on the correctness of the answer.

By understanding these aspects, you can effectively manage complex UI behaviors in React.


Certainly! If you want to simplify the code for changing the background color of the selected options, you can use a function to handle this logic. This function can return the appropriate CSS class based on the conditions.

Here's a simplified version of your code:

```jsx
function getOptionClass(option, index) {
  if (!isAnswered) return "";
  if (userAnswer === option && userAnswer !== questions[currentQuestion].answer) return "bg-red-200";
  if (questions[currentQuestion].answer === option) return "bg-green-200";
  return "";
}

return (
  <div>
    <ul className="space-y-2">
      {questions[currentQuestion].options.map((option, index) => (
        <li
          key={index}
          className={`p-2 cursor-pointer rounded-lg hover:bg-gray-100 ${getOptionClass(option, index)}`}
          onClick={() => {
            if (!isAnswered) {
              setUserAnswer(option);
            }
          }}
        >
          {option}
        </li>
      ))}
    </ul>
  </div>
);
```

In this version, I've created a function called `getOptionClass` that takes the current option and its index as arguments. This function returns the appropriate background color class based on whether the answer is correct, incorrect, or not yet submitted.

This way, the logic for determining the background color is encapsulated in a single function, making the code easier to read and maintain.