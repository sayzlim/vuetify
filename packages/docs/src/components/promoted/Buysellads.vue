<template>
  <template v-if="ad && ad.statlink">
    <div id="carbonads" v-html="carbonEl"></div>
  </template>
  <div id="bsa-zone_1691166982595-9_123456" v-else></div>
</template>

<script>
import { IN_BROWSER } from '@/util/globals';

const timestamp = Date.now() % 600000;

const buildCarbonHtml = (ad) => {
  let pixelEl = '';

  if (ad.pixel) {
    pixelEl = pixel(ad.pixel, timestamp);
  }

  const carbonEl = `
      <span>
        <span class="carbon-wrap">
          <a href="${ad.statlink}" class="carbon-img" target="_blank" rel="noopener sponsored"><img src="${ad.smallImage}" alt="ads via Carbon" border="0" height="100" width="130" style="max-width: 130px"/></a>
          <a href="${ad.statlink}" class="carbon-text" target="_blank" rel="noopener sponsored">${ad.description}</a>
        </span>
        <a href="${ad.ad_via_link}" class="carbon-poweredby" target="_blank" rel="noopener sponsored">ads via Carbon</a>
      </span>
      ${pixelEl}
    `;

  return carbonEl;
};

const loadOptimizeTag = () => {
  if (!IN_BROWSER) return;

  const script = document.createElement('script');
  script.type = 'text/javascript';
  script.src = `https://cdn4.buysellads.net/pub/vuetifyjs.js?${timestamp}`;

  const head =
    document.head ||
    document.getElementsByTagName('head')[0];
  head.appendChild(script);
};

const pixel = (pixels, timestamp) => {
  let c = '';
  if (pixels)
    pixels.split('||').forEach((pixel, index) => {
      c +=
        '<img src="' +
        pixel.replace('[timestamp]', timestamp) +
        '" style="display:none;" height="0" width="0" />';
    });
  return c;
};

export default {
  data() {
    return {
      ad: null,
      carbonEl: null,
      optimizeScript: null,
    };
  },
  mounted() {
    fetch('https://srv.buysellads.com/ads/CWYDC27W.json')
      .then((response) => response.json())
      .then((responseData) => {
        const ad = responseData.ads[0];

        this.ad = ad;
        if (!!ad.statlink) {
          this.carbonEl = buildCarbonHtml(ad);
          console.log(this.carbonEl);
        } else {
          loadOptimizeTag();
        }
      })
      .catch((error) => {
        console.error('Error fetching data:', error);
      });
  },
};
</script>

<style lang="sass">
  @media only screen and (min-width: 0px) and (min-height: 0px)
    div[id^="bsa-zone_1691166982595-9_123456"]
      min-width: 300px
      min-height: 250px

  @media only screen and (min-width: 760px) and (min-height: 0px)
    div[id^="bsa-zone_1691166982595-9_123456"]
      min-width: 728px
      min-height: 90px

  #carbonads-script
    width: 100%

  #carbonads
    width: 100%
    max-width: 360px

    > span
      display: flex
      position: relative

    .carbon-wrap
      display: flex

    .carbon-text,
    .carbon-poweredby
      max-width: 200px
      padding: 0 0 0 16px
      text-decoration: none

    .carbon-img
      display: inline-flex
      margin: 0.5rem

      img
        border-radius: 4px 0 0 4px
        max-height: 100px

    .carbon-text
      color: inherit
      font-size: 0.75rem
      padding: 0.475rem

    .carbon-poweredby
      bottom: 0.5rem
      font-size: 0.625rem
      font-weight: 400
      letter-spacing: 0.09375rem
      position: absolute
      right: 0.5rem
      text-transform: uppercase

  .v-app-ad.theme--light
    .carbon-poweredby
      color: rgba(0, 0, 0, .6)

  .v-app-ad.theme--dark
    .carbon-poweredby
      color: inherit
</style>
