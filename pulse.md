---
layout: default
title: Pulse
---

<div style="text-align: center; padding: 20px;">
    <h1 style="color: #ff66cc; font-size: 36px;">Welcome to the Pulse page!</h1>
    <p style="color: #ff66cc; font-size: 18px;">Here you can store information about your heartrate and how much you exercise!!</p>
</div>

<div style="display: flex; justify-content: center; margin-bottom: 30px;">
    <div style="width: 50%; background-color: #ffecf2; padding: 20px; border-radius: 15px;">
        <h2 style="color: #ff66cc;">Enter your pulse number to see on average how much exercise people do to have that heartrate</h2>
        <form id="formToGetOnePulseDetail">
            <label for="Pulse" style="color: #ff66cc;">Pulse number to get details:</label><br />
            <input type="text" id="Pulse" name="Pulse" style="padding: 8px; border-radius: 5px; margin-bottom: 10px;"><br />
            <button type="submit" value="btnToGetPulseDetail" id="get_pulsey" style="background-color: #ff66cc; color: white; padding: 10px 15px; border: none; border-radius: 5px; cursor: pointer;">Submit</button>
        </form>
    </div>
</div>

<div style="display: flex; justify-content: center;">
    <div style="width: 50%; background-color: #ffecf2; padding: 20px; border-radius: 15px;">
        <h2 style="color: #ff66cc;">Add a new pulse number</h2>
        <form id="myForm">
            <label for="Pulse" style="color: #ff66cc;">What is your pulse rate now?</label><br />
            <input type="text" id="Pulse" name="Pulse" style="padding: 8px; border-radius: 5px; margin-bottom: 10px;"><br />
            <label for="Exercise" style="color: #ff66cc;">What level of exercise did you just do? - 1 for none, 2 for mild, 3 for extreme</label><br />
            <input type="text" id="Exercise" name="Exercise" style="padding: 8px; border-radius: 5px; margin-bottom: 10px;"><br />
            <button type="submit" value="Submit" id="create_pulsey" style="background-color: #ff66cc; color: white; padding: 10px 15px; border: none; border-radius: 5px; cursor: pointer;">Submit</button>
        </form>
    </div>
</div>

<div style="text-align: center; margin-top: 30px;">
    <h2 style="color: #ff66cc;">Pulse Data from all users</h2>
    <table id="PulseTable" style="border-collapse: collapse; width: 80%; margin: 0 auto;">
        <thead>
            <tr>
                <th style="background-color: #ff66cc; color: white; padding: 10px;">Pulse Number</th>
                <th style="background-color: #ff66cc; color: white; padding: 10px;">Exercise</th>
                <th style="background-color: #ff66cc; color: white; padding: 10px;">Edit</th>
                <th style="background-color: #ff66cc; color: white; padding: 10px;">Delete</th>
            </tr>
        </thead>
        <tbody></tbody>
    </table>
</div>

<div id="editModalBackdrop" class="modal-backdrop">
	<div id="editModal" class="modal-content">
		<button id="closeModal" class="close-modal">X</button>
		<form id="editForm">
			<label for="editActive" style="color: #ff66cc;">Pulse Number</label><br />
			<input type="text" id="editActive" name="editActive" style="padding: 8px; border-radius: 5px; margin-bottom: 10px;"><br />
            <label for="editExercise" style="color: #ff66cc;">Exercise</label><br />
			<input type="text" id="editExercise" name="editExercise" style="padding: 8px; border-radius: 5px; margin-bottom: 10px;"><br />
			<input type="submit" value="Update" style="background-color: #ff66cc; color: white; padding: 10px 15px; border: none; border-radius: 5px; cursor: pointer;">
		</form>
	</div>
</div>

