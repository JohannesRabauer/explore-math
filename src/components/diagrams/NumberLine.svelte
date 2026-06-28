<script lang="ts">
  let { min = 0, max = 20, highlights = [] as number[], label = 'Ziehe den Punkt!' } = $props();
  let dragging = $state<number | null>(null);
  let positions = $state<number[]>(highlights.length > 0 ? [...highlights] : [5]);

  const width = 600;
  const height = 80;
  const padding = 40;

  function xToVal(x: number): number {
    return Math.round(min + ((x - padding) / (width - 2 * padding)) * (max - min));
  }

  function valToX(v: number): number {
    return padding + ((v - min) / (max - min)) * (width - 2 * padding);
  }

  function onPointerDown(i: number) {
    dragging = i;
  }

  function onPointerMove(e: PointerEvent) {
    if (dragging === null) return;
    const svg = (e.currentTarget as SVGElement);
    const rect = svg.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const val = Math.max(min, Math.min(max, xToVal(x)));
    positions[dragging] = val;
  }

  function onPointerUp() {
    dragging = null;
  }

  const ticks = $derived(Array.from({ length: max - min + 1 }, (_, i) => min + i));
</script>

<div class="my-6">
  <p class="text-sm text-slate-600 mb-2 font-medium">{label}</p>
  <svg {width} {height} class="max-w-full" viewBox="0 0 {width} {height}"
    onpointermove={onPointerMove} onpointerup={onPointerUp} onpointerleave={onPointerUp}>
    <!-- axis line -->
    <line x1={padding} y1={height/2} x2={width - padding} y2={height/2} stroke="#94a3b8" stroke-width="2" />
    <!-- ticks -->
    {#each ticks as t}
      {#if (max - min) <= 20 || t % 5 === 0}
        <line x1={valToX(t)} y1={height/2 - 6} x2={valToX(t)} y2={height/2 + 6} stroke="#94a3b8" stroke-width="1" />
        <text x={valToX(t)} y={height/2 + 20} text-anchor="middle" class="text-xs fill-slate-500" font-size="11">{t}</text>
      {/if}
    {/each}
    <!-- draggable points -->
    {#each positions as pos, i}
      <circle cx={valToX(pos)} cy={height/2} r="10"
        class="cursor-grab active:cursor-grabbing"
        fill={i === 0 ? '#6366f1' : '#f59e0b'}
        stroke="white" stroke-width="2"
        onpointerdown={() => onPointerDown(i)} />
      <text x={valToX(pos)} y={height/2 - 16} text-anchor="middle" font-size="13" font-weight="bold"
        fill={i === 0 ? '#6366f1' : '#f59e0b'}>{pos}</text>
    {/each}
  </svg>
</div>
