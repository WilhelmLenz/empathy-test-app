<template>
  <MainScrollItem :item="result" tag="article" class="x-result">
    <BaseResultLink class="x-result__picture" :result="result">
      <BaseResultImage :result="result" class="x-picture--colored">
        <template #placeholder>
          <BasePlaceholderImage />
        </template>
        <template #fallback>
          <BaseFallbackImage />
        </template>
      </BaseResultImage>
    </BaseResultLink>

    <div class="x-result__overlay x-list x-list--horizontal">
      <BaseAddToCart :result="result" class="x-list__item--expand">
        <CartIcon />
        <span>{{ $t('result.addToCart') }}</span>
      </BaseAddToCart>
    </div>

    <BaseResultLink class="x-result__description x-list x-list--gap-04" :result="result">
      <div class="x-list x-list--gap-02">
        <h1 class="x-small x-text--bold">{{ result.season }}</h1>
        <h2 class="x-text x-text--secondary x-ellipsis" data-test="result-title">
          {{ result.name }}
        </h2>
      </div>

      <BaseResultCurrentPrice :result="result" class="x-text x-text--accent x-text--bold" />
    </BaseResultLink>
  </MainScrollItem>
</template>

<script lang="ts">
  import { Result } from '@empathyco/x-types';
  import {
    BaseAddToCart,
    BaseResultLink,
    BaseResultImage,
    BaseResultCurrentPrice,
    BasePlaceholderImage,
    BaseFallbackImage,
    CartIcon
  } from '@empathyco/x-components';
  import { MainScrollItem } from '@empathyco/x-components/scroll';
  import { Component, Prop, Vue } from 'vue-property-decorator';

  @Component({
    components: {
      BaseAddToCart,
      BaseFallbackImage,
      BasePlaceholderImage,
      BaseResultCurrentPrice,
      BaseResultImage,
      BaseResultLink,
      CartIcon,
      MainScrollItem
    }
  })
  export default class ResultComponent extends Vue {
    @Prop()
    public result!: Result;
  }
</script>
