<script>
    import { onMount } from 'svelte';

    onMount(() => {
        loadGame();
        setInterval(() => {
            saveGame();
        }, 20000);
    });

    setInterval(() => {
        count += rate;
    }, 1000);

    let rate = 0;
    let clickRate = 1;
    let count = 0;
    let abt = 0;
    let eabt = 0;
    let bte = 0;
    let btg = 0;
    let cbp = 0;
    let showclickcount = false;
    let saveText = false;
    let ph = false;
    let tph = false;
    let ta = false;

    $: rate = abt * 8 + eabt * 15 + bte * 20 + btg * 50 + cbp * 100;

    function saveGame() {
        saveText = true;
        const gameData = {
            clickRate: clickRate,
            count: count,
            abt: abt,
            eabt: eabt,
            bte: bte,
            btg: btg,
            cbp: cbp,
            ph: ph,
            tph: tph,
            ta: ta,
        };
        localStorage.setItem('gameData', JSON.stringify(gameData));
        setTimeout(() => {
            saveText = false;
        }, 3000);
    }

    function loadGame() {
        const storedData = localStorage.getItem('gameData');
        if (storedData) {
            const gameData = JSON.parse(storedData);
            clickRate = gameData.clickRate;
            count = gameData.count;
            abt = gameData.abt;
            eabt = gameData.eabt;
            bte = gameData.bte;
            btg = gameData.btg;
            cbp = gameData.cbp;
            ph = gameData.ph;
            tph = gameData.tph;
            ta = gameData.ta;
        }
    }

    function destroyGame() {
        localStorage.removeItem('gameData');
        rate = 0;
        clickRate = 1;
        count = 0;
        abt = 0;
        eabt = 0;
        bte = 0;
        btg = 0;
        cbp = 0;
        ph = false;
        tph = false;
        ta = false;
    }

    function addBall() {
        count += clickRate;
        showclickcount = true;
        setTimeout(() => {
            showclickcount = false;
        }, 500);
    }

    function buyGadget(price, gadget) {
        if (count >= price) {
            count -= price;
            gadget++;
        }
        return { gadget, count };
    }

    function upgradeHand(price, upgrade, power) {
        if (count >= price && !upgrade) {
            count -= price;
            upgrade = true;
            clickRate += power;
        }
        return { upgrade, count };
    }
</script>

<svelte:head>
    <title>Ball Clicker</title>
    <meta name="title" content="Ball Clicker" />
    <meta name="description" content="Click the balls as fast as you can" />
    <meta name="keywords" content="balls, clicker, idle, game" />
    <meta name="robots" content="index, nofollow" />
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta name="language" content="English" />
</svelte:head>

