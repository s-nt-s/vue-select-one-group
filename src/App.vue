<script setup lang="ts">
  import { ref } from 'vue';
  import { SelectOneGroup } from './components/';

  function rg(a:number, b:number){
    if (b === undefined) {
      b = a;
      a = 1;
    }
    return [...Array(b-a+1).keys()].map(x => x+a);
  }

  const vals = ref([]);
</script>

<template>
      <SelectOneGroup
        v-model="vals"
        multiple
        required
        style="width:200px;"
        size="12"
      >
        <optgroup v-for="g in rg(1, 3)" :label="'Group '+g">
          <option v-for="i in rg(1, 4-g+1)" :value="g+'x'+i">
            Item {{g}}x{{i}}
          </option>
        </optgroup>
      </SelectOneGroup>
      <p>
        Value: {{vals.join(", ")}}
      </p>
</template>