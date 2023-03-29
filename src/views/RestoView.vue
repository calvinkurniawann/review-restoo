<script setup>
import { useRestoRepository } from "@/composables";
import { ref, onMounted } from "vue";
import BaseCard from "../components/BaseCard.vue";
import BaseContainer from "../components/BaseContainer.vue";


const repository = useRestoRepository();
const isLoading = ref(true);
const restos = ref([]);
const fetchRestos = async () => {
  isLoading.value = true;
  try {
    const { data } = await repository.index();
    restos.value = data;
  } catch (e) {
    console.error(e);
  }
  isLoading.value = false;
};
onMounted(() => fetchRestos());
const excerpt = (text, maxLenght = 10, indicator = "...") => {
  let textCopy = text;
  if (textCopy.length > maxLenght) {
    textCopy = textCopy.substring(0, maxLenght) + indicator;
  }
  return textCopy;
};
</script>

<template>
    <div class="">
    <nav
      class="
        container
        px-6
        py-6
        mx-auto
        md:flex md:justify-between md:items-center
      "
    >
      <div class="flex items-center justify-between">
        <router-link
          to="/"
          class="
            text-4xl
            font-semibold
            text-[#4FF8F4]
            md:text-4xl "
          >DishDash
        </router-link>
        <!-- Mobile menu button -->
        <div @click="toggleNav" class="flex md:hidden">
          <button
            type="button"
            class="
              text-gray-100
              hover:text-gray-400
              focus:outline-none focus:text-gray-400
            "
          >
            <svg viewBox="0 0 24 24" class="w-6 h-6 fill-current">
              <path
                fill-rule="evenodd"
                d="M4 5h16a1 1 0 0 1 0 2H4a1 1 0 1 1 0-2zm0 6h16a1 1 0 0 1 0 2H4a1 1 0 0 1 0-2zm0 6h16a1 1 0 0 1 0 2H4a1 1 0 0 1 0-2z"
              ></path>
            </svg>
          </button>
        </div>
      </div>

      <!-- Mobile Menu open: "block", Menu closed: "hidden" -->
      <ul
        :class="showMenu ? 'flex' : 'hidden'"
        class="
          flex-col
          mt-8
          space-y-4
          md:flex md:space-y-0 md:flex-row md:items-center md:space-x-10 md:mt-0
        "
      >
        <router-link to="" ><li class="text-gray-100 hover:text-[#4FF8F4]">Home</li></router-link>
        <router-link to="" ><li class="text-gray-100 hover:text-[#4FF8F4]">About</li></router-link>
        <router-link to="" ><li class="text-gray-100 hover:text-[#4FF8F4]">Contact Us</li></router-link>
        <router-link to="" ><li class="text-gray-100 hover:text-[#4FF8F4]">Create Resto</li></router-link>
      </ul>
    </nav>
  </div>
  <BaseContainer>
    <div class="grid grid-cols-12 gap-4">
      <div v-for="resto in restos" :key="resto.id" class="col-span-4">
        <!-- Card -->
        <BaseCard :to="{ name: 'restos-show', params: { id: resto.id } }">
          <template #title>{{ resto.name }}</template>
          {{ excerpt(resto.description, 40) }}
        </BaseCard>
      </div>
    </div>
  </BaseContainer>
</template>
<script>
import { ref } from 'vue';
export default {
  setup() {
    let showMenu = ref(false);
    const toggleNav = () => (showMenu.value = !showMenu.value);
    return { showMenu, toggleNav };
  },
};
</script>
