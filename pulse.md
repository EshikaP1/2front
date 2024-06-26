---
layout: default
comments: true
title: Pulse
---

<div style="text-align: center; padding: 20px;">
    <h1 style="color: #FF66CC; font-size: 36px;">Welcome to the Pulse page!</h1>
    <p style="color: #FF66CC; font-size: 18px;">Here you can store information about your heartrate and how much you exercise!!</p>
</div>
<div style="display: flex; justify-content: center; margin-bottom: 30px;">
    <div style="width: 50%; background-color: #FFECF2; padding: 20px; border-radius: 15px;">
        <h2 style="color: #FF66CC;">Enter your pulse number to see on average how much exercise people do to have that heartrate</h2>
        <form id="formToGetOnePulseDetail">
            <label for="Pulse" style="color: #FF66CC;">Pulse number to get details:</label><br />
            <input type="text" id="Pulse" name="Pulse" style="padding: 8px; border-radius: 5px; margin-bottom: 10px;"><br />
            <button type="submit" value="btnToGetPulseDetail" id="get_pulsey" style="background-color: #FF66CC; color: white; padding: 10px 15px; border: none; border-radius: 5px; cursor: pointer;">Submit</button>
            <div id="getOnePulseDetailResponse" style="color: #FF66CC; font-size: 18px;"></div>
        </form>
    </div>
</div>
<div style="display: flex; justify-content: center;">
    <div style="width: 50%; background-color: #FFECF2; padding: 20px; border-radius: 15px;">
        <h2 style="color: #FF66CC;">Add a new pulse number</h2>
        <form id="myForm">
            <label for="Pulse" style="color: #FF66CC;">What is your pulse rate now?</label><br />
            <input type="text" id="Pulse" name="Pulse" style="padding: 8px; border-radius: 5px; margin-bottom: 10px;"><br />
            <label for="Exercise" style="color: #FF66CC;">What level of exercise did you just do? - 1 for none, 2 for mild, 3 for extreme</label><br />
            <input type="text" id="Exercise" name="Exercise" style="padding: 8px; border-radius: 5px; margin-bottom: 10px;"><br />
            <button type="submit" value="Submit" id="create_pulsey" style="background-color: #FF66CC; color: white; padding: 10px 15px; border: none; border-radius: 5px; cursor: pointer;">Submit</button>
        </form>
    </div>
</div>
<div style="text-align: center; margin-top: 30px;">
    <h2 style="color: #FF66CC;">Pulse Data from all users</h2>
    <table id="PulseTable" style="border-collapse: collapse; width: 80%; margin: 0 auto;">
        <thead>
            <tr>
                <th style="background-color: #FF66CC; color: white; padding: 10px;">Pulse Number</th>
                <th style="background-color: #FF66CC; color: white; padding: 10px;">Exercise</th>
                <th style="background-color: #FF66CC; color: white; padding: 10px;">Edit</th>
                <th style="background-color: #FF66CC; color: white; padding: 10px;">Delete</th>
            </tr>
        </thead>
        <tbody></tbody>
    </table>
</div>
<div id="editModalBackdrop" class="modal-backdrop">
    <div id="editModal" class="modal-content">
        <button id="closeModal" class="close-modal">X</button>
        <form id="editForm">
            <label for="editActive" style="color: #FF66CC;">Pulse Number</label><br />
            <input type="text" id="editActive" name="editActive" style="padding: 8px; border-radius: 5px; margin-bottom: 10px;"><br />
            <label for="editExercise" style="color: #FF66CC;">Exercise</label><br />
            <input type="text" id="editExercise" name="editExercise" style="padding: 8px; border-radius: 5px; margin-bottom: 10px;"><br />
            <input type="submit" value="Update" style="background-color: #FF66CC; color: white; padding: 10px 15px; border: none; border-radius: 5px; cursor: pointer;">
        </form>
    </div>
</div>
<style>
    #PulseTable {
        width: 100%;
        border-collapse: collapse;
        margin-top: 20px;
    }
    #PulseTable th, #PulseTable td {
        border: 1px solid #ddd;
        padding: 8px;
        text-align: left;
    }
    #PulseTable th {
        background-color: #F2F2F2;
    }
    body {
        background-color: #FFE6F2; 
        font-family: Arial, sans-serif; 
    }
    .modal-backdrop {
    }
    .modal-content {
        color: white; 
    }
    form label {
        font-weight: bold;
        margin-bottom: 5px;
    }
    button {
        background-color: #FF66CC; 
        color: white;
        padding: 10px 15px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }
    #PulseTable button {
        background-color: #FF3399; 
        color: white;
        padding: 5px 10px;
        margin: 2px;
        border: none;
        border-radius: 3px;
        cursor: pointer;
    }
</style>
<style>
    .modal-backdrop {
        display: none;
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(255, 192, 203, 0.7); 
        z-index: 1;
    }
    .modal-content {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        background: #FF66CC; 
        padding: 40px;
        z-index: 2;
    }
    .close-modal {
        position: absolute;
        top: 10px;
        right: 10px;
        cursor: pointer;
        background: none;
        border: none;
        font-size: 24px;
        color: white;
    }
    .wrapper,
    section {
        max-width: 900px;
    }
</style>