<div class="container">
    <div class="shop">
        <div
            class="shopitem"
            role="button"
            tabindex=""
            on:click={() => {
                const result = buyGadget(50, abt);
                abt = result.gadget;
                count = result.count;
            }}
        >
            <p>Automatic Ball Toucher</p>
            <span>
                <p class={count < 50 ? 'unaffordable' : 'affordable'}>
                    50 Balls
                </p>
                <p>x{abt}</p>
            </span>
        </div>
        <div
            class="shopitem"
            role="button"
            tabindex=""
            on:click={() => {
                const result = buyGadget(300, eabt);
                eabt = result.gadget;
                count = result.count;
            }}
        >
            <p>Enhanced Automatic Ball Toucher</p>
            <span>
                <p class={count < 300 ? 'unaffordable' : 'affordable'}>
                    300 Balls
                </p>
                <p>x{eabt}</p>
            </span>
        </div>
        <div
            class="shopitem"
            role="button"
            tabindex=""
            on:click={() => {
                const result = buyGadget(500, bte);
                bte = result.gadget;
                count = result.count;
            }}
        >
            <p>Ball Touching Employee</p>
            <span>
                <p class={count < 500 ? 'unaffordable' : 'affordable'}>
                    500 Balls
                </p>
                <p>x{bte}</p>
            </span>
        </div>
        <div
            class="shopitem"
            role="button"
            tabindex=""
            on:click={() => {
                const result = buyGadget(1000, btg);
                btg = result.gadget;
                count = result.count;
            }}
        >
            <p>Ball Touching Goblin</p>
            <span>
                <p class={count < 1000 ? 'unaffordable' : 'affordable'}>
                    1000 Balls
                </p>
                <p>x{btg}</p>
            </span>
        </div>
        <div
            class="shopitem"
            role="button"
            tabindex=""
            on:click={() => {
                const result = buyGadget(10000, cbp);
                cbp = result.gadget;
                count = result.count;
            }}
        >
            <p>Counterfeit Ball Printer</p>
            <span>
                <p class={count < 10000 ? 'unaffordable' : 'affordable'}>
                    10000 Balls
                </p>
                <p>x{cbp}</p>
            </span>
        </div>
    </div>
    <div class="stage">
        <div class="information">
            <span class="rate info">{rate}/sec</span>
            <span class="clickRate info">{clickRate} Ball Power</span>
            <span class="count info">{count}</span>
        </div>
        <div class="counters">
            {#if rate > 0}
                <div class="ballFeed counter info">+{rate}</div>
            {/if}
            {#if showclickcount}
                <div class="ballClickFeed counter info">+{clickRate}</div>
            {/if}
        </div>
        <div id="button" role="button" tabindex="" on:click={addBall} />
        <div class="footer">
            <button on:click={saveGame}>Save</button>
            {#if saveText}
                <p id="savetext">Game has been saved.</p>
            {/if}
            <button on:click={destroyGame}>Reset</button>
        </div>
    </div>
    <div class="shop2">
        <div
            role="button"
            tabindex=""
            on:click={() => {
                const result = upgradeHand(250, ph, 1);
                ph = result.upgrade;
                count = result.count;
            }}
            class={ph ? 'die' : 'shopitemalt'}
        >
            <span>
                <p class={count < 250 && !ph ? 'unaffordable' : 'affordable'}>
                    250 Balls
                </p>
                {#if ph}
                    <p class="affordable">✓</p>
                {:else}
                    <p class="unaffordable">✗</p>
                {/if}
            </span>
            <p>Plastic Hand</p>
        </div>
        <div
            role="button"
            tabindex=""
            on:click={() => {
                const result = upgradeHand(1000, tph, 6);
                tph = result.upgrade;
                count = result.count;
            }}
            class={tph ? 'die' : 'shopitemalt'}
        >
            <span>
                <p class={count < 1000 && !tph ? 'unaffordable' : 'affordable'}>
                    1000 Balls
                </p>
                {#if tph}
                    <p class="affordable">✓</p>
                {:else}
                    <p class="unaffordable">✗</p>
                {/if}
            </span>
            <p>Two Plastic Hands</p>
        </div>
        <div
            role="button"
            tabindex=""
            on:click={() => {
                const result = upgradeHand(5000, ta, 8);
                ta = result.upgrade;
                count = result.count;
            }}
            class={ta ? 'die' : 'shopitemalt'}
        >
            <span>
                <p class={count < 5000 && !ta ? 'unaffordable' : 'affordable'}>
                    5000 Balls
                </p>
                {#if ta}
                    <p class="affordable">✓</p>
                {:else}
                    <p class="unaffordable">✗</p>
                {/if}
            </span>
            <p>Third Arm</p>
        </div>
    </div>
</div>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Archivo+Black&family=Black+Ops+One&family=Gabarito&family=Oxygen&family=Poppins&family=Prompt&family=Roboto:wght@900&display=swap');
    @import url('https://fonts.googleapis.com/css2?family=Fragment+Mono&display=swap');

    * {
        font-size: 1.2rem;
        font-family: gabarito, sand-serif;
        user-select: none;
    }

    .container {
        display: flex;
        flex-direction: row;
        flex: auto;
    }

    .shop {
        display: flex;
        flex: 25%;
        flex-direction: column;
    }

    .shopitem {
        display: flex;
        align-items: center;
        padding: 0rem 1rem;
        height: 100px;
        border-bottom: solid 1px #ffffff10;
        justify-content: space-between;
        cursor: pointer;
        transition: all 0.3s ease-in-out;
        text-align: right;
    }

    .disabled {
        background-color: red;
    }

    .shopitemalt {
        display: flex;
        align-items: center;
        padding: 0rem 1rem;
        height: 100px;
        border-bottom: solid 1px #ffffff10;
        justify-content: space-between;
        cursor: pointer;
        transition: all 0.3s ease-in-out;
        text-align: left;
    }

    .die {
        display: flex;
        align-items: center;
        padding: 0rem 1rem;
        height: 100px;
        border-bottom: solid 1px #ffffff10;
        justify-content: space-between;
        cursor: not-allowed;
        color: #a1a1a1;
        transition: all 0.3s ease-in-out;
        text-align: left;
    }

    .shopitem:hover,
    .shopitemalt:hover {
        background-color: #ffffff10;
        transition: all 0.3s ease-in-out;
    }

    .unaffordable {
        color: rgb(255, 137, 137);
        transition: all 0.2s ease-in-out;
    }

    .affordable {
        color: rgb(184, 255, 182);
        transition: all 0.2s ease-in-out;
    }

    .stage {
        position: relative;
        display: flex;
        flex-direction: column;
        flex: 50%;
        border-left: solid 1px #ffffff10;
        border-right: solid 1px #ffffff10;
        justify-content: center;
        align-items: center;
    }

    .information {
        display: flex;
        top: 0;
        width: 100%;
        position: absolute;
        height: 100px;
        background-color: #ffffff10;
        justify-content: space-evenly;
        align-items: center;
    }

    .info {
        font-family: 'Fragment Mono', monospace;
    }

    .counters {
        position: absolute;
        width: 200px;
        height: 30px;
        top: 20%;
        display: flex;
        justify-content: space-evenly;
    }

    .counters .counter {
        position: absolute;
        width: 200px;
        height: 200px;
        top: 20%;
    }

    #button {
        background-color: hsl(187, 59%, 46%);
        border-radius: 50%;
        width: 128px;
        height: 128px;
        cursor: pointer;
        transition: background-color 0.1s ease-in-out;
    }

    #button:hover {
        background-color: hsl(187, 60%, 56%);
        transition: background-color 0.1s ease-in-out;
    }

    #button:active {
        transform: translateY(4px);
        transition: transform 0.1s ease-in-out;
    }

    .footer {
        display: flex;
        bottom: 0;
        width: 100%;
        position: absolute;
        height: 100px;
        background-color: #ffffff10;
        justify-content: space-evenly;
        align-items: center;
    }

    #savetext {
        margin: 0;
        padding: 0;
        position: fixed;
        animation: fade 3s ease-out;
    }

    @keyframes fade {
        0% {
            opacity: 100%;
        }
        33% {
            opacity: 100%;
        }
        66% {
            opacity: 60%;
        }
        100% {
            opacity: 0%;
        }
    }

    button {
        width: 128px;
        height: 100%;
        border-radius: 5px;
        background-color: transparent;
        color: white;
        font-family: 'Fragment Mono', monospace;
        border-top: none;
        border-right: solid 3px #ffffff10;
        border-left: solid 3px #ffffff10;
        border-bottom: none;
        transition: all 0.3s ease-in-out;
    }

    button:hover {
        background-color: #ffffff10;
        transition: all 0.3s ease-in-out;
    }

    .shop2 {
        display: flex;
        flex: 25%;
        flex-direction: column;
    }

    .ballFeed {
        animation: ballfeed 1s linear infinite;
    }

    .ballClickFeed {
        animation: ballfeed 0.5s linear;
    }

    @keyframes ballfeed {
        0% {
            transform: scale(1);
            opacity: 100%;
        }
        50% {
            transform: scale(1.2);
            opacity: 50%;
        }
        100% {
            transform: scale(1.5);
            opacity: 0%;
        }
    }
</style>
