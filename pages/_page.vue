<template>
  <div class="meta">
    <Support />
    <div class="meta__main">
      <div class="meta__colour-container meta__colour-container_colour_grey-top" />
      <div class="meta__colour-container meta__colour-container_colour_white" />
      <div class="meta__colour-container meta__colour-container_colour_grey-bottom">
        <Drop class="meta__drop" />
      </div>
      <div class="meta__colour-container meta__colour-container_colour_red" />
      <LogoWhite />
      <div class="meta__container">
        <h1 ref="titleContainer" class="meta__title">
          <span v-for="line in splitTitle" :key="line" class="text-with-line">{{ line }}</span>
        </h1>
        <p class="meta__subtitle">
          {{ pageData['subtitle_' + $i18n.locale] }}
        </p>
      </div>
      <div class="meta__wrapper">
        <div class="meta__data" v-html="pageData['content_' + $i18n.locale]" />
      </div>
    </div>
  </div>
</template>

<script>
import splitLine from '@/utilites/splitLine'

export default {
  name: 'MetaPage',
  async asyncData ({ store, route, error }) {
    if (
      !store.getters.dynamicPagesData[route.params.page] ||
      !Object.keys(store.getters.dynamicPagesData[route.params.page]).length
    ) {
      await store.dispatch('dynamicPagesDataInit', route.params.page)
      if (!store.getters.dynamicPagesData[route.params.page]) {
        error({ statusCode: 404 })
      }
    }
    return {
      pageData: store.getters.dynamicPagesData[route.params.page]
    }
  },
  data () {
    return {
      splitTitle: [],
      resizeTimeout: null,
      pageData: {},
      meta: {}
    }
  },
  head() {
    return {
      title: this.pageData['title_' + this.$i18n.locale] + '|' + this.meta.title,
      meta: [
        { hid: 'description', name: 'description', content: this.pageData.Description },
        { hid: 'keywords', name: 'keywords', content: this.pageData.Keywords },
        { hid: 'og:title', name: 'og:title', content: this.pageData['title_' + this.$i18n.locale] },
        { hid: 'og:description', name: 'og:description', content: this.pageData.og_description },
        { hid: 'og:image', name: 'og:image', content: this.$config.constants.serverUrl + this.meta.logo.url },
        { hid: 'twitter:title', name: 'twitter:title', content: this.pageData['title_' + this.$i18n.locale] },
        { hid: 'twitter:description', name: 'twitter:description', content: this.pageData.og_description },
        { hid: 'twitter:image', name: 'twitter:image', content: this.$config.constants.serverUrl + this.meta.logo.url }
      ]
    }
  },
  created() {
    this.meta = this.$store.getters.globalMeta
  },
  beforeMount () {
    this.handleSplitTitle()
  },
  mounted() {
    window.addEventListener('resize', this.handleSplitTitle)
  },
  beforeDestroy () {
    window.removeEventListener('resize', this.handleSplitTitle)
  },
  methods: {
    handleSplitTitle () {
      if (!this.resizeTimeout) {
        this.resizeTimeout = setTimeout(() => {
          this.resizeTimeout = null
          this.splitTitle = splitLine(
            this.pageData['title_' + this.$i18n.locale],
            this.$refs.titleContainer,
            false,
            70
          )
        }, 40)
      }
    }
  }
}
</script>

<style scoped>
.meta {
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-self: stretch;
  align-items: center;
  width: 100%;
}

.meta__drop {
  display: none;
}

.meta__main {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
  align-self: stretch;
}

.meta__container {
  background-color: #cbcbcb;
  width: 100%;
  margin-bottom: 30px;
}

.meta__title {
  width: 80%;
  margin: 27px 0 14px 10px;
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  font-family: Vollkorn, Times, serif;
  font-size: 110px;
  line-height: 69px;
  color: #fff;
  font-style: italic;
  word-break: break-all;
}

.text-with-line {
  font-family: Vollkorn, Times, serif;
  font-size: 110px;
  line-height: 69px;
  color: #fff;
  font-style: italic;
  position: relative;
  word-break: normal;
}

.text-with-line::after {
  content: '';
  height: 15px;
  width: 100%;
  background-color: #b23438;
  bottom: 2px;
  left: 0;
  position: absolute;
}

.meta__subtitle {
  font-family: Vollkorn, Times, serif;
  font-size: 18px;
  line-height: 19px;
  font-style: italic;
  font-weight: 600;
  max-width: 72%;
  margin-left: 22px;
  margin-bottom: 18px;
}

.meta__data {
  width: 85%;
  overflow: hidden;
  margin: 0 auto;
}

@media screen and (min-width: 768px) {
  .meta__title {
    font-size: 220px;
    line-height: 137px;
    width: 87%;
    margin: 63px 0 30px 45px;
  }

  .text-with-line {
    font-size: 220px;
    line-height: 137px;
  }

  .text-with-line::after {
    height: 33px;
    width: 100%;
  }

  .meta__subtitle {
    font-size: 36px;
    line-height: 38px;
    font-style: normal;
    max-width: 72%;
    margin-left: 52px;
    margin-bottom: 52px;
  }
}

@media screen and (min-width: 1098px) {
  .meta__main {
    display: grid;
    grid-template-columns: auto 1098px auto;
    grid-template-areas:
      "grey-top title white"
      "red data grey-bottom";
    align-items: unset;
  }

  .meta__colour-container {
    width: calc((100vw - 1098px) / 2);
    height: auto;
  }

  .meta__colour-container_colour_grey-top {
    grid-area: grey-top;
    background-color: #cbcbcb;
  }

  .meta__colour-container_colour_white {
    grid-area: white;
    background-color: #fff;
  }

  .meta__colour-container_colour_grey-bottom {
    grid-area: grey-bottom;
    background-color: #cbcbcb;
    position: relative;
  }

  .meta__colour-container_colour_red {
    grid-area: red;
    background-color: #b23438;
  }

  .meta__container {
    grid-area: title;
  }

  .meta__title {
    font-size: 247px;
    line-height: 178px;
    width: 92%;
  }

  .text-with-line {
    font-size: 247px;
    line-height: 178px;
  }

  .meta__subtitle {
    font-size: 40px;
    line-height: 42px;
    max-width: 80%;
  }

  .meta__wrapper {
    grid-area: data;
    margin-top: 50px;
  }
}

@media screen and (min-width: 1280px) {
  .meta__drop {
    display: block;
    width: 36px;
    height: 62px;
  }
}
</style>
