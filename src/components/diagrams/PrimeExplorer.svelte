<script lang="ts">
  let maxNum = $state(50);
  let selectedNum = $state(12);

  const primes = $derived.by(() => {
    const sieve = Array(maxNum + 1).fill(true);
    sieve[0] = sieve[1] = false;
    for (let i = 2; i * i <= maxNum; i++) {
      if (sieve[i]) for (let j = i * i; j <= maxNum; j += i) sieve[j] = false;
    }
    return sieve;
  });

  const factorization = $derived.by(() => {
    if (selectedNum < 2) return [];
    const factors: number[] = [];
    let n = selectedNum;
    for (let d = 2; d * d <= n; d++) {
      while (n % d === 0) { factors.push(d); n /= d; }
    }
    if (n > 1) factors.push(n);
    return factors;
  });
</script>

<div class="my-6">
  <div class="flex items-center gap-3 mb-4">
    <label class="text-sm font-medium">Zahl auswählen:</label>
    <input type="range" min={2} max={maxNum} bind:value={selectedNum} class="flex-1" />
    <span class="text-xl font-bold text-indigo-600 w-10 text-right">{selectedNum}</span>
  </div>

  <div class="bg-white rounded-lg border border-slate-200 p-4 mb-4">
    <p class="text-sm text-slate-600 mb-2 font-medium">Primzahlen bis {maxNum} (gelb markiert):</p>
    <div class="grid grid-cols-10 gap-1">
      {#each Array(maxNum) as _, i}
        <div class="w-8 h-8 flex items-center justify-center text-xs rounded {primes[i+1] ? 'bg-amber-200 text-amber-800 font-bold' : 'bg-slate-100 text-slate-500'} {i+1 === selectedNum ? 'ring-2 ring-indigo-500' : ''}">
          {i + 1}
        </div>
      {/each}
    </div>
  </div>

  <div class="bg-indigo-50 rounded-lg p-4">
    <p class="font-semibold text-indigo-900">
      {selectedNum} = {primes[selectedNum] ? '⭐ Primzahl! (nur durch 1 und sich selbst teilbar)' : factorization.join(' × ')}
    </p>
    {#if !primes[selectedNum] && factorization.length > 0}
      <p class="text-sm text-indigo-700 mt-1">Primfaktorzerlegung: {selectedNum} = {factorization.join(' × ')}</p>
    {/if}
  </div>
</div>
