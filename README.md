# survey_builder_app
## Attendance Activity
## Date: 5-05-2025

## AIM
To create a Portfolio using HTML and CSS.

## ALGORITHM
### STEP 1
Initialize State Variables
### STEP 2
Implement Mode Toggle UI

### STEP 3
Build Mode UI Layout

### STEP 4
Add Question Functionality


### STEP 5
Edit and Remove Questions

### STEP 6
Switch to Fill Mode

### STEP 7
Capture User Input

### STEP 8
Handle Form Submission

### STEP 9
Display Summary of Responses

### STEP 10
Finalize and Test

## PROGRAM
### App.js
```
import React, { useState } from "react";
import './App.css';
export default function SurveyBuilderApp() {
  const [mode, setMode] = useState("build");
  const [questions, setQuestions] = useState([]);
  const [questionText, setQuestionText] = useState("");
  const [questionType, setQuestionType] = useState("text");
  const [optionsText, setOptionsText] = useState("");
  const [responses, setResponses] = useState({});
  const [submitted, setSubmitted] = useState(false);

  const addQuestion = () => {
    const newQuestion = {
      id: Date.now(),
      text: questionText,
      type: questionType,
      options: questionType !== "text" ? optionsText.split(",").map(opt => opt.trim()) : [],
    };
    setQuestions([...questions, newQuestion]);
    setQuestionText("");
    setQuestionType("text");
    setOptionsText("");
  };

  const removeQuestion = (id) => {
    setQuestions(questions.filter(q => q.id !== id));
  };

  const handleResponseChange = (id, value) => {
    setResponses(prev => ({ ...prev, [id]: value }));
  };

  const handleCheckboxChange = (id, value) => {
    const current = responses[id] || [];
    if (current.includes(value)) {
      setResponses({ ...responses, [id]: current.filter(v => v !== value) });
    } else {
      setResponses({ ...responses, [id]: [...current, value] });
    }
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    setSubmitted(true);
  };

  return (
    <div className="p-6 max-w-3xl mx-auto">
      <div className="mb-6">
        <button
          className="mr-4 px-4 py-2 bg-blue-500 text-white rounded"
          onClick={() => setMode("build")}
        >
          Build Mode
        </button>
        <button
          className="px-4 py-2 bg-green-500 text-white rounded"
          onClick={() => setMode("fill")}
        >
          Fill Mode
        </button>
      </div>

      {mode === "build" && (
        <div>
          <h2 className="text-xl font-bold mb-4">Create Survey</h2>
          <input
            className="border p-2 w-full mb-2"
            placeholder="Question text"
            value={questionText}
            onChange={e => setQuestionText(e.target.value)}
          />
          <select
            className="border p-2 w-full mb-2"
            value={questionType}
            onChange={e => setQuestionType(e.target.value)}
          >
            <option value="text">Text</option>
            <option value="radio">Radio</option>
            <option value="checkbox">Checkbox</option>
          </select>
          {questionType !== "text" && (
            <input
              className="border p-2 w-full mb-2"
              placeholder="Comma-separated options"
              value={optionsText}
              onChange={e => setOptionsText(e.target.value)}
            />
          )}
          <button
            className="bg-blue-600 text-white px-4 py-2 rounded mb-4"
            onClick={addQuestion}
          >
            Add Question
          </button>

          <ul className="space-y-2">
            {questions.map(q => (
              <li key={q.id} className="border p-4 rounded">
                <div className="flex justify-between">
                  <span>{q.text} ({q.type})</span>
                  <button
                    className="text-red-500"
                    onClick={() => removeQuestion(q.id)}
                  >
                    Remove
                  </button>
                </div>
                {q.options.length > 0 && (
                  <ul className="text-sm text-gray-600 mt-1">
                    {q.options.map((opt, idx) => (
                      <li key={idx}>- {opt}</li>
                    ))}
                  </ul>
                )}
              </li>
            ))}
          </ul>
        </div>
      )}

      {mode === "fill" && (
        <div>
          <h2 className="text-xl font-bold mb-4">Fill Survey</h2>
          <form onSubmit={handleSubmit} className="space-y-4">
            {questions.map(q => (
              <div key={q.id}>
                <label className="font-semibold block mb-1">{q.text}</label>
                {q.type === "text" && (
                  <input
                    className="border p-2 w-full"
                    type="text"
                    value={responses[q.id] || ""}
                    onChange={e => handleResponseChange(q.id, e.target.value)}
                  />
                )}
                {q.type === "radio" && q.options.map((opt, idx) => (
                  <label key={idx} className="block">
                    <input
                      type="radio"
                      name={`radio-${q.id}`}
                      value={opt}
                      checked={responses[q.id] === opt}
                      onChange={() => handleResponseChange(q.id, opt)}
                    /> {opt}
                  </label>
                ))}
                {q.type === "checkbox" && q.options.map((opt, idx) => (
                  <label key={idx} className="block">
                    <input
                      type="checkbox"
                      value={opt}
                      checked={(responses[q.id] || []).includes(opt)}
                      onChange={() => handleCheckboxChange(q.id, opt)}
                    /> {opt}
                  </label>
                ))}
              </div>
            ))}
            <button
              type="submit"
              className="bg-green-600 text-white px-4 py-2 rounded"
            >
              Submit Survey
            </button>
          </form>

          {submitted && (
            <div className="mt-6">
              <h3 className="text-lg font-bold mb-2">Survey Responses:</h3>
              <ul className="space-y-2">
                {questions.map(q => (
                  <li key={q.id}>
                    <strong>{q.text}:</strong> {Array.isArray(responses[q.id]) ? responses[q.id].join(", ") : responses[q.id] || "(no response)"}
                  </li>
                ))}
              </ul>
            </div>
          )}
        </div>
      )}
    </div>
  );
}

```
### App.css

body {
  font-family: sans-serif;
  background-color: #f8f9fa;
  margin: 0;
  padding: 20px;
}

.container {
  max-width: 720px;
  margin: 0 auto;
  padding: 20px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

h2 {
  font-size: 1.5rem;
  margin-bottom: 1rem;
}

input[type="text"],
select,
textarea {
  width: 100%;
  padding: 8px;
  margin-bottom: 0.75rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  cursor: pointer;
  border: none;
  border-radius: 4px;
  padding: 10px 16px;
  font-weight: bold;
}

button:hover {
  opacity: 0.9;
}

button:focus {
  outline: none;
  box-shadow: 0 0 0 3px rgba(100, 149, 237, 0.3);
}

button.build-btn {
  background-color: #007bff;
  color: white;
  margin-right: 1rem;
}

button.fill-btn {
  background-color: #28a745;
  color: white;
}

button.add-btn {
  background-color: #0056b3;
  color: white;
  margin-bottom: 1rem;
}

button.remove-btn {
  color: #dc3545;
  background: none;
  border: none;
  font-weight: bold;
  float: right;
  margin-left: auto;
}

.question-item {
  border: 1px solid #ddd;
  border-radius: 6px;
  padding: 12px;
  margin-bottom: 0.75rem;
  background-color: #fff;
}

.response-summary {
  background-color: #f1f3f5;
  padding: 12px;
  border-radius: 6px;
  margin-top: 1rem;
}

## OUTPUT
![alt text](<img/Screenshot 2025-05-05 114116.png>)
![alt text](<img/Screenshot 2025-05-05 114136.png>)
![alt text](<img/Screenshot 2025-05-05 114153.png>)
## RESULT
The program for survey building app is created successfully.
