<link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">
<script lang="ts">
import data from '$lib/questions.json';
import moraleImage from '$lib/images/morale.png';
import moneyImage from '$lib/images/money.png';
import peopleImage from '$lib/images/people.png';
import materialsImage from '$lib/images/materials.png';
const questions = data.questions;
let story = "";
let peopleA = 1;
let moneyA = 200;
let moraleA = 200;
let materialsA = 1;
let num = 0;
let hidden = [false, true, true, true, true];

$: people = {
    "normal": "height:"+peopleA+"px",
    "clear": "height:"+(400-peopleA)+"px"
};
$: money = {
    "normal": "height:"+moneyA+"px",
    "clear": "height:"+(400-moneyA)+"px"
};
$: morale = {
    "normal": "height:"+moraleA+"px; object-fit:cover",
    "clear": "height:"+(400-moraleA)+"px"
}
$: materials = {
    "normal": "height:"+materialsA+"px; object-fit:cover",
    "clear": "height:"+(400-materialsA)+"px"
}
$: question = questions[num];
$: text = question.text;
$: buttonOne = question.one;
$: buttonTwo = question.two;
function one() {
    if (buttonOne.end.do) {
        end(buttonOne.end.text);
    } else {
        for (let i = 0; i < buttonOne.add.length; i++) {
            if (buttonOne.add[i].type == "people" && peopleA + buttonOne.add[i].amount < 401) {
                peopleA += buttonOne.add[i].amount;
            } else if (buttonOne.add[i].type == "money" && moneyA + buttonOne.add[i].amount < 401) {
                moneyA += buttonOne.add[i].amount;
            } else if (buttonOne.add[i].type == "materials" && materialsA + buttonOne.add[i].amount < 401) {
                materialsA += buttonOne.add[i].amount;
            } else if (buttonOne.add[i].type == "morale" && moraleA + buttonOne.add[i].amount < 401) {
                moraleA += buttonOne.add[i].amount;
            }
        }
        story += buttonOne.story;
        if (peopleA < 1) {
            end("Tu as perdu tout tes peuples.");
        } else if (moneyA < 1) {
            end("Tu as perdu tout ton argent.");
        } else if (moraleA < 1) {
            end("Tu as perdu tout ton moral.");
        } else if (materialsA < 1) {
            end("Tu as perdu tout tes matériaux.");
        } else {
            num = buttonOne.to-1;
        }
    }
}
function two() {
    if (buttonTwo.end.do) {
        end(buttonTwo.end.text);
    } else {
        for (let i = 0; i < buttonTwo.add.length; i++) {
            if (buttonTwo.add[i].type == "people" && peopleA + buttonTwo.add[i].amount < 401) {
                peopleA += buttonTwo.add[i].amount;
            } else if (buttonTwo.add[i].type == "money" && moneyA + buttonTwo.add[i].amount < 401) {
                moneyA += buttonTwo.add[i].amount;
            } else if (buttonTwo.add[i].type == "materials" && materialsA + buttonTwo.add[i].amount < 401) {
                materialsA += buttonTwo.add[i].amount;
            } else if (buttonTwo.add[i].type == "morale" && moraleA + buttonTwo.add[i].amount < 401) {
                moraleA += buttonTwo.add[i].amount;
            }
        }
        story += buttonTwo.story;
        if (peopleA < 1) {
            end("Tu as perdu tout tes peuples.");
        }  else if (moneyA < 1) {
            end("Tu as perdu tout ton argent.");
        } else if (moraleA < 1) {
            end("Tu as perdu tout ton moral.");
        } else if (materialsA < 1) {
            end("Tu as perdu tout tes matériaux.");
        } else {
            num = buttonTwo.to-1;
        }
    }
}
function restart() {
    window.location.reload();
}
function end(endText: string) {
    text = endText += "<br><br><strong>Histoire:</strong><br>" + story + " La fin.";
    hidden = [true, false, true, false, true];
}
function start() {
    hidden = [true, false, false, false, false];
}
</script>

<h1>Questions</h1>
<button hidden={hidden[0]} on:click={start}>Commence le jeu</button>
<p hidden={hidden[1]}>{@html text}</p>
<button hidden={hidden[2]} on:click={one}>{buttonOne.text}</button>
<button hidden={hidden[2]} on:click={two}>{buttonTwo.text}</button>
<br>
<button hidden={hidden[3]} on:click={restart}>Recommencer</button>
<div class="container" hidden={hidden[4]}>
    <div class="bar back">
        <div style={people.clear} class="bar"></div>
        <div style={people.normal} class="bar people left"><img style={people.normal} src={peopleImage} alt="people"></div>
    </div>
    <div class="bar back">
        <div style={money.clear} class="bar"></div>
        <div style={money.normal} class="bar money middle"><img style={money.normal} src={moneyImage} alt="money"></div>
    </div>
    <div class="bar back">
        <div style={morale.clear} class="bar"></div>
        <div style={morale.normal} class="bar morale right"><img style={morale.normal} src={moraleImage} alt="morale"></div>
    </div>
    <div class="bar back">
        <div style={materials.clear} class="bar"></div>
        <div style={materials.normal} class="bar materials right"><img style={materials.normal} src={materialsImage} alt="materials"></div>
    </div>
</div>