<!-- Start base code given by teacher. -->
<script type="module">
    import { uri, options } from '{{site.baseurl}}/assets/js/api/config.js';
    const API_URL = uri + '/api/pulses/';
    const createbutton = document.getElementById("create_pulsey");
    createbutton.addEventListener("click", submitForm);
    const getPulsebutton = document.getElementById("get_pulsey");
    getPulsebutton.addEventListener("click", getOnePulse);
    function loadItems() {
        fetch(API_URL, {
            method: 'GET'
        })
        .then((response) => response.json())
        .then(data => {
            displayItems(data);
        })
        .catch(error => {
            console.error("Error calling get all pulses:", error);
        });
    }
    function displayItems(items) {
        const table = document.getElementById("PulseTable");
        items.forEach((pulse) => {
            const row = table.insertRow();
            row.setAttribute("data-id", pulse.id);
            ["Active", "Exercise"].forEach((field) => {
                const cell = row.insertCell();
                cell.innerText = pulse[field];
            });
            const editCell = row.insertCell();
            const editButton = document.createElement("button");
            editButton.innerHTML = "Edit";
            editButton.addEventListener("click", () => editPulse(pulse.Active, pulse.Exercise));
            editCell.appendChild(editButton);
            const deleteCell = row.insertCell();
            const deleteButton = document.createElement("button");
            deleteButton.innerText = "Delete";
            deleteButton.addEventListener("click", () => deletePulse(pulse.Active,row));
            deleteCell.appendChild(deleteButton);
        })
        .catch((error) => {
            console.error('Error:', error);
        });
    }
    function editPulse(Active, Exercise) {
        console.log('Edit Pulse:', Active);
        const form = document.getElementById("editForm");
        form.querySelector("#editActive").value = Active;
        form.querySelector("#editExercise").value = Exercise;
        document.getElementById("editModalBackdrop").style.display = "block"; 
    }
    function deletePulse(Active, row) {
        console.log('Delete Pulse:', Active);
        const confirmation = prompt('Type "DELETE" to confirm.');
        if (confirmation === "DELETE") {
            const payload = {
                Active
            };
            console.log(JSON.stringify(payload));
            fetch(API_URL, {
                method: "DELETE",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(payload)
            })
            .then((response) => {
                if (response.ok) {
                    console.log("Successfully deleted",Active);
                    location.reload();
                    return response.json();
                } else {
                    alert("Server error");
                    throw new Error("Server");
                }
            })
        }
    }
    document.addEventListener("DOMContentLoaded", function () {
        loadItems();
        document.getElementById("closeModal").addEventListener("click", function () {
            document.getElementById("editModalBackdrop").style.display = "none"; 
        });
    });
    function submitForm(event) {
        event.preventDefault();
        const form = document.getElementById('myForm')
        const Active = form.elements['Pulse'].value
        const Exercise = parseInt(form.elements['Exercise'].value)
        const payload = {
            Active,
            Exercise,
        };
        console.log(JSON.stringify(payload));
        fetch(API_URL, {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify(payload),
        })
        .then((response) => {
            if (response.ok) {
                return response.json();
            } else {
                alert("Server error");
                throw new Error("Server");
            }
        })
        .then((data) => {
            location.reload();
        })
        .catch((error) => console.error("Error:", error));
    }
    function getOnePulse(event) {
        event.preventDefault();
        const form = document.getElementById('formToGetOnePulseDetail')
        const Active = form.elements['Pulse'].value
        const responseDiv = document.getElementById('getOnePulseDetailResponse')
        const URL_FOR_GetOne = API_URL + Active;
        fetch(URL_FOR_GetOne, {
            method: "GET"
        })
        .then((response) => {
            if (response.ok) {
                return response.json();
            } else {
                alert("Server error");
                throw new Error("Server");
            }
        })
        .then((data) => {
            console.log(data);
            responseDiv.textContent = " a pulse of " + data[0]['Active'] + " has an average exercise level of " + data[0]['Exercise'];
        })
        .catch((error) => console.error("Error:", error));
    }
</script>
<!-- End base code given by teacher -->
<style>
body {
    background-color: #FFE6F2; 
    font-family: Arial, sans-serif; 
    color: #661A33; 
}
h1, h2, h3 {
    color: #FF66CC; 
}
ul {
    list-style-type: none;
    padding-left: 0;
}
li::before {
    content: "•";
    color: #FF66CC; 
    font-weight: bold;
    display: inline-block;
    width: 1em;
    margin-left: -1em;
}
.container {
    border: 2px solid #FF66CC; 
    border-radius: 15px; 
    padding: 20px; 
    margin-bottom: 20px; 
}
</style>

### Enhance Your Pulse Experience
Taking care of your pulse and exercise routine is crucial for maintaining a healthy lifestyle. Here are some tips to help you make the most of your pulse data:
1. **Monitor Your Pulse Effectively**: During workouts, keep an eye on your pulse to ensure you're exercising within a safe and effective range. Use your pulse readings as a guide to adjust the intensity of your workouts and avoid overexertion.
2. **Maintain a Healthy Heart Rate**: Understanding your target heart rate zone can help you optimize your workouts for maximum benefit. Aim to keep your pulse within 50-85% of your maximum heart rate during exercise to improve cardiovascular fitness and endurance.
3. **Optimize Exercise Intensity**: Adjust your exercise intensity based on your pulse readings and fitness goals. Incorporate a mix of moderate and vigorous activities into your routine to challenge your cardiovascular system and improve overall health.
4. **Stay Informed**: Educate yourself about the relationship between pulse, exercise, and overall health. Explore reputable resources to learn more about cardiovascular fitness, pulse monitoring techniques, and the importance of regular physical activity.
By paying attention to your pulse and incorporating these tips into your routine, you can better manage your exercise regimen and support your long-term health and well-being.