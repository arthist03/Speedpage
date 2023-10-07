<script>
  // import { Label, Input, Range } from "flowbite-svelte";
  // import Intro from '$lib/Intro.svelte';
  // import Copy from '$lib/Copy.svelte';
  // import { error } from "@sveltejs/kit";

  export let data;
  let pageInfo = "";
  let cruxMetrics = {};
  let lighthouseMetrics = {};
  let inputUrl = "";

  async function run() {
    const url = setUpQuery(inputUrl);
    try {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error(`API request failed with status ${response.status}`);
      }
      const json = await response.json();
      showInitialContent(json.id);
      cruxMetrics = {
        "First Contentful Paint":
          json.loadingExperience.metrics.FIRST_CONTENTFUL_PAINT_MS.category,
        "First Input Delay":
          json.loadingExperience.metrics.FIRST_INPUT_DELAY_MS.category,
      };
      lighthouseMetrics = {
        "First Contentful Paint":
          json.lighthouseResult.audits["first-contentful-paint"]
            ?.displayValue || "N/A",
        "Speed Index":
          json.lighthouseResult.audits["speed-index"]?.displayValue || "N/A",
        "Time To Interactive":
          json.lighthouseResult.audits["interactive"]?.displayValue || "N/A",
        "First Meaningful Paint":
          json.lighthouseResult.audits["first-meaningful-paint"]
            ?.displayValue || "N/A",
        "First CPU Idle":
          json.lighthouseResult.audits["first-cpu-idle"]?.displayValue || "N/A",
        "Estimated Input Latency":
          json.lighthouseResult.audits["estimated-input-latency"]
            ?.displayValue || "N/A",
      };
    } catch (error) {
      console.log("Error:", error);
    }
  }

  function setUpQuery(url) {
    const api = "https://www.googleapis.com/pagespeedonline/v5/runPagespeed";
    const parameters = {
      url: encodeURIComponent(url),
    };
    let query = `${api}?`;
    for (const key in parameters) {
      query += `${key}=${parameters[key]}&`;
    }
    return query;
  }

  function showInitialContent(id) {
    pageInfo = `${id}`;
  }
</script>

<!-- <Intro heading={data.meta.title} description={data.meta.description} /> -->

<div class="container">
  <div class="heading">
    <h1 class="animate-charcter">SpeedPage</h1>
  </div>
  <br />
  <div class="draw">
    <h2>
      <span>Introducing</span>
      <span>Speedage,</span>
      <span>your</span>
      <span>all-in-one</span>
      <span>solution</span>
      <span>for</span>
      <span>optimizing</span>
      <span>your</span>
      <span>website's</span>
      <span>speed</span>
      <span>and</span>
      <span>performance.</span>
      <span>With</span>
      <span>Speedage,</span>
      <span>you</span>
      <span>can</span>
      <span>easily</span>
      <span>gauge</span>
      <span>the</span>
      <span>speed</span>
      <span>of</span>
      <span>your</span>
      <span>website</span>
      <span>and</span>
      <span>gain</span>
      <span>valuable</span>
      <span>insights</span>
      <span>into</span>
      <span>its</span>
      <span>performance</span>
      <span>metrics.</span>
      <span>We</span>
      <span>go</span>
      <span>beyond</span>
      <span>just</span>
      <span>assessment;</span>
      <span>our</span>
      <span>tool</span>
      <span>offers</span>
      <span>actionable</span>
      <span>recommendations</span>
      <span>to</span>
      <span>enhance</span>
      <span>your</span>
      <span>site's</span>
      <span>loading</span>
      <span>response.</span>
    </h2>
  </div>
  <div class="insert">
    <label for="urlInput" class="tag">Enter URL:</label>
    <input type="text" class="w3-input" id="urlInput" bind:value={inputUrl} />

    <button on:click={run} class="btn">Search</button>
  </div>
  <img
    src="C:\Users\Kadiya\devstar\src\routes\(tools)\speedpage\bg.jpg"
    alt=""
  />
