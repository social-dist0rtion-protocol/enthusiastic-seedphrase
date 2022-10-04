<script lang="ts">
  import { ethers } from "ethers";

  let seedphrase = [];

  function play(words: string[]) {
    const w = words.shift();
    if (w) {
      const a = new Audio(`/words/${w}.mp3`);
      a.play();
      seedphrase.push(w);
      // This thing doesn't make much sense Svelte
      seedphrase = seedphrase;
      a.addEventListener("ended", () => play(words));
    }
  }

  function onClick() {
    const w = ethers.Wallet.createRandom();
    seedphrase = [];
    play(w.mnemonic.phrase.split(" "));
  }
</script>

<main>
  <section>
    <h1>Enthusiastic Seedphrase</h1>
    <p>TikTok approved</p>
    <button on:click={onClick}>Tell me a seedphrase</button>
    {#if seedphrase.length}
      <ol>
        {#each seedphrase as word}
          <li>
            {word}
          </li>
        {/each}
      </ol>
    {/if}
  </section>
</main>

<style>
</style>
