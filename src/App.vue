<template>
  <div class="x" data-test="x" :dir="documentDirection">
    <SnippetConfigExtraParams />
    <SnippetCallbacks />
    <DeviceDetector @DeviceProvided="$setLocaleDevice" :breakpoints="breakpoints" />
    <Tagging />
    <UrlHandler env="env" />
    <XModal v-if="isOpen" :openEvents="openEvents" />
  </div>
</template>

<script lang="ts">
  import { SnippetCallbacks, SnippetConfig, XEvent, XOn, XProvide } from '@empathyco/x-components';
  import { DeviceDetector } from '@empathyco/x-components/device';
  import { Tagging } from '@empathyco/x-components/tagging';
  import { UrlHandler } from '@empathyco/x-components/url';
  import { SnippetConfigExtraParams } from '@empathyco/x-components/extra-params';
  import { Dictionary } from '@empathyco/x-utils';
  import { Component, Inject, Vue, Watch } from 'vue-property-decorator';
  import currencies from './i18n/currencies';
  import '@empathyco/x-components/design-system/full-theme.css';
  import './design-system/tokens.scss';

  @Component({
    components: {
      DeviceDetector,
      SnippetCallbacks,
      SnippetConfigExtraParams,
      Tagging,
      UrlHandler,
      XModal: () => import('./components/x-modal.vue').then(m => m.default)
    }
  })
  export default class Layer extends Vue {
    protected breakpoints: Dictionary<number> = {
      mobile: 640,
      tablet: 900,
      desktop: Number.POSITIVE_INFINITY
    };
    protected isOpen = false;
    protected openEvents: XEvent[] = [
      'UserClickedOpenEventsModal',
      'UserOpenXProgrammatically',
      'UserClickedOpenX'
    ];

    @XOn((component: Vue) => (component as Layer).openEvents)
    open(): void {
      this.isOpen = true;
    }

    @Inject('snippetConfig')
    protected snippetConfig!: SnippetConfig;

    protected get documentDirection(): string {
      return (
        document.documentElement.dir ||
        document.body.dir ||
        (this.snippetConfig.documentDirection ?? 'ltr')
      );
    }

    @XProvide('currencyFormat')
    public get currencyFormat(): string {
      return currencies[this.snippetConfig.currency!];
    }

    @Watch('snippetConfig.lang')
    syncLang(lang: string): void {
      this.$setLocale(lang);
    }
  }
</script>

<style scoped>
  .x-modal::v-deep .x-modal__content {
    width: 100%;
    height: 100%;
    background-color: white;
    overflow: auto;
  }
</style>
<style lang="scss">
  *:not(.x-keyboard-navigation *) {
    outline: none;
  }

  .x-banner,
  .x-promoted {
    &__title {
      display: none;
    }
  }
</style>
