<template>
  <div
    class="w-full flex flex-col items-center bg-[url(./assets/HKIA_Keyart_with_Logo.jpg)] bg-cover bg-no-repeat bg-blend-multiply h-screen overflow-auto"
  >
    <!-- Header -->
    <div class="bg-white/25 w-full flex flex-center item-center justify-center">
      <h1 class="text-5xl m-10">HKIA Wiki Furniture Code Generator</h1>
    </div>
    <!-- Body -->
    <div class="flex flex-wrap mt-8 w-4/5 gap-4 justify-around">
      <!-- Colors -->
      <div
        class="flex flex-col items-center grow-1 shrink basis-0 rounded-4xl bg-white/85 p-4 border gap-4"
      >
        <h2 class="text-2xl">Colors</h2>
        <div
          class="flex flex-wrap gap-4 justify-start content-start overflow-auto h-108"
        >
          <div v-for="color in colors">
            <div
              class="flex justify-center items-center gap-4 bg-white border rounded-2xl p-2 cursor-pointer h-16"
              :class="{
                '!bg-pink-100 border-pink-700':
                  selectedColor.name === color.name,
              }"
              @click="handleColor(color)"
            >
              <img :src="color.src" />
              <p class="text-xl">{{ color.name }}</p>
            </div>
          </div>
        </div>
      </div>
      <!-- Materials -->
      <div
        class="flex flex-col items-center grow-1 shrink basis-0 rounded-4xl bg-white/85 p-4 border gap-4"
      >
        <h2 class="text-2xl">Materials</h2>
        <div
          class="flex flex-wrap gap-4 justify-start content-start overflow-auto h-108"
        >
          <div class="w-full" v-for="material in materials">
            <div
              class="flex justify-around w-full items-center gap-4 bg-white border rounded-4xl p-2 cursor-pointer h-16"
              :class="{
                '!bg-pink-100 border-pink-700': material.count > 0,
              }"
            >
              <img :src="material.src" />
              <p class="text-xl">{{ material.name }}</p>
              <input
                class="w-16 bg-white border text-center focus:placeholder:text-transparent rounded-lg h-3/4"
                @input="input => handleMaterial(input, material)"
                :placeholder="0"
              />
            </div>
          </div>
        </div>
      </div>
      <!-- Dyes -->
      <div
        class="flex flex-col items-center grow-1 shrink basis-0 rounded-4xl bg-white/85 p-4 border gap-4"
      >
        <h2 class="text-2xl">Dye Pouhes</h2>
        <div
          class="flex flex-wrap gap-4 justify-start content-start overflow-auto h-108"
        >
          <div class="w-full" v-for="dye in dyes">
            <div
              class="flex justify-around w-full items-center gap-4 bg-white border rounded-4xl p-2 cursor-pointer h-16"
              :class="{
                '!bg-pink-100 border-pink-700': dye.count > 0,
              }"
            >
              <img :src="dye.src" />
              <p class="text-xl">{{ dye.name }}</p>
              <input
                class="w-16 bg-white border text-center focus:placeholder:text-transparent rounded-lg h-3/4"
                @input="input => handleDye(input, dye)"
                :placeholder="0"
              />
            </div>
          </div>
        </div>
      </div>
    </div>
    <div
      class="w-2/3 flex flex-col items-center m-4 p-4 rounded-4xl border bg-white/85"
    >
      <h2 class="text-2xl mb-4">Code</h2>
      <textarea class="bg-white w-full border font-mono min-h-40 rounded-4xl"
        >{{ code }}
      </textarea>
    </div>
  </div>
</template>

<script setup>
import colorsJson from './colors'
import materialsJson from './materials'
import dyesJson from './dyes'

import { ref, computed } from 'vue'

const colors = ref(colorsJson)
const materials = ref(materialsJson)
const dyes = ref(dyesJson)

const selectedColor = ref({})
const selectedMaterials = ref([])
const selectedDyes = ref([])
const code = computed(() => {
  var str = ''
  selectedMaterials.value.forEach(selectedMaterial => {
    str += `{{Item|${selectedMaterial.name}|${selectedMaterial.count}}},`
  })

  selectedDyes.value.forEach(selectedDye => {
    str += `{{Item|${selectedDye.name}|${selectedDye.count}}},`
  })
  str = str.slice(0, -1)

  return `
  {{FurnitureColor
  |color = ${selectedColor.value.code}
  |item= ${str}
  }}
  `
})

const handleColor = color => {
  console.log(color)
  selectedColor.value = color
  console.log(selectedColor.value.name)
  console.log(code.value)
}

const handleMaterial = (input, material) => {
  console.log(input.data)
  console.log(material)

  var selectedMaterial = materials.value.find(mat => mat.name === material.name)
  selectedMaterial.count = input.data

  selectedMaterials.value = materials.value.filter(mat => mat.count)
}

const handleDye = (input, dye) => {
  console.log(input.data)
  console.log(dye)

  var selectedDye = dyes.value.find(d => d.name === dye.name)
  selectedDye.count = input.data

  selectedDyes.value = dyes.value.filter(d => d.count)
}
</script>
