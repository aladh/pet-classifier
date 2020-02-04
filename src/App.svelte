<script>
	var el = x => document.getElementById(x);

	function showPicker(inputId) { el('file-input').click(); }

	function showPicked(event) {
		let input = event.target;
		el('upload-label').innerHTML = input.files[0].name;
		var reader = new FileReader();
		reader.onload = function (e) {
			el('image-picked').src = e.target.result;
			el('image-picked').className = '';
		}
		reader.readAsDataURL(input.files[0]);
	}

	function analyze() {
		var uploadFiles = el('file-input').files;
		if (uploadFiles.length != 1) alert('Please select 1 file to analyze!');

		el('analyze-button').innerHTML = 'Analyzing...';
		var xhr = new XMLHttpRequest();
		xhr.open('POST', 'analyze', true);
		xhr.onerror = function() {alert (xhr.responseText);}
		xhr.onload = function(e) {
			if (this.readyState === 4) {
				let {prediction, confidence} = JSON.parse(e.target.responseText);

				el('prediction').innerHTML = `Prediction: ${prediction}`;
				el('confidence').innerHTML = `Confidence: ${confidence}%`;
			}
			el('analyze-button').innerHTML = 'Analyze';
		}

		var fileData = new FormData();
		fileData.append('file', uploadFiles[0]);
		xhr.send(fileData);
	}
</script>

<div>
	<div class='center'>
		<div class='title'>Pet Classifier</div>
		<div>Classify a photo of a dog or cat into one of 37 breeds</div>
		<div class='content'>
			<div class='no-display'>
				<input id='file-input'
					   class='no-display'
					   type='file'
					   name='file'
					   accept='image/*'
					   on:change={showPicked}>
			</div>
			<button class='choose-file-button' type='button' on:click={showPicker}>Select Image</button>
			<div class='upload-label'>
				<label id='upload-label'>No file chosen</label>
			</div>
			<div>
				<img id='image-picked' class='no-display' alt='Chosen Image' height='200'>
			</div>
			<div class='analyze'>
				<button id='analyze-button' class='analyze-button' type='button' on:click={analyze}>Analyze</button>
			</div>
			<div class='result-label'>
				<p id="prediction"></p>
				<p id="confidence"></p>
			</div>
		</div>
	</div>
</div>

<style>
	.no-display {
		display: none;
	}

	.center {
		margin: auto;
		padding: 10px;
		padding-left:50px;
		padding-right:50px;
		text-align: center;
		font-size: 14px;
	}

	.title {
		font-size: 30px;
		margin-top: 1em;
		margin-bottom: 1em;
		color: #262626;
	}

	.content {
		margin-top: 10em;
	}

	.analyze {
		margin-top: 5em;
	}

	.upload-label {
		padding: 10px;
		font-size: 12px;
	}

	.result-label {
		margin-top: 0.5em;
		padding: 10px;
		font-size: 13px;
	}

	button.choose-file-button {
		width: 200px;
		height: 40px;
		border-radius: 2px;
		background-color: #ffffff;
		border: solid 1px #7052CB;
		font-size: 13px;
		color: #7052CB;
	}

	button.analyze-button {
		width: 200px;
		height: 40px;
		border: solid 1px #7052CB;
		border-radius: 2px;
		background-color: #7052CB;
		font-size: 13px;
		color: #ffffff;
	}

	button:focus {outline:0;}
</style>