<style>
    /* ... (existing styles) ... */
    /* Add styles for the table */
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
        background-color: #f2f2f2;
    }
    /* Add styles for the background */
    body {
        background-color: #ffe6f2; /* Light pink background color */
        font-family: Arial, sans-serif; /* Set your preferred font */
    }
    /* Adjust the modal styles */
    .modal-backdrop {
        /* ... (existing styles) ... */
    }
    .modal-content {
        /* ... (existing styles) ... */
        color: white; /* Set the text color inside the modal */
    }
    /* Add styles for form labels */
    form label {
        font-weight: bold;
        margin-bottom: 5px;
    }
    /* Add styles for buttons */
    button {
        background-color: #ff66cc; /* Pink background color */
        color: white;
        padding: 10px 15px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }
    /* Style the edit and delete buttons in the table */
    #PulseTable button {
        background-color: #ff3399; /* Dark pink background color */
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
		background-color: rgba(255, 192, 203, 0.7); /* Light pink background color */
		z-index: 1;
	}

	.modal-content {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		background: #ff66cc; /* Pink background color */
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


<script type="module">
    import { uri, options } from '{{site.baseurl}}/assets/js/api/config.js';

    const API_URL = uri + '/api/pulses/'
    const createbutton = document.getElementById("create_pulsey")
    createbutton.addEventListener("click", submitForm);

    const getPulsebutton = document.getElementById("get_pulsey")
    getPulsebutton.addEventListener("click", getOnePulse);

    //Get all pulse items
    function loadItems() {
        fetch(API_URL, {
            method: 'GET'
            // headers: options.headers,
            //body: JSON.stringify(body),
        }) 
        .then((response) => response.json())
        .then(data => {
            displayItems(data);
        })
        .catch(error => {
            console.error("Error calling get all pulses:", error)
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
                //deleteButton.addEventListener("click", () => deletePulse(pulse.id, row));
                deleteButton.addEventListener("click", () => deletePulse(pulse.Active,row));
                deleteCell.appendChild(deleteButton);
            })
            .catch((error) => {
            console.error('Error:', error);
        });
    }

    // function to show Edit pulse form pop up
    function editPulse(Active, Exercise) {
        // Implement edit functionalityActive based on the pulseId
        console.log('Edit Pulse:', Active);
        
        const form = document.getElementById("editForm");

		form.querySelector("#editActive").value = Active;
		form.querySelector("#editExercise").value = Exercise;

        document.getElementById("editModalBackdrop").style.display = "block"; // show pop up edit modal
    }

    function deletePulse(Active, row) {
        // Implement delete functionality based on the pulseId
        console.log('Delete Pulse:', Active);

        const confirmation = prompt('Type "DELETE" to confirm.');
		if (confirmation === "DELETE") {
            const payload = {
                Active
            };
            console.log(JSON.stringify(payload))
            
            fetch(API_URL, {
                method: "DELETE",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(payload)
            })
            .then((response) => {
                if (response.ok) {
                    console.log("Successfully deleted",Active)
                    location.reload();
                    return response.json();
                } else {
                    alert("Server error");
                    throw new Error("Server");
                }
            })
		}

        // Remove the row from the table
        //row.remove();
    }

    	// Fetch users and ensure close modal interaction
	document.addEventListener("DOMContentLoaded", function () {
		loadItems();
		document.getElementById("closeModal").addEventListener("click", function () {
			document.getElementById("editModalBackdrop").style.display = "none"; // close pop up edit form
		});
	});

    //create method
    function submitForm(event) {

        event.preventDefault();
        //const formData = new FormData(event.target);
        //const pulse = formData.get("Pulse");
        //console.log("got form data Pulse")
        //const exercise = formData.get("Exercise");

        const form = document.getElementById('myForm')
        const Active = form.elements['Pulse'].value
        const Exercise = parseInt(form.elements['Exercise'].value)
        
        const payload = {
            Active,
            Exercise,
        };
        console.log(JSON.stringify(payload))

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
            location.reload()
        })
        .catch((error) => console.error("Error:", error));
    }

    //function to get details on one pulse
    function getOnePulse(event) {
        event.preventDefault();
        //const formData = new FormData(event.target);
        //const pulse = formData.get("Pulse");
        //console.log("got form data Pulse")
        //const exercise = formData.get("Exercise");

        const form = document.getElementById('formToGetOnePulseDetail')
        const Active = form.elements['Pulse'].value
        const responseDiv = document.getElementById('getOnePulseDetailResponse')


        const URL_FOR_GetOne = API_URL + Active

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
            console.log(data)
            responseDiv.textContent =  data[0]['Active'] + " has " + data[0]['exercise'] + " exercise"
        })
        .catch((error) => console.error("Error:", error));
    }
</script>

<style>
/* Add styles for markdown content */
body {
    background-color: #ffe6f2; /* Light pink background color */
    font-family: Arial, sans-serif; /* Set your preferred font */
    color: #661a33; /* Dark pink text color */
}

/* Style the headers */
h1, h2, h3 {
    color: #ff66cc; /* Dark pink header color */
}

/* Style the bullet points */
ul {
    list-style-type: none;
    padding-left: 0;
}

/* Style the list items */
li::before {
    content: "•";
    color: #ff66cc; /* Dark pink bullet color */
    font-weight: bold;
    display: inline-block;
    width: 1em;
    margin-left: -1em;
}

/* Add a border around the content */
.container {
    border: 2px solid #ff66cc; /* Dark pink border color */
    border-radius: 15px; /* Rounded corners */
    padding: 20px; /* Add padding */
    margin-bottom: 20px; /* Add margin */
}
</style>

### Enhance Your Pulse Experience

Taking care of your pulse and exercise routine is crucial for maintaining a healthy lifestyle. Here are some tips to help you make the most of your pulse data:

1. **Monitor Your Pulse Effectively**: During workouts, keep an eye on your pulse to ensure you're exercising within a safe and effective range. Use your pulse readings as a guide to adjust the intensity of your workouts and avoid overexertion.

2. **Maintain a Healthy Heart Rate**: Understanding your target heart rate zone can help you optimize your workouts for maximum benefit. Aim to keep your pulse within 50-85% of your maximum heart rate during exercise to improve cardiovascular fitness and endurance.

3. **Optimize Exercise Intensity**: Adjust your exercise intensity based on your pulse readings and fitness goals. Incorporate a mix of moderate and vigorous activities into your routine to challenge your cardiovascular system and improve overall health.

4. **Stay Informed**: Educate yourself about the relationship between pulse, exercise, and overall health. Explore reputable resources to learn more about cardiovascular fitness, pulse monitoring techniques, and the importance of regular physical activity.

By paying attention to your pulse and incorporating these tips into your routine, you can better manage your exercise regimen and support your long-term health and well-being.