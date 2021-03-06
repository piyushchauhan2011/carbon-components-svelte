<script context="module">
  const fetched = new Set();
</script>

<script>
  import { isActive, url, layout, beforeUrlChange } from "@sveltech/routify";
  import {
    Header,
    HeaderUtilities,
    HeaderAction,
    HeaderActionLink,
    HeaderPanelLinks,
    HeaderPanelLink,
    HeaderPanelDivider,
    SkipToContent,
    SideNav,
    SideNavItems,
    SideNavMenu,
    SideNavMenuItem,
  } from "carbon-components-svelte";
  import LogoGithub20 from "carbon-icons-svelte/lib/LogoGithub20";
  import Theme from "../components/Theme.svelte";
  import Footer from "../components/Footer.svelte";

  let isOpen = false;
  let isSideNavOpen = true;
  let innerWidth = 2048;

  $: isMobile = innerWidth < 1056;
  $: components = $layout.children.filter(
    (child) => child.title === "components"
  )[0];

  $beforeUrlChange(() => {
    if (isMobile) isSideNavOpen = false;
    return true;
  });
</script>

<style global>
  .bx--col > h1 {
    font-size: var(--cds-productive-heading-07-font-size);
    font-weight: var(--cds-productive-heading-07-font-weight);
    letter-spacing: var(--cds-productive-heading-07-letter-spacing);
    line-height: var(--cds-productive-heading-07-line-height);
    margin-bottom: var(--cds-layout-01);
  }

  .big-paragraph {
    font-size: 1.5rem;
    font-weight: var(--cds-productive-heading-03-font-weight);
    letter-spacing: var(--cds-productive-heading-03-letter-spacing);
    line-height: var(--cds-productive-heading-03-line-height);
    margin-top: var(--cds-layout-03);
    margin-bottom: var(--cds-layout-05);
  }

  .bx--col > .big-paragraph > code {
    font-size: var(--cds-productive-heading-03-font-size);
    font-weight: var(--cds-productive-heading-03-font-weight);
    letter-spacing: var(--cds-productive-heading-03-letter-spacing);
    line-height: var(--cds-productive-heading-03-line-height);
  }

  .big-link {
    font-size: 1.5rem;
    font-weight: var(--cds-productive-heading-03-font-weight);
    letter-spacing: var(--cds-productive-heading-03-letter-spacing);
    line-height: var(--cds-productive-heading-03-line-height);
  }

  .bx--col > p {
    max-width: 44rem;
  }

  .bx--col > p > code {
    font-size: var(--cds-code-02-font-size);
    font-weight: var(--cds-code-02-font-weight);
    line-height: var(--cds-code-02-line-height);
    letter-spacing: var(--cds-code-02-letter-spacing);
    background-color: var(--cds-ui-03);
    border-radius: 0.25rem;
    padding: 0 var(--cds-spacing-02);
  }

  .bx--col > h2 {
    font-size: var(--cds-productive-heading-06-font-size);
    font-weight: var(--cds-productive-heading-06-font-weight);
    letter-spacing: var(--cds-productive-heading-06-letter-spacing);
    line-height: var(--cds-productive-heading-06-line-height);
    padding-top: var(--cds-layout-03);
    margin-bottom: var(--cds-layout-01);
  }

  .bx--col > h3 {
    font-size: var(--cds-productive-heading-04-font-size);
    font-weight: var(--cds-productive-heading-04-font-weight);
    letter-spacing: var(--cds-productive-heading-04-letter-spacing);
    line-height: var(--cds-productive-heading-04-line-height);
    padding-top: var(--cds-layout-03);
    margin-bottom: var(--cds-layout-02);
  }

  .bx--col > h4 {
    font-size: var(--cds-productive-heading-02-font-size);
    font-weight: var(--cds-productive-heading-02-font-weight);
    letter-spacing: var(--cds-productive-heading-02-letter-spacing);
    line-height: var(--cds-productive-heading-02-line-height);
    padding-top: var(--cds-layout-04);
    margin-bottom: var(--cds-layout-01);
  }

  .bx--col > p {
    margin-bottom: var(--cds-layout-02);
  }

  main.bx--content {
    background: none;
  }

  @media (max-width: 1056px) {
    .bx--side-nav ~ .bx--content {
      margin-left: 0;
      padding-left: 1rem;
      padding-right: 1rem;
    }

    .bx--side-nav--expanded ~ .bx--content {
      white-space: nowrap;
      min-width: 28rem;
    }
  }

  .platform-name {
    display: flex;
    align-items: baseline;
  }

  .platform-name code {
    margin-left: var(--cds-spacing-02);
    font-size: var(--cds-code-01-font-size);
    font-weight: var(--cds-code-01-font-weight);
    letter-spacing: var(--cds-code-01-letter-spacing);
    line-height: var(--cds-code-01-line-height);
    color: #c6c6c6;
  }

  @media (max-width: 580px) {
    .platform-name code {
      display: none;
    }
  }

  @media (min-width: 1057px) {
    .bx--side-nav__navigation {
      z-index: 1;
    }
  }

  .bx--side-nav__submenu[aria-expanded="true"] + .bx--side-nav__menu {
    max-height: 120rem;
  }
