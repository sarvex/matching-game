<script lang="ts">
  import { emoji } from './emoji';

  type State = 'start' | 'playing' | 'paused' | 'won' | 'lost';

  let state: State = 'start';
  let size = 20;
  let grid = createGrid();
  let maxMatches = grid.length / 2;
  let selected: number[] = [];
  let matches: string[] = [];

  function createGrid() {
    let cards = new Set<string>();
    let maxSize: number = size / 2;

    while (cards.size < maxSize) {
      const randomIndex = Math.floor(Math.random() * emoji.length);
      cards.add(emoji[randomIndex]);
    }

    return shuffle([...cards, ...cards]);
  }

  function shuffle<Items>(array: Items[]) {
    return array.sort(() => Math.random() - 0.5);
  }
</script>

{#if state === 'start'}
  <h1>Matching Game</h1>
  <button on:click={() => (state = 'playing')}>Play</button>
{/if}

{#if state === 'playing'}
  <div class="cards">
    {#each grid as card, cardIndex}
      <button class="card">
        <div>{card}</div>
      </button>
    {/each}
  </div>
{/if}

{#if state === 'lost'}
  <h1>You Lost 💩</h1>
  <button on:click={() => (state = 'playing')}>Play Again</button>
{/if}

{#if state === 'won'}
  <h1>You WIN 🎉</h1>
  <button on:click={() => (state = 'playing')}>Play Again</button>
{/if}

<style>
  .cards {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 0.4rem;
  }

  .card {
    height: 140px;
    width: 140px;
    font-size: 4rem;
    background-color: var(--bg-2);

    &.selected {
      border: 4px solid var(--border);
    }
  }
</style>
