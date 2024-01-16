<script setup>
import ContentForLegend from "~/components/elements/ContentForLegend.vue";
import UnitInfo from "~/components/unit/UnitInfo.vue";
import UnitMain from "~/components/unit/UnitMain.vue";

onMounted(() => {
  document.body.classList.add("overscroll-none", "overflow-hidden");
});

onUnmounted(() => {
  document.body.classList.remove("overscroll-none", "overflow-hidden");
});
</script>

<template>
  <div id="cy" class="right-0 xl:right-[34%] overscroll-none"></div>
  <div id="scroll" class="right-0 xl:right-[34%] overscroll-none">
    <div id="scrollBody">&nbsp;</div>
  </div>
  <ContentForLegend />
  <ClientOnly>
    <UnitMain />
  </ClientOnly>
  <UnitInfo/>
</template>
