<script lang="ts">
  import type { Snippet } from "svelte";
  import iconHome from "@ktibow/iconset-material-symbols/home-outline";
  import iconHomeS from "@ktibow/iconset-material-symbols/home";
  import iconPalette from "@ktibow/iconset-material-symbols/palette-outline";
  import iconPaletteS from "@ktibow/iconset-material-symbols/palette";
  import iconBook from "@ktibow/iconset-material-symbols/book-2-outline";
  import iconBookS from "@ktibow/iconset-material-symbols/book-2";
  import iconAnimation from "@ktibow/iconset-material-symbols/animation";
  import iconAnimationS from "@ktibow/iconset-material-symbols/animation";
  import { base } from "$app/paths";
  import { page } from "$app/state";
  import NavCMLX from "$lib/nav/NavCMLX.svelte";
  import NavCMLXItem from "$lib/nav/NavCMLXItem.svelte";
  import { styling } from "./themeStore";
  import Button from "$lib/buttons/Button.svelte";
  import "../app.css";

  let { children }: { children: Snippet } = $props();

  const paths = [
    {
      path: base || "/",
      icon: iconHome,
      iconS: iconHomeS,
      label: "Home",
    },
    {
      path: base + "/theme",
      icon: iconPalette,
      iconS: iconPaletteS,
      label: "Theme",
    },
  ];
  const normalizePath = (path: string) => {
    const u = new URL(path, page.url.href);
    path = u.pathname;
    if (path.endsWith("/")) path = path.slice(0, -1);
    return path || "/";
  };
</script>
<div style="display: none;">
  <Button variant="tonal" iconType="none" onclick={() => {}}>
    <svg width="16" height="16" viewBox="0 0 59 59" fill="none" xmlns="http://www.w3.org/2000/svg">
        <g clip-path="url(#clip0_845_2368)">
        <path d="M45.0613 10.244C44.5229 9.8236 43.8223 9.6761 43.1659 9.83835L19.5659 15.7383C18.5776 15.9817 17.8918 16.8667 17.8918 17.8845V36.9415C14.16 35.3116 9.81613 37.0152 8.18626 40.7543C6.55639 44.4861 8.26001 48.83 11.9991 50.4599C15.7309 52.0897 20.0748 50.3861 21.7046 46.647C22.1103 45.7177 22.3168 44.7147 22.3168 43.697V19.6102L41.4918 14.8165V31.0415C37.76 29.4116 33.4161 31.1152 31.7863 34.8543C30.1564 38.5861 31.86 42.93 35.5991 44.5598C39.3309 46.1897 43.6748 44.4861 45.3046 40.747C45.7103 39.8177 45.9168 38.8147 45.9168 37.797V11.9845C45.9168 11.306 45.5996 10.657 45.0686 10.244H45.0613ZM14.9344 46.647C13.3045 46.647 11.9844 45.3268 11.9844 43.697C11.9844 42.0671 13.3045 40.747 14.9344 40.747C16.5643 40.747 17.8844 42.0671 17.8844 43.697C17.8844 45.3268 16.5643 46.647 14.9344 46.647ZM38.5344 40.747C36.9045 40.747 35.5844 39.4268 35.5844 37.797C35.5844 36.1671 36.9045 34.847 38.5344 34.847C40.1643 34.847 41.4844 36.1671 41.4844 37.797C41.4844 39.4268 40.1643 40.747 38.5344 40.747Z" fill="currentColor"/>
        </g>
        <defs>
        <clipPath id="clip0_845_2368">
        <rect width="59" height="59" fill="white"/>
        </clipPath>
        </defs>
    </svg>
  </Button>
</div>
{@html `<style>${$styling}</style>`}
<div class="container">
  <div class="sidebar">
    <NavCMLX variant="auto">
      {#each paths as { path, icon, iconS, label }}
        {@const selected = normalizePath(path) === normalizePath(page.url.pathname)}
        <NavCMLXItem
          variant="auto"
          href={normalizePath(path)}
          {selected}
          icon={selected ? iconS : icon}
          text={label}
        />
      {/each}
      {#if page.url.pathname.startsWith(base + "/docs")}
        {#each [["Quick start", `${base}/docs/quick-start`], ["Walkthrough", `${base}/docs/detailed-walkthrough`], ["llms.txt", `${base}/llms.txt`]] as [text, href]}
          <NavCMLXItem
            variant="auto"
            {href}
            selected={page.url.pathname == href}
            icon={page.url.pathname == href ? iconBookS : iconBook}
            {text}
          />
        {/each}
      {:else}
        <NavCMLXItem
          variant="auto"
          href={normalizePath(base + "/docs/quick-start")}
          selected={page.url.pathname.startsWith(base + "/docs")}
          icon={page.url.pathname.startsWith(base + "/docs") ? iconBookS : iconBook}
          text="Docs"
        />
      {/if}
      <NavCMLXItem
        variant="auto"
        href={normalizePath(base + "/transitions")}
        selected={page.url.pathname.startsWith(base + "/transitions")}
        icon={page.url.pathname.startsWith(base + "/transitions") ? iconAnimationS : iconAnimation}
        text="Animations"
      />
    </NavCMLX>
  </div>
  <div class="content">
    {@render children()}
  </div>
</div>

<style>
  .container {
    display: grid;
    min-height: 100dvh;
  }
  .sidebar {
    display: flex;
    position: sticky;
  }
  .content {
    display: flex;
    flex-direction: column;
    padding: 1rem;
  }
  @media (width < 52.5rem) {
    .container {
      grid-template-rows: 1fr auto;
      --m3-util-bottom-offset: 5rem;
    }
    .sidebar {
      flex-direction: column;
      bottom: 0;
      width: 100%;
      z-index: 3;
      grid-row: 2;
    }
  }
  @media (width >= 52.5rem) {
    .container {
      grid-template-columns: auto 1fr;
    }
    .sidebar {
      grid-column: 1;
      top: 0;
      left: 0;
      flex-direction: column;
      height: 100dvh;
      @media (width < 100rem) {
        width: 6rem;
        > :global(nav) {
          position: absolute;
          top: 50%;
          translate: 0 -50%;
        }
      }
    }
    .content {
      padding: 1.5rem;
      grid-column: 2;
    }
  }
</style>