</div>
<div class="inTheend">
  <p class="Website">Website: {pageInfo}</p>

  <h2 class="Op">User Experience Report Results</h2>
  <ul class="out">
    {#each Object.keys(cruxMetrics) as key}
      <li>{key}: {cruxMetrics[key]}</li>
    {/each}
  </ul>

  <h2 class="Op">Lighthouse Results</h2>
  <ul class="out">
    {#each Object.keys(lighthouseMetrics) as key}
      <li>{key}: {lighthouseMetrics[key]}</li>
    {/each}
  </ul>
</div>

<style>
  .heading {
    display: block;
    text-align: center;
    margin: 10px 0px;
    /* border: 2px solid black */
  }
  .animate-charcter {
    text-align: center;
    text-transform: uppercase;
    background-image: linear-gradient(
      -225deg,
      #231557 0%,
      #d3aaff 29%,
      #ff1361 67%,
      #9c6bff 100%
    );
    background-size: auto auto;
    background-clip: border-box;
    background-size: 200% auto;
    color: #fff;
    background-clip: text;
    text-fill-color: transparent;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    animation: textclip 2s linear infinite;
    display: inline-block;
    margin: 0px 10px;
    text-align: center;
    font-size: 100px;
  }

  @keyframes textclip {
    to {
      background-position: 200% center;
    }
  }

  .draw {
    margin-top: 0px;
    align-items: center;
    padding: 10px;
    font-family: "Montserrat Medium";
    max-width: auto;
    text-align: center;
    transform: scale(0.94);
    animation: scale 3s forwards cubic-bezier(0.5, 1, 0.89, 1);
    /* border: 2px solid black; */
    margin: auto;
    margin-bottom: 15px;
    width: 75%;
  }
  @keyframes scale {
    100% {
      transform: scale(1);
    }
  }

  span {
    display: inline-block;
    opacity: 0;
    filter: blur(4px);
  }

  span:nth-child(1) {
    animation: fade-in 0.8s 0.1s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(2) {
    animation: fade-in 0.8s 0.2s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(3) {
    animation: fade-in 0.8s 0.3s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(4) {
    animation: fade-in 0.8s 0.4s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(5) {
    animation: fade-in 0.8s 0.5s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(6) {
    animation: fade-in 0.8s 0.6s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(7) {
    animation: fade-in 0.8s 0.7s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(8) {
    animation: fade-in 0.8s 0.8s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(9) {
    animation: fade-in 0.8s 0.9s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(10) {
    animation: fade-in 0.8s 1s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(11) {
    animation: fade-in 0.8s 1.1s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(12) {
    animation: fade-in 0.8s 1.2s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(13) {
    animation: fade-in 0.8s 1.3s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(14) {
    animation: fade-in 0.8s 1.4s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(15) {
    animation: fade-in 0.8s 1.5s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(16) {
    animation: fade-in 0.8s 1.6s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(17) {
    animation: fade-in 0.8s 1.7s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(18) {
    animation: fade-in 0.8s 1.8s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(19) {
    animation: fade-in 0.8s 1.9s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(20) {
    animation: fade-in 0.8s 2s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(21) {
    animation: fade-in 0.8s 2.1s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(22) {
    animation: fade-in 0.8s 2.2s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(23) {
    animation: fade-in 0.8s 2.3s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(24) {
    animation: fade-in 0.8s 2.4s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(25) {
    animation: fade-in 0.8s 2.5s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(26) {
    animation: fade-in 0.8s 2.6s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(27) {
    animation: fade-in 0.8s 2.7s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(28) {
    animation: fade-in 0.8s 2.8s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(29) {
    animation: fade-in 0.8s 2.9s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(30) {
    animation: fade-in 0.8s 3s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(31) {
    animation: fade-in 0.8s 3.1s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(32) {
    animation: fade-in 0.8s 3.2s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(33) {
    animation: fade-in 0.8s 3.3s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(34) {
    animation: fade-in 0.8s 3.4s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(35) {
    animation: fade-in 0.8s 3.5s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(36) {
    animation: fade-in 0.8s 3.6s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(37) {
    animation: fade-in 0.8s 3.7s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(38) {
    animation: fade-in 0.8s 3.8s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(39) {
    animation: fade-in 0.8s 3.9s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(40) {
    animation: fade-in 0.8s 4s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(41) {
    animation: fade-in 0.8s 4.1s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(42) {
    animation: fade-in 0.8s 4.2s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(43) {
    animation: fade-in 0.8s 4.3s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(44) {
    animation: fade-in 0.8s 4.4s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(45) {
    animation: fade-in 0.8s 4.5s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(46) {
    animation: fade-in 0.8s 4.6s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }
  span:nth-child(47) {
    animation: fade-in 0.8s 4.7s forwards cubic-bezier(0.11, 0, 0.5, 0);
  }

  @keyframes fade-in {
    100% {
      opacity: 1;
      filter: blur(0);
    }
  }

  .insert {
    padding: 10px;
    font-size: 20px;
    /* border:2px solid black; */
  }

  #urlInput {
    border-radius: 10px;
    width: 60%;
    margin: 10px;
  }

  .inTheend {
    border: 2px solid black;
    width: 100%;
    padding: 10px;
    display: block;
    align-items: center;
    margin: left;
    margin-top: 10px;
  }

  .Website {
    color: black;;
    background-color: beige;
    align-items: left;
    justify-content: left;
    padding: 15px;
  }

  .Op {
    align-items: center;
    padding: 15px;
  }
  .out {
    color: #000;
    border: 2px solid black;
    background-color: beige;
    align-items: center;
    padding: 15px;
  }

  .btn {
    text-align: center;
    text-transform: uppercase;
    cursor: pointer;
    font-size: 15px;
    letter-spacing: 5px;
    position: relative;
    background-color: #5da016;
    border: none;
    color: #fff;
    padding: 20px;
    width: 200px;
    height: 55px;
    text-align: center;
    transition-duration: 0.4s;
    overflow: hidden;
    box-shadow: 0 5px 15px #193047;
    border-radius: 20px;
  }

  .btn:hover {
    background: #fff;
    box-shadow: 0px 2px 10px 5px #1abc9c;
    color: #000;
  }

  .btn:after {
    content: "";
    background: #1abc9c;
    display: block;
    position: absolute;
    padding-top: 300%;
    padding-left: 350%;
    margin-left: -20px !important;
    margin-top: -120%;
    opacity: 0;
    transition: all 0.8s;
  }

  .btn:active:after {
    padding: 0;
    margin: 0;
    opacity: 1;
    transition: 0s;
  }

  .btn:focus {
    outline: 0;
  }
</style>
