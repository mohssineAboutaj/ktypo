<template>
  <main
    class="container mx-auto pt-16 h-screen flex justify-center items-center"
  >
    <div class="card md:w-4/6">
      <h1 class="card-title capitalize mx-auto font-black text-4xl mb-4">
        {{ title }}
      </h1>
      <h3 class="card-title capitalize mx-auto">{{ subtitle }}</h3>
      <div class="card-body">
        <div class="grid grid-cols-5 gap-y-8 gap-x-4">
          <div class="col-span-5 md:col-span-2">
            <label for="text" class="label">
              {{ contentTxt.text.label }}
            </label>
            <textarea
              id="text"
              v-model="form.text"
              autofocus
              class="textarea textarea-bordered textarea-primary w-full"
              rows="10"
              :placeholder="contentTxt.text.label"
              @input="reverseText"
              :dir="currentLayout === keyboardLayouts[0] ? 'rtl' : 'ltr'"
            ></textarea>
          </div>
          <div class="col-span-5 md:col-span-1 flex justify-center items-start">
            <div class="grid grid-cols-2 md:grid-cols-1 gap-4 md:mt-10">
              <select
                class="select select-primary w-full"
                v-model="currentLayout"
              >
                <option
                  v-for="(layout, index) in keyboardLayouts"
                  :key="index"
                  :value="layout"
                >
                  {{ layout }}
                </option>
              </select>
              <button
                class="btn btn-primary uppercase w-full"
                @click="reverseText"
                :class="{ 'btn-disabled': form.text.length === 0 }"
              >
                <span>reverse</span>
              </button>
            </div>
          </div>
          <div class="col-span-5 md:col-span-2">
            <label for="result" class="label">
              {{ contentTxt.result.label }}
            </label>
            <textarea
              id="result"
              v-model="form.result"
              class="textarea textarea-bordered w-full bg-primary bg-opacity-25 font-bold"
              rows="10"
              :dir="currentLayout === keyboardLayouts[0] ? 'ltr' : 'rtl'"
              readonly
            ></textarea>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
import { reactive, ref } from "vue"
import { useLayouts } from "@/compasables/layouts"

// data
/// static
const title = require("../package.json").name
const subtitle = "text reverse"
const contentTxt = {
  text: {
    label: "type your text to encode/transform",
  },
  result: {
    label: "result of encoding/transform",
  },
}
const keyboardLayouts = ["arabic", "english"]
/// dynamic
const form = reactive({
  text: "",
  result: "",
})
const currentLayout = ref(keyboardLayouts[0])

// methods
/**
 * @description transform text from arabic to english as in typing, the function will just replace letters based on it places in keyboard QWERTY layout, for example: "مرحبا" will be "lvpfh"
 *
 * @param {string} char
 * @param {string} layout
 *
 * @returns {string}
 */
const transformText = (char) => {
  const { arabicLayout, englishLayout } = useLayouts()
  let newChar = ""

  // chart not in both arrays, return it as it is
  if (arabicLayout.concat(englishLayout).indexOf(char) === -1) {
    newChar = char
  } else {
    // replace char based on given layout
    if (currentLayout.value === keyboardLayouts[0]) {
      newChar = englishLayout[arabicLayout.indexOf(char)]
    } else if (currentLayout.value === keyboardLayouts[1]) {
      newChar = arabicLayout[englishLayout.indexOf(char)]
    } else {
      newChar = char
    }

    // in newChar is undefined, return it as it is
    if (newChar === undefined) {
      newChar = char
    }
  }

  // return new char
  return newChar
}
/**
 * @description get text and reverse it
 *
 * @param {string} text
 *
 * @returns void
 */
const reverseText = () => {
  let output = ""
  const text = form.text

  for (let i = 0; i < text.length; i++) {
    output += transformText(text[i])
  }

  form.result = output
}
</script>
