<script lang="ts">
  let { numerator = 3, denominator = 4, showSliders = true } = $props();
  let num = $state(numerator);
  let den = $state(denominator);

  const width = 300;
  const height = 60;
  const barPad = 4;
</script>

<div class="my-6">
  {#if showSliders}
    <div class="flex gap-6 mb-3 text-sm">
      <label class="flex items-center gap-2">
        Zähler: <input type="range" min="0" max={den} bind:value={num} class="w-24" /> <span class="font-bold text-indigo-600">{num}</span>
      </label>
      <label class="flex items-center gap-2">
        Nenner: <input type="range" min="1" max="12" bind:value={den} class="w-24" /> <span class="font-bold text-indigo-600">{den}</span>
      </label>
    </div>
  {/if}
  <svg {width} {height} viewBox="0 0 {width} {height}" class="max-w-full">
    <rect x="0" y="0" {width} {height} rx="8" fill="#f1f5f9" stroke="#e2e8f0" stroke-width="1" />
    {#each Array(den) as _, i}
      <rect
        x={barPad + i * ((width - 2*barPad) / den) + 1}
        y={barPad}
        width={(width - 2*barPad) / den - 2}
        height={height - 2*barPad}
        rx="4"
        fill={i < num ? '#6366f1' : '#e2e8f0'}
      />
    {/each}
  </svg>
  <p class="mt-2 text-lg font-bold text-center text-indigo-600">
    <span class="border-b-2 border-indigo-600 px-1">{num}</span> / {den} = {(num / den).toFixed(3)}
  </p>
</div>
