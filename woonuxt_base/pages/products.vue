<script setup>
const { setProducts, updateProductList, products } = useProducts();
const { isQueryEmpty } = useHelpers();

const { data } = await useAsyncGql('getProducts');
const allProducts = data.value?.products?.nodes || [];
setProducts(allProducts);

onMounted(() => {
  if (!isQueryEmpty.value) updateProductList();
});

useHead({
  title: 'Products',
  meta: [{ hid: 'description', name: 'description', content: 'Products' }],
});
</script>

<template>
  <div class="container flex items-start gap-16" v-if="allProducts.length">
    <Filters />

    <div class="w-full">
      <div class="flex items-center justify-between w-full gap-4 mt-8 md:gap-8">
        <ProductResultCount />
        <OrderByDropdown class="hidden md:inline-flex" />
        <LazyShowFilterTrigger class="md:hidden" />
      </div>
      <ProductGrid />
      <LazyNoProductsFound />
    </div>
  </div>
</template>
