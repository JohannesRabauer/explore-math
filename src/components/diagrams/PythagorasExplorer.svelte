<script lang="ts">
  let a = $state(3);
  let b = $state(4);
  const c = $derived(Math.sqrt(a * a + b * b));
  const scale = 40;
</script>

<div class="my-6">
  <div class="flex gap-6 mb-3 text-sm">
    <label class="flex items-center gap-2">a: <input type="range" min="1" max="8" step="0.5" bind:value={a} class="w-28" /> <span class="font-bold text-indigo-600">{a}</span></label>
    <label class="flex items-center gap-2">b: <input type="range" min="1" max="8" step="0.5" bind:value={b} class="w-28" /> <span class="font-bold text-amber-600">{b}</span></label>
  </div>
  <svg width="480" height="360" viewBox="0 0 480 360" class="max-w-full bg-white rounded-lg border border-slate-200">
    <!-- triangle -->
    <polygon points="{50},{300} {50 + a*scale},{300} {50},{300 - b*scale}" fill="#e0e7ff" stroke="#6366f1" stroke-width="2" />
    <!-- right angle marker -->
    <rect x="50" y={300 - 15} width="15" height="15" fill="none" stroke="#64748b" stroke-width="1" />
    <!-- squares on sides -->
    <!-- a² -->
    <rect x="50" y="300" width={a*scale} height={a*scale * 0.3} fill="#6366f180" stroke="#6366f1" stroke-width="1" rx="2" />
    <text x={50 + a*scale/2} y={300 + a*scale*0.15 + 5} text-anchor="middle" font-size="13" fill="#4f46e5" font-weight="bold">a² = {(a*a).toFixed(1)}</text>
    <!-- b² -->
    <rect x={50 - b*scale*0.3} y={300 - b*scale} width={b*scale*0.3} height={b*scale} fill="#f59e0b80" stroke="#f59e0b" stroke-width="1" rx="2" />
    <text x={50 - b*scale*0.15} y={300 - b*scale/2 + 5} text-anchor="middle" font-size="13" fill="#b45309" font-weight="bold">b² = {(b*b).toFixed(1)}</text>
    <!-- labels on triangle sides -->
    <text x={50 + a*scale/2} y="295" text-anchor="middle" font-size="14" fill="#6366f1" font-weight="bold">a = {a}</text>
    <text x="35" y={300 - b*scale/2} text-anchor="middle" font-size="14" fill="#f59e0b" font-weight="bold">b = {b}</text>
    <text x={50 + a*scale/2 + 15} y={300 - b*scale/2 - 5} font-size="14" fill="#059669" font-weight="bold">c = {c.toFixed(2)}</text>
  </svg>
  <div class="mt-4 text-center">
    <p class="text-lg font-bold">a² + b² = c²</p>
    <p class="text-indigo-600 font-mono">{(a*a).toFixed(1)} + {(b*b).toFixed(1)} = {(a*a + b*b).toFixed(1)} = {c.toFixed(2)}²</p>
  </div>
</div>
