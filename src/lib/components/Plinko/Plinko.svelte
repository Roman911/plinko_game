<script lang="ts">
  import { plinkoEngine, plinkoEngine2 } from '$lib/stores/game';
  import CircleNotch from 'phosphor-svelte/lib/CircleNotch';
  import type { Action } from 'svelte/action';
  import BinsRow from './BinsRow.svelte';
  import BinsRow2 from './BinsRow2.svelte';
  import LastWins from './LastWins.svelte';
  import PlinkoEngine from './PlinkoEngine';
  import PlinkoEngine2 from './PlinkoEngine2';

  const { WIDTH, HEIGHT } = PlinkoEngine;
  const { WIDTH2, HEIGHT2 } = PlinkoEngine2;

  const initPlinko: Action<HTMLCanvasElement> = (node) => {
    $plinkoEngine = new PlinkoEngine(node);
    $plinkoEngine.start();

    return {
      destroy: () => {
        $plinkoEngine?.stop();
      },
    };
  };

  const initPlinko2: Action<HTMLCanvasElement> = (node) => {
    $plinkoEngine2 = new PlinkoEngine2(node);
    $plinkoEngine2.start();

    return {
      destroy: () => {
        $plinkoEngine2?.stop();
      },
    };
  };
</script>

<div class="relative bg-gray-900">
  <div class="mx-auto flex h-full flex-col px-4 pb-4" style:max-width={`${WIDTH}px`}>
    <div class="relative w-full" style:aspect-ratio={`${WIDTH} / ${HEIGHT}`}>
      {#if $plinkoEngine === null}
        <div class="absolute left-1/2 top-1/2 -translate-x-1/2 -translate-y-1/2">
          <CircleNotch class="size-20 animate-spin text-slate-600" weight="bold" />
        </div>
      {/if}

      <canvas use:initPlinko width={WIDTH} height={HEIGHT} class="absolute inset-0 h-full w-full" />
    </div>
    <BinsRow />
  </div>
  <div class="mx-auto flex h-full flex-col px-4 pb-4" style:max-width={`${WIDTH2}px`}>
    <div class="relative w-full" style:aspect-ratio={`${WIDTH2} / ${HEIGHT2}`}>
      {#if $plinkoEngine2 === null}
        <div class="absolute left-1/2 top-1/2 -translate-x-1/2 -translate-y-1/2">
          <CircleNotch class="size-20 animate-spin text-slate-600" weight="bold" />
        </div>
      {/if}
      <canvas use:initPlinko2 width={WIDTH2} height={HEIGHT2} class="absolute inset-0 h-full w-full" />
    </div>
    <BinsRow2 />
  </div>
  <div class="absolute right-[5%] top-1/2 -translate-y-1/2">
    <LastWins />
  </div>
</div>
