<script lang="ts">
  let sides = $state(6);
  let rolls = $state<number[]>([]);
  let rolling = $state(false);

  function roll() {
    rolling = true;
    const result = Math.floor(Math.random() * sides) + 1;
    rolls = [...rolls.slice(-49), result];
    setTimeout(() => rolling = false, 200);
  }

  function rollMany(n: number) {
    for (let i = 0; i < n; i++) {
      rolls = [...rolls, Math.floor(Math.random() * sides) + 1];
    }
    rolls = rolls.slice(-200);
  }

  function reset() { rolls = []; }

  const counts = $derived.by(() => {
    const c: Record<number, number> = {};
    for (let i = 1; i <= sides; i++) c[i] = 0;
    for (const r of rolls) c[r]++;
    return c;
  });

  const maxCount = $derived(Math.max(1, ...Object.values(counts)));
</script>

<div class="my-6">
  <div class="flex flex-wrap gap-2 mb-4">
    <button onclick={roll} class="px-4 py-2 bg-indigo-600 text-white rounded-lg text-sm hover:bg-indigo-700 transition-colors">🎲 Würfeln</button>
    <button onclick={() => rollMany(10)} class="px-3 py-2 bg-indigo-100 text-indigo-700 rounded-lg text-sm hover:bg-indigo-200">10× würfeln</button>
    <button onclick={() => rollMany(100)} class="px-3 py-2 bg-indigo-100 text-indigo-700 rounded-lg text-sm hover:bg-indigo-200">100× würfeln</button>
    <button onclick={reset} class="px-3 py-2 bg-slate-100 text-slate-700 rounded-lg text-sm hover:bg-slate-200">Reset</button>
    <span class="self-center text-sm text-slate-500">Würfe: {rolls.length}</span>
  </div>

  {#if rolls.length > 0}
    <div class="flex items-end gap-2 h-40 bg-white rounded-lg border border-slate-200 p-4">
      {#each Array(sides) as _, i}
        <div class="flex-1 flex flex-col items-center gap-1">
          <span class="text-xs text-slate-500">{counts[i+1]} ({(counts[i+1] / rolls.length * 100).toFixed(0)}%)</span>
          <div class="w-full bg-indigo-500 rounded-t transition-all duration-200" style="height: {(counts[i+1] / maxCount) * 100}%"></div>
          <span class="text-sm font-bold">{i + 1}</span>
        </div>
      {/each}
    </div>
    <p class="mt-2 text-sm text-slate-600 text-center">Theoretische Wahrscheinlichkeit: je {(100/sides).toFixed(1)}%</p>
  {/if}
</div>