</style>

<svelte:body
  on:mouseover="{async (e) => {
    if (process.env.NODE_ENV === 'development') return;
    if (!e.target.href || !e.target
        .getAttribute('class')
        .includes('bx--side-nav__link')) return;
    if (fetched.has(e.target.href)) return;
    fetched.add(e.target.href);
    await fetch(e.target.href);
  }}" />

<svelte:window bind:innerWidth />

<Theme persist>
  <Header
    aria-label="Navigation"
    href="{$url('/')}"
    expandedByDefault="{true}"
    bind:isSideNavOpen
  >
    <div slot="skip-to-content">
      <SkipToContent />
    </div>

    <span slot="platform" class="platform-name">
      Carbon Components Svelte <code>v{process.env.VERSION || ''}</code>
    </span>

    <HeaderUtilities>
      <HeaderActionLink
        icon="{{ render: LogoGithub20 }}"
        href="https://github.com/IBM/carbon-components-svelte"
        target="_blank"
        rel="noopener"
      />
      <HeaderAction bind:isOpen>
        <HeaderPanelLinks>
          <HeaderPanelDivider>Carbon Svelte portfolio</HeaderPanelDivider>
          <HeaderPanelLink href="https://github.com/IBM/carbon-icons-svelte">
            Carbon Icons Svelte
          </HeaderPanelLink>
          <HeaderPanelLink
            href="https://github.com/IBM/carbon-pictograms-svelte"
          >
            Carbon Pictograms Svelte
          </HeaderPanelLink>
          <HeaderPanelLink
            href="https://github.com/carbon-design-system/carbon-charts/tree/master/packages/svelte"
          >
            Carbon Charts Svelte
          </HeaderPanelLink>
          <HeaderPanelDivider>Resources</HeaderPanelDivider>
          <HeaderPanelLink href="https://www.carbondesignsystem.com/">
            Carbon Design System
          </HeaderPanelLink>
          <HeaderPanelLink href="https://www.ibm.com/design/language/">
            IBM Design Language
          </HeaderPanelLink>
        </HeaderPanelLinks>
      </HeaderAction>
    </HeaderUtilities>
  </Header>

  <SideNav bind:isOpen="{isSideNavOpen}">
    <SideNavItems>
      <SideNavMenu expanded text="Components">
        {#each components.children as child, i (child.path)}
          <SideNavMenuItem
            text="{child.title}"
            href="{$url(child.path)}"
            isSelected="{$isActive($url(child.path))}"
          />
        {/each}
      </SideNavMenu>
    </SideNavItems>
  </SideNav>
  <slot />
  <Footer />
</Theme>
