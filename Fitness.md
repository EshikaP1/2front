---
layout: default
title: Fitness Recs
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fitness Recs</title>
    <style>
       body {
    background-color: #fce4ec; /* Replace #f0f0f0 with your desired background color */
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 20px;
}
.container {
            text-align: center;
            padding: 50px;
            background-color: rgb(255, 182, 193);
            border-radius: 10px;
            margin: 50px auto;
            max-width: 600px;
        }
        h1 {
            color: #333;
        }
        input[type="text"],
        input[type="number"] {
            width: 100%;
            padding: 10px;
            margin-bottom: 10px;
        }
        #save-button {
            background-color: #8257B4;
            color: #fff;
            border: none;
            padding: 10px 20px;
            cursor: pointer;
        }
        #weekly-log {
            text-align: left;
            margin-top: 20px;
            padding: 10px;
            background-color: #fff;
            border-radius: 5px;
            box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.3);
        }
        #weekly-log table {
            width: 100%;
            border-collapse: collapse;
        }
        #weekly-log th, #weekly-log td {
            padding: 8px;
            border-bottom: 1px solid #ddd;
        }
        .dropdown {
            display: inline-block;
            width: 100%;
        }
        .dropdown select {
            width: 100%;
            padding: 10px;
        }
    </style>
</head>

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BMI Calculator</title>
    <style>
        /* Add your CSS styles here */
    </style>
</head>
<body>



<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RECS</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
    <div id="errorMessage"></div>
    <form id="fitnessForm">
        <p><label for="age">Age:</label>
            <input type="number" name="age" id="age" required>
        </p>
        <p><label for="weight">Weight (kg):</label>
            <input type="number" name="weight" id="weight" required>
        </p>
        <p><label for="height">Height (cm):</label>
            <input type="number" name="height" id="height" required>
        </p>
        <p><label for="gender">Gender:</label>
            <select id="gender" name="gender" required>
                <option value="male">Male</option>
                <option value="female">Female</option>
            </select>
        </p>
        <p><label for="activity_level">Activity Level:</label>
            <select id="activity_level" name="activity_level" required>
                <option value="sedentary">Sedentary</option>
                <option value="lightly_active">Lightly Active</option>
                <option value="moderately_active">Moderately Active</option>
                <option value="very_active">Very Active</option>
                <option value="extra_active">Extra Active</option>
            </select>
        </p>
        <button type="submit">Get Recommendation</button>
    </form>

<canvas id="bmiGraph"></canvas>

<script>
        document.getElementById("fitnessForm").addEventListener("submit", function(event) {
            event.preventDefault();
            const formData = new FormData(this);
            const jsonData = {};
            formData.forEach((value, key) => {jsonData[key] = value});

            fetch('http://127.0.0.1:8086/api/fitness', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(jsonData)
            })
            .then(response => response.json())
            .then(data => {
                const bmi = data.bmi;
                const recommendation = data.recommendation;

                document.getElementById('bmiGraph').style.display = 'block';
                displayBMI(bmi);
                displayRecommendation(recommendation);
            })
            .catch(error => {
                console.error('Error:', error);
                const errorMessageDiv = document.getElementById('errorMessage');
                errorMessageDiv.innerHTML = '<label style="color: green;">You need to walk more and drink a gallon of water each day</label>';
            });
        });

        function displayBMI(bmi) {
            const ctx = document.getElementById('bmiGraph').getContext('2d');
            const chart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['BMI'],
                    datasets: [{
                        label: 'Your BMI',
                        data: [bmi],
                        backgroundColor: 'rgba(75, 192, 192, 0.2)',
                        borderColor: 'rgba(75, 192, 192, 1)',
                        borderWidth: 1
                    }]
                },
                options: {
                    scales: {
                        yAxes: [{
                            ticks: {
                                beginAtZero: true
                            }
                        }]
                    }
                }
            });
        }

        function displayRecommendation(recommendation) {
            // Display the recommendation to the user
            console.log(recommendation);
        }
    </script>
</body>
</html>





<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BMI Calculator</title>
<style>
    body {
        font-family: Arial, sans-serif;
    }
    #container {
        width: 300px;
        margin: 0 auto;
        text-align: center;
        padding-top: 50px;
    }
    input[type="number"] {
        width: 100%;
        padding: 8px;
        margin: 5px 0;
        box-sizing: border-box;
    }
    button {
        background-color: #4CAF50;
        color: white;
        padding: 10px 20px;
        margin: 8px 0;
        border: none;
        cursor: pointer;
        width: 100%;
    }
    #result {
        margin-top: 20px;
    }
</style>
</head>
<body>
<div id="container">
    <h2>BMI Calculator</h2>
    <label for="weight">Weight (kg):</label>
    <input type="number" id="weight" placeholder="Enter your weight" required>
    <label for="height">Height (m):</label>
    <input type="number" id="height" placeholder="Enter your height" required>
    <button onclick="calculateBMI()">Calculate BMI</button>
    <div id="result"></div>
</div>

<script>
function calculateBMI() {
    var weight = parseFloat(document.getElementById('weight').value);
    var height = parseFloat(document.getElementById('height').value);
    
    if (isNaN(weight) || isNaN(height) || weight <= 0 || height <= 0) {
        alert("Please enter valid weight and height values.");
        return;
    }
    
    var bmi = weight / (height * height);
    displayResult(bmi);
}

function displayResult(bmi) {
    var resultDiv = document.getElementById('result');
    resultDiv.innerHTML = "<h3>Your BMI is: " + bmi.toFixed(2) + "</h3>";
    
    var category = "";
    if (bmi < 18.5) {
        category = "Underweight";
    } else if (bmi < 25) {
        category = "Normal weight";
    } else if (bmi < 30) {
        category = "Overweight";
    } else {
        category = "Obese";
    }
    
    resultDiv.innerHTML += "<p>You are " + category + "</p>";
}
</script>
</body>
</html>

