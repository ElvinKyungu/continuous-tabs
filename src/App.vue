<script setup lang="ts">
import { animate } from 'motion'
import { ref, onMounted } from 'vue'

const items = ref<HTMLLIElement[]>([])
const activeIndex = ref(0)

const moveIndicator = (index: number) => {
  if (items.value[index]) {
    const target = items.value[index].getBoundingClientRect()
    const nav = items.value[index].parentElement!.getBoundingClientRect()

    // Animation pour déplacer l'indicateur
    animate(
      '.indicator',
      {
        x: target.left - nav.left,
        width: target.width,
      },
      {
        duration: 0.4,
        easing: 'ease-in-out',
        type: 'spring',
      },
    )

    activeIndex.value = index
  }
}

onMounted(() => {
  moveIndicator(0)
})
</script>

<template>
  <main class="h-screen w-full flex justify-center items-center">
    <nav class="relative">
      <div
        class="indicator absolute h-full bg-black rounded-full z-0"
        style="top: 0; left: 0; width: 0; transition: none"
      ></div>
      <ul
        class="flex justify-between shadow-md py-1 items-center px-5 border border-gray-100 rounded-full gap-4 relative z-10"
        ref="nav"
      >
        <li
          v-for="(item, index) in ['Home', 'Interactions', 'Resources', 'Docs']"
          :key="index"
          class="cursor-pointer py-2 px-4 relative z-10 transition-colors duration-300"
          :class="{ 'text-white': activeIndex === index }"
          @click="moveIndicator(index)"
          ref="items"
        >
          {{ item }}
        </li>
      </ul>
    </nav>
  </main>
</template>

<style scoped>
.indicator {
  transition:
    transform 0.4s ease-in-out,
    width 0.4s ease-in-out;
  height: calc(100% - 0.5rem);
  margin: 0.25rem 0;
}
li {
  position: relative;
  z-index: 1;
}
</style>
