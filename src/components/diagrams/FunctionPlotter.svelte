<script lang="ts">
  let { fn = (x: number) => x, fnLabel = 'f(x) = x', xMin = -5, xMax = 5, yMin = -5, yMax = 5, showSlider = false, sliderLabel = 'a', sliderMin = -3, sliderMax = 3, sliderStep = 0.1 } = $props();

  let param = $state(1);
  const width = 500;
  const height = 400;
  const pad = 40;

  function toSvgX(x: number) { return pad + ((x - xMin) / (xMax - xMin)) * (width - 2 * pad); }
  function toSvgY(y: number) { return height - pad - ((y - yMin) / (yMax - yMin)) * (height - 2 * pad); }

  const points = $derived.by(() => {
    const pts: string[] = [];
    const steps = 200;
    for (let i = 0; i <= steps; i++) {
      const x = xMin + (i / steps) * (xMax - xMin);
      const y = fn(x, param);
      if (isFinite(y) && y >= yMin - 2 && y <= yMax + 2) {
        pts.push(`${toSvgX(x)},${toSvgY(Math.max(yMin, Math.min(yMax, y)))}`);
      }
    }
    return pts.join(' ');
  });

  const xTicks = $derived(Array.from({ length: xMax - xMin + 1 }, (_, i) => xMin + i));
  const yTicks = $derived(Array.from({ length: yMax - yMin + 1 }, (_, i) => yMin + i));
</script>

<div class="my-6">
  {#if showSlider}
    <div class="flex items-center gap-3 mb-3">
      <label class="text-sm font-medium text-slate-700">{sliderLabel} = {param.toFixed(1)}</label>
      <input type="range" min={sliderMin} max={sliderMax} step={sliderStep} bind:value={param} class="flex-1" />
    </div>
  {/if}
  <svg {width} {height} viewBox="0 0 {width} {height}" class="max-w-full bg-white rounded-lg border border-slate-200">
    <!-- grid -->
    {#each xTicks as x}
      <line x1={toSvgX(x)} y1={pad} x2={toSvgX(x)} y2={height - pad} stroke="#f1f5f9" stroke-width="1" />
    {/each}
    {#each yTicks as y}
      <line x1={pad} y1={toSvgY(y)} x2={width - pad} y2={toSvgY(y)} stroke="#f1f5f9" stroke-width="1" />
    {/each}
    <!-- axes -->
    <line x1={pad} y1={toSvgY(0)} x2={width - pad} y2={toSvgY(0)} stroke="#64748b" stroke-width="1.5" />
    <line x1={toSvgX(0)} y1={pad} x2={toSvgX(0)} y2={height - pad} stroke="#64748b" stroke-width="1.5" />
    <!-- axis labels -->
    {#each xTicks as x}
      {#if x !== 0}
        <text x={toSvgX(x)} y={toSvgY(0) + 15} text-anchor="middle" font-size="10" fill="#94a3b8">{x}</text>
      {/if}
    {/each}
    {#each yTicks as y}
      {#if y !== 0}
        <text x={toSvgX(0) - 8} y={toSvgY(y) + 4} text-anchor="end" font-size="10" fill="#94a3b8">{y}</text>
      {/if}
    {/each}
    <!-- function curve -->
    <polyline points={points} fill="none" stroke="#6366f1" stroke-width="2.5" stroke-linecap="round" />
    <!-- label -->
    <text x={width - pad - 5} y={pad + 15} text-anchor="end" font-size="13" fill="#6366f1" font-weight="bold">{fnLabel}{showSlider ? ` (${sliderLabel}=${param.toFixed(1)})` : ''}</text>
  </svg>
</div>
