<script lang="ts">
  let { mode = 'triangle' as 'triangle' | 'rectangle' | 'circle' | 'angle' } = $props();

  let pointA = $state({ x: 100, y: 280 });
  let pointB = $state({ x: 400, y: 280 });
  let pointC = $state({ x: 250, y: 80 });
  let dragging = $state<'A' | 'B' | 'C' | null>(null);
  let radius = $state(100);

  const width = 500;
  const height = 340;

  function dist(a: {x:number,y:number}, b: {x:number,y:number}) {
    return Math.sqrt((a.x - b.x) ** 2 + (a.y - b.y) ** 2);
  }

  function angle(a: {x:number,y:number}, vertex: {x:number,y:number}, b: {x:number,y:number}) {
    const v1 = { x: a.x - vertex.x, y: a.y - vertex.y };
    const v2 = { x: b.x - vertex.x, y: b.y - vertex.y };
    const dot = v1.x * v2.x + v1.y * v2.y;
    const cross = v1.x * v2.y - v1.y * v2.x;
    return Math.atan2(Math.abs(cross), dot) * (180 / Math.PI);
  }

  const sideAB = $derived(dist(pointA, pointB).toFixed(0));
  const sideBC = $derived(dist(pointB, pointC).toFixed(0));
  const sideCA = $derived(dist(pointC, pointA).toFixed(0));
  const angleA = $derived(angle(pointB, pointA, pointC).toFixed(1));
  const angleB = $derived(angle(pointA, pointB, pointC).toFixed(1));
  const angleC = $derived(angle(pointA, pointC, pointB).toFixed(1));
  const area = $derived((Math.abs((pointB.x - pointA.x) * (pointC.y - pointA.y) - (pointC.x - pointA.x) * (pointB.y - pointA.y)) / 2).toFixed(0));

  function onPointerDown(p: 'A' | 'B' | 'C') { dragging = p; }
  function onPointerMove(e: PointerEvent) {
    if (!dragging) return;
    const svg = (e.currentTarget as SVGElement).getBoundingClientRect();
    const x = Math.max(10, Math.min(width - 10, e.clientX - svg.left));
    const y = Math.max(10, Math.min(height - 10, e.clientY - svg.top));
    if (dragging === 'A') pointA = { x, y };
    else if (dragging === 'B') pointB = { x, y };
    else pointC = { x, y };
  }
  function onPointerUp() { dragging = null; }
</script>

<div class="my-6">
  <p class="text-sm text-slate-600 mb-2 font-medium">Ziehe die Punkte, um die Figur zu verändern!</p>
  <svg {width} {height} viewBox="0 0 {width} {height}" class="max-w-full bg-white rounded-lg border border-slate-200 touch-none"
    onpointermove={onPointerMove} onpointerup={onPointerUp} onpointerleave={onPointerUp}>
    {#if mode === 'triangle' || mode === 'angle'}
      <polygon points="{pointA.x},{pointA.y} {pointB.x},{pointB.y} {pointC.x},{pointC.y}"
        fill="#e0e7ff" stroke="#6366f1" stroke-width="2" />
      <!-- side labels -->
      <text x={(pointA.x + pointB.x) / 2} y={(pointA.y + pointB.y) / 2 + 18} text-anchor="middle" font-size="11" fill="#4f46e5">{sideAB}px</text>
      <text x={(pointB.x + pointC.x) / 2 + 12} y={(pointB.y + pointC.y) / 2} text-anchor="start" font-size="11" fill="#4f46e5">{sideBC}px</text>
      <text x={(pointC.x + pointA.x) / 2 - 12} y={(pointC.y + pointA.y) / 2} text-anchor="end" font-size="11" fill="#4f46e5">{sideCA}px</text>
    {:else if mode === 'rectangle'}
      <rect x={Math.min(pointA.x, pointB.x)} y={Math.min(pointA.y, pointC.y)}
        width={Math.abs(pointB.x - pointA.x)} height={Math.abs(pointC.y - pointA.y)}
        fill="#e0e7ff" stroke="#6366f1" stroke-width="2" />
    {:else if mode === 'circle'}
      <circle cx={pointA.x} cy={pointA.y} r={radius} fill="#e0e7ff" stroke="#6366f1" stroke-width="2" />
    {/if}
    <!-- draggable points -->
    {#each [['A', pointA], ['B', pointB], ['C', pointC]] as [lbl, pt]}
      <circle cx={pt.x} cy={pt.y} r="8" fill="#6366f1" stroke="white" stroke-width="2"
        class="cursor-grab" onpointerdown={() => onPointerDown(lbl)} />
      <text x={pt.x} y={pt.y - 14} text-anchor="middle" font-size="12" font-weight="bold" fill="#4f46e5">{lbl}</text>
    {/each}
  </svg>
  <div class="mt-3 text-sm text-slate-700 grid grid-cols-2 gap-2">
    <div>∠A = {angleA}°</div>
    <div>∠B = {angleB}°</div>
    <div>∠C = {angleC}°</div>
    <div>Fläche = {area} px²</div>
  </div>
</div>
