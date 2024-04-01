<script>
	import Popup from './Popup.svelte';
	let isPopupOpen = false;

    const openPopup = ()=> {
        isPopupOpen = true;
    }

    function handlePopupClose() {
        isPopupOpen = false;
    }
	let audio;

  function playMusic() {
    audio.play();
  }
let level = 0
let clickCounter = 0
let streetCred = 0
let money = 0
let max = 100
let cashFlow = 1
let streetFlow = 1
let cancerFlow = 3
let rvDisabled = true
let jesseDisabled = true
let labDisabled = true
let chemoDisabled = true
let cookDisabled = false
let rvCount = 0
let jesseCount = 0
let labCount = 0 
let imgPicker = 0
let walt0 = "./images/walt0.png"
let walt1 = "./images/walt1.png"
let walt2 = "./images/walt2.png"
let walt3 = "./images/walt3.png"
let walt4 = "./images/walt4.png"
let walts =[walt0, walt1, walt2, walt3, walt4]
let cancer = 0
let chemo = 500
let rvPrice = 14
let jessePrice = 100
let labPrice = 1000
const handleCook = () => {
	streetCred += streetFlow
	money += cashFlow
	clickCounter += 1
	credCheck()
	moneyCheck()
	cancerProgression()
	treatmentCheck()
	deathCheck()
	
	
}

const handleRv = () => {
	cashFlow += 2
	streetFlow += 1
	money -= rvPrice
	rvCount += 1
	rvPrice = Math.ceil(rvPrice + 0.1 *rvPrice)
	moneyCheck()
}

const rvCheck = () =>{
	if(money >= rvPrice){
		rvDisabled = false}
	else{
		rvDisabled = true
	}
}

const credCheck = () =>{
	if(streetCred >= max){
		max += 400 * (level+1)
		level += 1
		if(level <5){
			imgPicker += 1
		}
		streetCred = 0
	}
}

const handleJesse = () => {
	cashFlow += 6
	streetFlow += 3
	money -= jessePrice
	jesseCount += 1
	jessePrice = Math.ceil(jessePrice + 0.1 *jessePrice)
	moneyCheck()
}

const moneyCheck = () =>{
	jesseCheck()
	rvCheck()
	labCheck()
	treatmentCheck()
}

const jesseCheck = () =>{
	if(money >= jessePrice){
		jesseDisabled = false}
	else {
		jesseDisabled = true
	}
}

const handleLab = () => {
	cashFlow += 15
	streetFlow += 5
	money -= labPrice
	labCount += 1
	labPrice = Math.ceil(labPrice + 0.1 *labPrice)
	moneyCheck()
}

const labCheck = () =>{
	if(money >= labPrice){
		labDisabled = false}
	else {
		labDisabled = true
	}
}
function randomIntFromInterval(min, max) { // min and max included 
  return Math.floor(Math.random() * (max - min + 1) + min)
  
}

const cancerProgression =() => {
	//if(clickCounter % 3 ===0)
	{
		const rndInt = randomIntFromInterval(0, cancerFlow)
		cancer += rndInt
		
	}
}

const handleTreatment = () => {
	cancer = 0
	money -= chemo
	if(level <= 1){
	chemo += 500 *(level)}
	else{chemo += 150 * level}
	cancerFlow += level
	moneyCheck()
}

const treatmentCheck =() =>{
	
	if(money >= chemo){
		chemoDisabled = false}
	else {
		chemoDisabled = true
	}
}
#each

const deathCheck = () =>{
	if(cancer >= 100){
		rvDisabled = true
		jesseDisabled = true
		labDisabled = true
		chemoDisabled = true
		cookDisabled = true
		
		openPopup()
		playMusic()
	}
	
}

</script>


<style>
    .container {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: flex-start; /* Adjust alignment as needed */
    }

    .column {
        flex: 1;
        margin: 0 50px;
        display: flex; /* Use flexbox */
        flex-direction: column; /* Stack items vertically */
        justify-content: center; /* Center items vertically */
        align-items: center; /* Center items horizontally */
    }
	.scroll-view {
        width: 500px; /* Adjust width as needed */
        height: 800px; /* Adjust height as needed */
        overflow: auto; /* This enables scrolling */
        
    }
	#file {
        width: 100%; /* Set the width to fill its container */
        height: 100px; /* Set the height to make it taller */
    }
	.white-text {
        color: white;
    }

    /* Style for disabled button */
    button[disabled] {
        /* Add your desired styles for disabled state */
        opacity: 0.5; /* Example: Reduce opacity */
        cursor: not-allowed; /* Example: Change cursor */}
</style>

<div class="container">
    <div class="column">
        <div></div>
		<h1>Street Cred</h1>
		<progress id="file" max="{max}" value="{streetCred}"></progress>

		<h1>Cancer</h1>
		<progress id="file" max="100" value="{cancer}"></progress>

		<button on:click={handleTreatment} disabled={chemoDisabled} style="color:white">Chemotherapy {chemo}$</button>

		

<Popup isOpen={isPopupOpen} on:close={handlePopupClose} />
<audio src="./audio/babyBlue.mp3" bind:this={audio}></audio>

    </div>
    <div class="column">
        
			<img src = "{walts[imgPicker]}" alt = "background" width="500" />
			<div></div>
			<h2> Level {level + 1} Walter White</h2>
			<div></div>
			<h2>Street Cred per click: {streetFlow}</h2>
			<div></div>
			<h2>Cash per click: {cashFlow} </h2>
			<button on:click={handleCook} disabled = {cookDisabled}>Cook</button>
			<h2> Clicks: {clickCounter}</h2>
		
		
    </div>
    <div class="column">
		<h1>{money}$</h1>
        <div class="scroll-view">
			<img src = "./images/rv.png" alt = "background" width="500" />
			<div style="display: inline-block;">
				<button on:click={handleRv} disabled={rvDisabled} class="white-text">RV {rvPrice}$</button>
				<h2>Owned: {rvCount}</h2>
			</div>
			<img src = "./images/jesse.png" alt = "background" width="500" />
			<div style="display: inline-block;">
				<button on:click={handleJesse} disabled={jesseDisabled} style="color:white">JESSE {jessePrice}$</button>
				<h2>Owned: {jesseCount}</h2>
			</div>
			<img src = "./images/lab.png" alt = "background" width="500" />
			<div style="display: inline-block;">
				<button on:click={handleLab} disabled={labDisabled} style="color:white">SUPERLAB {labPrice}$</button>
				<h2>Owned: {labCount}</h2>
			</div>
		</div>
    </div>
</div>
