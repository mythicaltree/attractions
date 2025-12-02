<script lang="ts">
  import { base } from '$app/paths';
  import { onMount } from 'svelte';
  import * as attractions from '$lib';
  import { s, formatFileSize } from '$lib/utils';
  import { GridIcon, Edit2Icon, FeatherIcon } from 'svelte-feather-icons';
  import InfoTile from '$components/home/info-tile.svelte';

  const totalComponents = Object.keys(attractions).length - 1; // utils
  let bundleSizePromise = Promise.resolve({ size: Infinity, gzip: Infinity });

  onMount(() => {
    bundleSizePromise = fetch(
      `https://bundlephobia.com/api/size?package=attractions@${process.latest_version}`
    ).then(response => response.json());
  });
</script>

<div class="info-tiles">
  <a class="info-tiles-link" href="{base}/docs/options">
    <div>
      <InfoTile
        icon={GridIcon}
        title="Опции по продукту"
        subtitle="Роутер Bitcord RTR-4"
      />
    </div>
  </a>

  <a class="info-tiles-link" href="{base}/docs/architecture">
    <div>
      <InfoTile
        icon={Edit2Icon}
        title="Описание архитектуры"
        subtitle="Встраиваемое ПО БР-01 (RTR-2)"
      />
    </div>
  </a>

  <a class="info-tiles-link" href="{base}/docs/configure">
    <div>
      <InfoTile
        icon={FeatherIcon}
        title="Обновления прошивок"
        subtitle="Онлайн-конфигуратор опций роутера Bitcord RTR-4"
      />
    </div>
  </a>
</div>

<style lang="scss">
  .info-tiles-link {
    text-decoration: none;
    color: #000000;
  }

  .info-tiles {
    margin-top: 3em;

    @media only screen and (min-width: 640px) {
      margin-top: 6em;
    }

    @media only screen and (min-width: 1024px) {
      margin-top: 0;
    }
  }
</style>
