<script setup lang="ts">
import { useWindowControls } from "@/composables/useWindowControls";

defineProps<{
  collapsed: boolean;
}>();

const router = useRouter();
const { isFullscreen, usesNativeTrafficLights } = useWindowControls();

/** 红绿灯可见时顶栏作为 macOS 标题栏使用 */
const nativeTitle = computed(() => usesNativeTrafficLights.value && !isFullscreen.value);

const goHome = (): void => {
  router.push("/");
};
</script>

<template>
  <div
    class="flex items-center h-16 shrink-0 px-4"
    :class="nativeTitle ? 'app-drag-region' : 'justify-center'"
  >
    <div
      v-if="!nativeTitle || !collapsed"
      role="link"
      tabindex="0"
      class="app-no-drag inline-flex items-center cursor-pointer transform-gpu transition-transform duration-300 hover:scale-105 active:scale-100"
      :class="{ 'app-traffic-lights-offset': nativeTitle }"
      @click="goHome"
    >
      <SLogo :size="30" class="shrink-0" />
      <span
        class="text-[22px] text-primary mt-0.5 leading-10 overflow-hidden whitespace-nowrap font-logo transition-[width,opacity,margin] duration-300"
        :class="collapsed ? 'w-0 opacity-0 ml-0' : 'w-[90px] ml-2'"
      >
        SPlayer
      </span>
    </div>
  </div>
</template>
