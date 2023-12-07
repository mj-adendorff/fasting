<script>
let startTime = Date.now();
let nowTime = Date.now();
let hours = 0;
let minutes = 0;
let seconds = 0;

let running = false;
let runner;

const zeroPad = (num) => String(num).padStart(2, '0')

function runCheck() {
    nowTime = Date.now();
    let diff = Math.abs(nowTime - startTime);
    hours = Math.floor(diff / 1000 / 3600);
    minutes = Math.floor(diff / 1000 / 60) % 60;
    seconds = Math.floor(diff/1000) % 60;
}

function startInterval() {
    runner = setInterval(() => {
        runCheck();
    }, 1000);
}

function start() {
    running = true;
    startTime = Date.now();
    localStorage.setItem("time_start", startTime + "");
    startInterval();
}


function end() {
    clearInterval(runner);
    localStorage.clear();
    running = false;
}

/* run on start */
let tempStartTime = localStorage.getItem("time_start");
if (tempStartTime !== null) {
    startTime = parseInt(tempStartTime);
    runCheck();
    startInterval();
    running = true;
}

/* initialize with a time */
if (window.location.hash.startsWith("#time=")) {
    let time = window.location.hash.substring(6);
    localStorage.setItem("time_start", time);
    startTime = parseInt(time);
}

</script>

<main>
    <div class="buttons">
        {#if running}
        <div class="button end" on:click={end} on:keydown={end}>End</div>
        {:else}
        <div class="button start" on:click={start} on:keydown={start}>Start</div>
        {/if}
    </div>
    <div class="main-container">
        {#if running}
        <div class="time">
            {zeroPad(hours)}:{zeroPad(minutes)}:{zeroPad(seconds)}
        </div>
        {:else}
        <div class="time">
           
        </div>
        {/if}
        
    </div>
</main>

<style>
    .main-container {
        color: white;
    }
    .time {
        font-family: "Arial";
        color: white;
        font-size: 50pt;
        font-weight: bold;
        font-family: "Arial";
        width: 265px;
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .button {
        width: 100%;
        height: 40px;
        display: flex;
        align-items: center;
        justify-content: center;
        font-family: "Arial";
        cursor: pointer;
    }
    .start {
        background-color: darkgreen;
        color: white;

    }
    .end {
        background-color: red;
        color: white;
    }
</style>