<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BMI Calculator with Graph</title>
<style>
    body {
        font-family: Arial, sans-serif;
    }
    #container {
        width: 80%;
        margin: 0 auto;
        text-align: center;
    }
    #bmiGraph {
        margin-top: 50px;
    }
    #bmiTable {
        margin-top: 50px;
    }
    table {
        width: 100%;
        border-collapse: collapse;
    }
    th, td {
        border: 1px solid #dddddd;
        text-align: left;
        padding: 8px;
    }
</style>
</head>
<body>
<div id="container">
    <h1>BMI Calculator with Graph</h1>
    <div id="bmiGraph">
        <h2>BMI Ranges</h2>
        <canvas id="myChart" width="400" height="200"></canvas>
    </div>
    <div id="bmiTable">
        <h2>BMI Categories</h2>
        <table>
            <tr>
                <th>Category</th>
                <th>Description</th>
            </tr>
            <tr>
                <td>Underweight</td>
                <td>BMI &lt; 18.5</td>
            </tr>
            <tr>
                <td>Normal weight</td>
                <td>18.5 &le; BMI &lt; 25</td>
            </tr>
            <tr>
                <td>Overweight</td>
                <td>25 &le; BMI &lt; 30</td>
            </tr>
            <tr>
                <td>Obese</td>
                <td>BMI &ge; 30</td>
            </tr>
        </table>
    </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
document.addEventListener('DOMContentLoaded', function() {
    createBMIGraph();
});

function createBMIGraph() {
    var ctx = document.getElementById('myChart').getContext('2d');
    var myChart = new Chart(ctx, {
        type: 'line',
        data: {
            labels: ['Underweight', 'Normal weight', 'Overweight', 'Obese'],
            datasets: [{
                label: 'BMI Ranges',
                data: [18.5, 25, 30],
                backgroundColor: 'rgba(255, 99, 132, 0.2)',
                borderColor: 'rgba(255, 99, 132, 1)',
                borderWidth: 1
            }]
        },
        options: {
            scales: {
                yAxes: [{
                    ticks: {
                        beginAtZero: true
                    }
                }]
            }
        }
    });
}
</script>
</body>
</html>


<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fitness Recommendation Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 20px;
        }
        .question {
            margin-bottom: 20px;
        }
    </style>
</head>
<body>
    <h1>Fitness Recommendation Game</h1>
    <div id="questions">
        <div class="question" id="question1">
            <p>How often do you exercise?</p>
            <input type="radio" name="exerciseFrequency" value="rarely" id="rarely">
            <label for="rarely">Rarely</label>
            <input type="radio" name="exerciseFrequency" value="occasionally" id="occasionally">
            <label for="occasionally">Occasionally</label>
            <input type="radio" name="exerciseFrequency" value="regularly" id="regularly">
            <label for="regularly">Regularly</label>
        </div>
        <div class="question" id="question2">
            <p>What type of exercise do you prefer?</p>
            <input type="checkbox" name="exerciseType" value="cardio" id="cardio">
            <label for="cardio">Cardio</label>
            <input type="checkbox" name="exerciseType" value="weightlifting" id="weightlifting">
            <label for="weightlifting">Weightlifting</label>
            <input type="checkbox" name="exerciseType" value="yoga" id="yoga">
            <label for="yoga">Yoga</label>
        </div>
        <button onclick="showRecommendation()">Get Recommendation</button>
    </div>

<div id="recommendation" style="display: none;">
        <h2>Your Fitness Recommendation</h2>
        <p id="recommendationText"></p>
    </div>

<script>
        function showRecommendation() {
            const exerciseFrequency = document.querySelector('input[name="exerciseFrequency"]:checked');
            const exerciseTypes = document.querySelectorAll('input[name="exerciseType"]:checked');

            if (!exerciseFrequency || exerciseTypes.length === 0) {
                alert("Please answer all questions.");
                return;
            }

            const frequencyValue = exerciseFrequency.value;
            const typeValues = Array.from(exerciseTypes).map(type => type.value);

            const recommendation = generateRecommendation(frequencyValue, typeValues);
            document.getElementById('recommendationText').textContent = recommendation;
            document.getElementById('recommendation').style.display = 'block';
        }

        function generateRecommendation(frequency, types) {
            let recommendation = "Based on your answers, we recommend the following fitness routine:\n\n";

            if (frequency === 'rarely') {
                recommendation += "- Start by incorporating light cardio exercises like walking or cycling for at least 30 minutes, 3 times a week.\n";
            } else if (frequency === 'occasionally') {
                recommendation += "- Aim to engage in moderate-intensity workouts such as jogging or swimming for at least 30 minutes, 4-5 times a week.\n";
            } else if (frequency === 'regularly') {
                recommendation += "- Continue with your regular exercise routine and consider adding variety by incorporating strength training, flexibility exercises, or high-intensity interval training (HIIT).\n";
            }

            recommendation += "\nFor your preferred exercise types:\n";
            types.forEach(type => {
                if (type === 'cardio') {
                    recommendation += "- Incorporate cardio exercises like running, cycling, or aerobics to improve cardiovascular health and endurance.\n";
                } else if (type === 'weightlifting') {
                    recommendation += "- Include weightlifting or resistance training to build muscle strength and improve metabolism.\n";
                } else if (type === 'yoga') {
                    recommendation += "- Practice yoga for flexibility, stress relief, and overall well-being.\n";
                }
            });

            return recommendation;
        }
    </script>
</body>
</html>

