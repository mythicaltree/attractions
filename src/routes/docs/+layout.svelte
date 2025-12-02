<script lang="ts">
  import { page } from '$app/stores';
  import { Button } from '$lib';
  import { HomeIcon, ArrowUpIcon } from 'svelte-feather-icons';
  import Header from '$containers/docs/header.svelte';
  import MobileNavigation from '$containers/docs/mobile-navigation.svelte';
  import DesktopNavigation from '$containers/docs/desktop-navigation.svelte';
  import type { Place } from '$containers/docs/place';

  $: segments = $page.url.pathname.split('/');
  $: segment = segments[segments.length - 2]; // 2 because of trailing slash

  function scrollToTop() {
    window.scrollTo(0, 0);
    // TODO: manage focus
  }

  let scrollbarVisible: boolean;
  page.subscribe(function updateScrollbarVisibility() {
    if (typeof window === 'undefined') {
      return;
    }
    scrollbarVisible = window.innerHeight < document.body.scrollHeight;
  });

  const places: Place[] = [
    {
      title: HomeIcon,
      segment: '',
    },
    {
      title: 'Главная',
      segment: 'welcome',
    },

    {
      title: 'Роутер RTR-4',
      segment: 'about-rtr-4',
    },
    {
      title: 'Опции',
      segment: 'options',
    },

    {
      title: 'Архитектура',
      segment: 'architecture',
    },
    {
      title: 'Конфигуратор',
      segment: 'configure',
    },
    /* {
      title: 'Custom Elements',
      segment: 'custom-elements',
    }, */
  ];
</script>

<Header {segment} />
<MobileNavigation {places} segment={segment || ''} />

<main class="padded extra">
  <DesktopNavigation {places} segment={segment || ''} />
  <article>
    <slot />
    {#if scrollbarVisible}
      <div class="center">
        <Button filled on:click={scrollToTop}>
          <ArrowUpIcon size="24" class="mr" />
          scroll to top
        </Button>
      </div>
    {/if}
  </article>
</main>

<style lang="scss">
  @use '$css/attractions-theme' as vars;
  @use '$css/global';

  main {
    display: flex;
    margin: 1em 0 2em;

    article {
      padding: 1em 0;
      width: 100%;
    }

    :global .required {
      color: vars.$main;
      font-size: 0.9em;
    }

    @media only screen and (min-width: 1024px) {
      flex: 1;
      margin-top: 2em;
    }

    :global small {
      display: block;
      white-space: nowrap;
    }

    :global mark {
      background: none;
      color: vars.$main;

      code {
        color: vars.$main;
      }
    }
  }

  .center {
    display: flex;
    justify-content: center;
    margin-top: 2em;
  }
</style>
