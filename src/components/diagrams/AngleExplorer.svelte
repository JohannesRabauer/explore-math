<script lang="ts">
  let angleDeg = $state(45);
  const cx = 200, cy = 200, r = 140;

  const endX = $derived(cx + r * Math.cos(-angleDeg * Math.PI / 180));
  const endY = $derived(cy + r * Math.sin(-angleDeg * Math.PI / 180));

  const arcPath = $derived.by(() => {
    const arcR = 50;
    const ex = cx + arcR * Math.cos(-angleDeg * Math.PI / 180);
    const ey = cy + arcR * Math.sin(-angleDeg * Math.PI / 180);
    const large = angleDeg > 180 ? 1 : 0;
    return `M ${cx + arcR} ${cy} A ${arcR} ${arcR} 0 ${large} 0 ${ex} ${ey}`;
  });

  function classify(deg: number): string {
    if (deg < 90) return 'spitzer Winkel (acute)';
    if (deg === 90) return 'rechter Winkel (right)';
    if (deg < 180) return 'stumpfer Winkel (obtuse)';
    if (deg === 180) return 'gestreckter Winkel (straight)';
    return 'überstumpfer Winkel (reflex)';
  }
</script>

<div class="my-6">
  <div class="flex items-center gap-3 mb-3">
    <label class="text-sm font-medium">Winkel: <span class="text-indigo-600 font-bold">{angleDeg}°</span></label>
    <input type="range" min="0" max="360" bind:value={angleDeg} class="flex-1" />
  </div>
  <svg width="400" height="250" viewBox="50 50 300 200" class="max-w-full bg-white rounded-lg border border-slate-200">
    <!-- base ray -->
    <line x1={cx} y1={cy} x2={cx + r} y2={cy} stroke="#94a3b8" stroke-width="2" />
    <!-- rotating ray -->
    <line x1={cx} y1={cy} x2={endX} y2={endY} stroke="#6366f1" stroke-width="2.5" />
    <!-- arc -->
    <path d={arcPath} fill="none" stroke="#f59e0b" stroke-width="2" />
    <!-- center dot -->
    <circle {cx} {cy} r="4" fill="#6366f1" />
    <!-- angle label -->
    <text x={cx + 60} y={cy - 10} font-size="14" fill="#f59e0b" font-weight="bold">{angleDeg}°</text>
  </svg>
  <p class="mt-2 text-sm text-slate-600 text-center">{classify(angleDeg)}</p>
</div>
