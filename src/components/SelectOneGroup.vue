<script setup lang="ts">
import { ref, unref, computed } from "vue";

const prop = defineProps(["modelValue"]);
const emit = defineEmits(["update:modelValue"]);
const localSelect = ref(null);

let localValue: string[] = [];

function gV(slc: HTMLSelectElement): string[] {
  return Array.from(slc.options)
    .filter((option) => option.selected)
    .map((option) => option.value);
}

function select_one_group(event: Event) {
  if (event == null || event.target == null) return;
  if (!(event.target instanceof HTMLSelectElement)) return;
  const slc = (event.target as HTMLSelectElement);
  const vals = gV(slc);
  const diff = vals.filter((x) => !localValue.includes(x));
  if (diff.length == 0 && vals.length == localValue.length) return;
  if (diff.length > 0) {
    slc.querySelectorAll("optgroup").forEach((e) => {
      if (e.querySelector("option[value='" + diff[0] + "']") == null) {
        e.querySelectorAll("option").forEach((o) => (o.selected = false));
      }
    });
  }
  localValue = gV(slc);
  emit("update:modelValue", localValue);
}

const computedValue = computed({
  get() {
    return prop.modelValue;
  },
  set(vals: string[]) {
    const diff = vals.filter((x) => !localValue.includes(x));
    if (diff.length == 0 && vals.length == localValue.length) return;
    const lcv = unref(localSelect);
    if (lcv == null) return;
    const slc = (lcv as HTMLSelectElement);
    if (diff.length > 0) {
      slc.querySelectorAll("optgroup").forEach((e) => {
        if (e.querySelector("option[value='" + diff[0] + "']") == null) {
          e.querySelectorAll("option").forEach((o) => (o.selected = false));
        }
      });
    }
    localValue = gV(slc);
    emit("update:modelValue", localValue);
  },
});
</script>
<template>
  <select multiple v-model="computedValue" ref="localSelect">
    <slot></slot>
  </select>
</template>
