<script lang="ts">
    import { cards } from '../../stores.js';
    let cardsData = [];
    let cardIndex = -1;
    let card = null;
    let isRevealed = false;

    cards.subscribe(value => {
        cardsData = value;
    });

    function reveal() {
        isRevealed = !isRevealed;
    }

    function previousCard() {
        if (cardIndex > 0) {
            cardIndex--;
            card = $cards[cardIndex];
            isRevealed = false;
        }
    }

    function nextCard() {
        if (cardIndex < cardsData.length) {
            cardIndex++;
        }
        if (cardIndex < cardsData.length) {
            card = $cards[cardIndex];
            isRevealed = false;
        }
    }
    function reset() {
        cardIndex = 0;
        card = $cards[cardIndex];
        isRevealed = false;
    }
</script>

<h2>Study cards</h2>

{#if cardIndex < 0}
    <button on:click={nextCard}>Start studying</button>
{:else if cardIndex >= cardsData.length}
    <h1>Finished studying!</h1>
{:else}
<div class="card" on:click={reveal} on:keypress={reveal}>
    <div class="card-front">
        <h1>{card.front}</h1>
    </div>
    {#if isRevealed}
        <div class="card-back">
            <hr>
            <h1>{card.back}</h1>
        </div>
    {/if}
</div>
{/if}
<button on:click={previousCard}>Previous card</button>
<button on:click={nextCard}>Next card</button>
<button on:click={reset}>Start over</button>

<style>
    button {
        background: rgba(0, 0, 0, 0.5);
        border: solid white 2px;
        color: white;
        width: 150px;
        height: 50px;
    }
    .card {
        background: rgba(0, 0, 0, 0.5);
        border: solid white 2px;
        border-radius: 6px;
        width: 450px;
        height: 150px;
    }
</style>
