<script lang="ts">
  import '../app.css'
  import Navigation from '$lib/components/Navigation.svelte'
  import Footer from '$lib/components/Footer.svelte'
  import { page } from '$app/state'

  let { children, data } = $props()

  // Shared across every route — one org description and one social card.
  // Absolute URLs: crawlers don't resolve relative og:image/canonical.
  const SITE = 'https://gothamdataclinic.org'
  let description = $derived(data.settings?.missionStatement || 'Gotham Data Clinic is a New York City-based nonprofit whose mission is to train the next generation of scientists and technologists in computing and data science — and to engage the public in these vital conversations.')
  // Organization + WebSite structured data: lets Google show the org name and logo in results.
  const jsonLd = JSON.stringify({
    '@context': 'https://schema.org',
    '@graph': [
      { '@type': 'WebSite', name: 'Gotham Data Clinic', url: `${SITE}/` },
      { '@type': 'NGO', name: 'Gotham Data Clinic', url: `${SITE}/`, logo: `${SITE}/icon.png`, taxID: '84-3894797' },
    ],
  })
</script>

<svelte:head>
  <title>Gotham Data Clinic</title>
  <meta name="description" content={description} />
  <meta property="og:site_name" content="Gotham Data Clinic" />
  <meta property="og:type" content="website" />
  <meta property="og:description" content={description} />
  <meta name="twitter:card" content="summary_large_image" />
  <meta name="twitter:description" content={description} />
  <meta property="og:image" content="{SITE}/og-image.png" />
  <meta property="og:image:width" content="1200" />
  <meta property="og:image:height" content="630" />
  <meta property="og:image:alt" content="Gotham Data Clinic" />
  <meta name="twitter:image" content="{SITE}/og-image.png" />
  <link rel="canonical" href="{SITE}{page.url.pathname}" />
  {@html `<script type="application/ld+json">${jsonLd}</script>`}
</svelte:head>

<Navigation settings={data.settings} />
<main>
  {@render children()}
</main>
<Footer settings={data.settings} />
