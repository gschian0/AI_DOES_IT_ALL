<script>
  import { onMount } from "svelte";
  import { Configuration, OpenAIApi } from "openai";
  let outputz = [];
  let date = new Date();
  let questionString = "";
  let question = "";
  let prediction;
  let error;
  let image = "https://www.dropbox.com/s/3bitcdgjejgjaqb/zen2.jpg?raw=1";
  let g = "https://www.dropbox.com/s/3bitcdgjejgjaqb/zen2.jpg?raw=1";
  let imgVar = "https://www.dropbox.com/s/3bitcdgjejgjaqb/zen2.jpg?raw=1";
  const deepai = require("deepai"); // OR include deepai.min.js as a script tag in your HTML

  deepai.setApiKey("d23189e7-d9d8-41b8-8967-c346d860a70d");

  const configuration = new Configuration({
    organization: "org-GxtWZIH71ZIk7DgYFhDNd3QR",
    apiKey: "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"
  });

  const openai = new OpenAIApi(configuration);
  onMount(async () => {
    const engines = await openai.listEngines();
    outputz = engines.data.data; //
  });

  async function clickImage() {
    // var resp = await deepai.callStandardApi("fantasy-world-generator", {
    //   text: `${questionString}`
    // });
    // console.log(resp);
    // console.log(resp.output_url);
    // image = resp.output_url;
    const response = await openai.createImage({
      prompt: `${questionString}`,
      n: 1,
      size: "1024x1024"
    });
    image = response.data.data[0].url;
  }

  const onSubmit = async e => {
    const response = await openai.createCompletion({
      model: "text-davinci-003",
      prompt: `Create a Dall-e 2 prompt for maximum effectiveness using the subject matter supplied to create a vivid ai generated image. 
                            SUBJECT: ${question}
                            PROMPT: `,
      max_tokens: 100,
      temperature: 0.5
    });
    //console.log(response.data.choices[0].text);
    // questionData = response.;
    questionString = response.data.choices[0].text;
  };

  const makeVariation = async () => {
    let downloadedImage = download(image);
    const response = await openai.createImageVariation(
      downloadedImage,
      2,
      "1024x1024"
    );
    console.log(response);
  };
  async function download(filename, url) {
    fetch(url, {
      mode: "no-cors"
    })
      .then(response => response.blob())
      .then(blob => URL.createObjectURL(blob))
      .then(uril => {
        var link = document.createElement("a");
        link.href = uril;
        link.download = filename + ".png";
        document.body.appendChild(link);
        // link.click();
        document.body.removeChild(link);
      });
  }
  //download(image)
</script>
<html lang="">
<body>
<br>
 <div class="center">
<h1>open ai?</h1>
            <img
              class="center"
              src={g}
              alt="output"
            />

        </div>
<h1> yo gpt-3, can i kick it with davinci 003?</h1>
<h1> for stable diffusion inspiration?</h1>
<p>PROMPT inspiration : {console.log('What do you want a Dall-e 2 diffusion prompt regarding?') || question}</p>
   <form >
              <input
                type="text"
                name="question"
                placeholder="subject for Dall-e 2"
                bind:value={question}
              />
            <button on:click|preventDefault={onSubmit}>
              SUBMIT
            </button>
            <br><br>
            <h1>when prompt field fills, hit submit2</h1>
            <input
                type="text"
                name="prompt"
                placeholder="Press submit with prompt"
                bind:value={questionString}
              />
            <button on:click|preventDefault={clickImage}>
              SUBMIT2
            </button>
<br>
              <button on:click|preventDefault={makeVariation}>
              Make Variation
            </button>
            </form>
          
             <h1>{questionString}</h1>
            <br> 
             <div class="center">

            <img
              class="center"
              src={image}
              alt="output"
            />
<br>
 <img
              class="center"
              src={imgVar}
              alt="output"
            />
        </div>
        <br>
            <h1> AI MODELS AVAILABLE AT OPEN AI </h1>
            <br>
<div class="photos">
	{#each outputz as out}
	
			<p>{out.id} </p>
	
	{:else}
		<!-- this block renders when photos.length === 0 -->
		<p>loading...</p>
	{/each}
</div>

       
   
</body>
<div><footer><h3>zen</h3>
          </footer></div>

</html>


<style>
	img {
	  border: 5px solid rgb(0, 0, 0);
	}
	body {
	  margin: 0;
	  text-align: center;
	  font-family: "Helvetica Neue", sans-serif;
	  font-size: 34px;
	  line-height: 1.5;
	  color: #333;
	  background: rgb(105, 231, 246);
	  background: radial-gradient(
	    circle,
	    rgba(105, 231, 246, 1) 0%,
	    rgba(0, 115, 250, 1) 89%
	  );
	}

	button {
	  height: 4rem;
	  width: 8rem;
	  background-color: #aaa;
	  border-color: #f1c40f;
	  color: #f1c40f;
	  font-size: 1.25rem;
	  background-image: linear-gradient(45deg, #f1c40f 50%, transparent 50%);
	  background-position: 100%;
	  background-size: 400%;
	  transition: background 300ms ease-in-out;
	}
	button:hover {
	  background-position: 0;
	  color: #aaa;
	}
	.center {
	  display: block;
	  margin-left: auto;
	  margin-right: auto;
	  width: 50%;
	}
</style>
