<script setup>
// import ContentForLegend from "~/components/elements/ContentForLegend.vue";
// import UnitInfo from "~/components/unit/UnitInfo.vue";
// import UnitMain from "~/components/unit/UnitMain.vue";

import fetchUnitInfo from "~/api/fetchUnitInfo.js";
import { useGlobalStateStore } from "~/stores/globalState.js";
import { useInfoStore } from "~/stores/info.js";

const globalState = useGlobalStateStore();
const infoStore = useInfoStore();

const { info } = storeToRefs(infoStore);
const { $socket } = useNuxtApp();

const router = useRouter();
const route = useRoute();

async function getUnitInfo(unit) {
  if (info && info.unit === unit) return;

  const result = await fetchUnitInfo($socket, unit);
  if (result.deleted) {
    globalState.setLastUnit("");
    // deletedHandler(unit);
    // alertShown.value = true;
    // setTimeout(hideAlert, 3000);
    // await router.push(`/`);
  } else {
    globalState.setLastUnit(result.unit);
  }
  infoStore.setInfo(result);
  infoStore.setReady(true);
}

const getUnitInformation = async () => {
  if (route.params.unit) {
    await getUnitInfo(route.params.unit);
    // $socket.emit(EventNames.GetUnit, { unit: route.params.unit }, updDag);
    return;
  }
  //
  // await getLastUnits();
}

await getUnitInformation();

onMounted(() => {
  document.body.classList.add("overscroll-none", "overflow-hidden");
});

onUnmounted(() => {
  document.body.classList.remove("overscroll-none", "overflow-hidden");
});
</script>

<template>
  <div>YUNIT: {{ route.params.unit }}</div>
  <div id="cy" class="right-0 xl:right-[34%] overscroll-none"></div>
  <div id="scroll" class="right-0 xl:right-[34%] overscroll-none">
    <div id="scrollBody">&nbsp;</div>
  </div>
<!--  <ContentForLegend />-->
<!--  <ClientOnly>-->
<!--    <UnitMain />-->
<!--  </ClientOnly>-->
<!--  <UnitInfo/>-->
</template>
