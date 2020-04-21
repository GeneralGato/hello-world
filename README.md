<!DOCTYPE HTML>
<html>
<head>
<meta name = "viewport" content="width=device-width, initial-scale=1">
	<style>

		/* Slider container styles */
		
		.player
		{
			display: flex;
			justify-content: center;
			margin-top: 20px;
		}
		
		.grid-container
		{
			display: inline-grid;
			grid-template-columns: 50px 50px 50px 50px 50px;
			background-color: transparent;
			padding: 10px;
			align-items: center;
		}
		
		.grid-item
		{
			display: flex;
			justify-content: center;
			background-color: transparent;
			border: 1px solid transparent;
			padding: 50px 0px;
		}
		
		/* Slider styles */
		
		input[type=range]
		{
			-webkit-appearance: none;
			-webkit-transform:rotate(270deg);
			-moz-transform: rotate(270deg);
		}

		input[type=range]::-webkit-slider-runnable-track 
		{
			width: 175px;
			height: 8px;
			background: #ccc;
			border: none;
			border-radius: 8px;
			opacity: 0.7;
			-webkit-transition: .2s;
			transition: opacity .2s;
		}
		
		input[type=range]::-webkit-slider-thumb 
		{
			-webkit-appearance: none;
			border: none;
			height: 16px;
			width: 16px;
			border-radius: 50%;
			background: #ffbf00;
			margin-top: -4px;
		}

		input[type=range]:focus 
		{
			outline: none;
		}
		
		input[type=range]:hover::-webkit-slider-runnable-track 
		{
			opacity: 1;
		}
		
		/* Mozzila design */
		
		input[type=range]::-moz-range-track 
		{
			width: 175px;
			height: 8px;
			background: #ccc;
			border: none;
			border-radius: 8px;
			-webkit-transition: .2s;
			transition: opacity .2s;
		}
		
		input[type=range]::-moz-range-thumb
		{
			-webkit-appearance: none;
			border: none;
			height: 16px;
			width: 16px;
			border-radius: 50%;
			background: #ffbf00;
			margin-top: -4px;
		}
		
		/* Button contaier */
		.functions-grid
		{
			display: inline-grid;
			padding: 10px;
		}
		
		.button 
		{
			width: 15px;
			height: 15px;
			border-radius: 50%;
			color: #fcad26;
			background-color: white;
			cursor: pointer;
			transition: all .2s;
		}

		.button:hover 
		{
			background: #ffbf00;
			color: #fff;
		}
		
		.button.muteButton::after
		{
			content: '\f026';
			font-family: FontAwesome;
			padding-left: 10px;
		}
		
	</style>
	<script src="AudioBufferManager.js"></script>
</head>
	<body>
		<div class="player">
			<div class="functions-grid">
				<button id="muteButton" title="Mute" class="button"></button>
				<button id="resetButton" title="Reset to default" class="button"></button>
				<button id="presetButtonS" title="Save Preset" class="button"></button>
				<button id="presetButtonL" title="Load Preset" class="button"></button>
			</div>
				
			<div class="grid-container">
				
				<div class="grid-item">
					<input type="range" style="position: relative" min="0" max="100" value="50" id="myRange1">
						<div style="font-family: sans-serif; position: absolute; top: 150px;">
							<p><span id="volume1"></span>%</p>
						</div>
				</div>
					
				<div class="grid-item">
					<input type="range" style="position: relative" min="0" max="100" value="50" id="myRange2">
						<div style="font-family: sans-serif; position: absolute; top: 150px">
							<p><span id="volume2"></span>%</p>
						</div>
				</div>
					
				<div class="grid-item">
					<input type="range" style="position: relative" min="0" max="100" value="50" id="myRange3">
						<div style="font-family: sans-serif; position: absolute; top: 150px">
							<p><span id="volume3"></span>%</p>
						</div>
				</div>
					
				<div class="grid-item">
					<input type="range" style="position: relative" min="0" max="100" value="50" id="myRange4">
						<div style="font-family: sans-serif; position: absolute; top: 150px">
							<p><span id="volume4"></span>%</p>
						</div>
				</div>
					
				<div class="grid-item">
					<input type="range" style="position: relative" min="0" max="100" value="50" id="myRange5">
						<div style="font-family: sans-serif; position: absolute; top: 150px">
							<p><span id="volume5"></span>%</p>
						</div>
				</div>
			</div>
		</div>
		<script>
			var audio = [];
			var muteButton;
			var slider = [];
			var output = [];
				
			/* Initialization of audio */
			function initAudioPlayer()
			{
				for(var i=1; i<=5; i++)
				{
					audio[i] = new Audio();
					audio[i].loop = true;
				}
				audio[1].src = "Sub-Bass.mp3";
				audio[2].src = "Low-Mid.mp3";
				audio[3].src = "Mid.mp3";
				audio[4].src = "High-Mid.mp3";
				audio[5].src = "High.mp3";
				for(var i=1; i<=5; i++)
				{
					audio[i].volume = 0.5;
					audio[i].play();
				}
				
				/* Slider, volume and value display setup */
				for(var i=1; i<=5; i++) 
				{
					slider[i] = document.getElementById("myRange" + i);
					output[i] = document.getElementById("volume" + i);
					slider[i].addEventListener("mousemove", setvolume);
					output[i].innerHTML = slider[i].value;
				}
				function setvolume()
				{
					for(var i=1; i<=5; i++)
					{
						audio[i].volume = slider[i].value / 100;
					}
				}
				
				/* Temporary output */
				slider[1].oninput = function()
				{
					output[1].innerHTML = this.value;
				}
				slider[2].oninput = function()
				{
					output[2].innerHTML = this.value;
				}
				slider[3].oninput = function()
				{
					output[3].innerHTML = this.value;
				}
				slider[4].oninput = function()
				{
					output[4].innerHTML = this.value;
				}
				slider[5].oninput = function()
				{
					output[5].innerHTML = this.value;
				}
				
				/* Mute function execution */
				muteButton = document.getElementById("muteButton");
				muteButton.addEventListener("click", mute);

				function mute()
				{
					for(var i=1; i<=5; i++)
					{
						if(audio[i].muted)
						{
							audio[i].muted = false;
						} 
						else 
						{
							audio[i].muted = true;
						}
					}
				}
				
				/* Reset function execution */
				resetButton = document.getElementById("resetButton");
				resetButton.addEventListener("click", reset);
				
				function reset()
				{
					for(var i=1; i<=5; i++)
					{
						audio[i].volume = 0.5;
						slider[i].value = 50;
						output[i].innerHTML = 50;
					}
				}
				
				/* Save preset function execution */
				
				presetButtonS = document.getElementById("presetButtonS");
				presetButtonS.addEventListener("click", savePreset);
				var preset = [];
				
				function savePreset()
				{
					for(var i=1; i<=5; i++)
					{
						preset[i] = slider[i].value;
					}
				}
				
				/* Load preset function execution */
				
				presetButtonL = document.getElementById("presetButtonL");
				presetButtonL.addEventListener("click", loadPreset);
				
				function loadPreset()
				{
					for(var i=1; i<=5; i++)
					{
						audio[i].volume = preset[i] / 100;
						slider[i].value = preset[i];
						output[i].innerHTML = preset[i];
					}
				}
			}	
			window.addEventListener("load", initAudioPlayer);
			
		</script>
	</body>
</html>
