<script lang="ts">
  import { Wallet, ethers } from "ethers";
  import { entropyToMnemonic, randomBytes } from "ethers/lib/utils";

  let seedphrase = [];
  let easyToRecover = false;

  function createEntropyArray(size: number, entropy: number): Uint8Array {
    if (entropy > 32) {
      console.log("lol good luck keyboard cowboy");
    }
    const maxValue = Math.pow(2, entropy) - 1;
    return new Uint8Array(size).map(() =>
      Math.floor(Math.random() * (maxValue + 1))
    );
  }

  function createRandom() {
    let entropy: Uint8Array;
    if (easyToRecover) {
      entropy = createEntropyArray(16, 2);
    } else {
      entropy = randomBytes(16);
    }
    console.log(entropy);
    const mnemonic = entropyToMnemonic(entropy);
    return Wallet.fromMnemonic(mnemonic);
  }

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
    const w = createRandom();
    seedphrase = [];
    play(w.mnemonic.phrase.split(" "));
  }
</script>

<main>
  <section>
    <h1>Enthusiastic Seedphrase</h1>
    <label>
      <input type="checkbox" bind:checked={easyToRecover} />
      Easy to recover
    </label>
    <br />
    <br />
    <button on:click={onClick}>Generate my seedphrase</button>
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
