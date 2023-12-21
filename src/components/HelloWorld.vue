<script setup lang="ts">
import { ref } from 'vue'
import { Grid } from '@progress/kendo-vue-grid'
import { Button, ButtonGroup } from "@progress/kendo-vue-buttons"
import converter from "number-to-words"

const COLUMNS = [
  { field: "name", title: "Name" },
  { title: "Optional 1", id: "o1", cell: "empty" },
  { title: "Optional 2", id: "o2", cell: "randomString" },
  { field: "number", title: "Number" },
  { title: "Optional 3", id: "o3", cell: "randomString" },
  { title: "empty", cell: "empty" },
  { field: "number", title: "Number 2" },
  { title: "Optional 4", id: "o4", field: "number2" },
  { field: "number", title: "Number 3" },
  { title: "Optional 5", id: "o5", cell: "randomString" },
];

const data = ref([gen(1), gen(2), gen(125478541), getRandom(), getRandom()])
const columns = ref(COLUMNS);

function gen(num: number): any {
  return { number: num, name: converter.toWords(num), number2: Math.ceil(Math.random() * 100) }
}
function getRandom(): any {
  return gen(100000000 + Math.ceil(Math.random() * 100000000))
}
function hide(col: string) {
  columns.value!.filter(x => x.id == col).forEach((x: any) => x.hidden = !x.hidden);
}
</script>

<template>
  <ButtonGroup>
    <Button @click="() => data.push(getRandom())">Add</Button>
    <Button @click="() => data.push(gen(Math.ceil(Math.random() * 100)))">Add Small</Button>
  </ButtonGroup>
  <ButtonGroup>
    <Button @click="() => hide('o1')">Toggle optional 1</Button>
    <Button @click="() => hide('o2')">Toggle optional 2</Button>
    <Button @click="() => hide('o3')">Toggle optional 3</Button>
    <Button @click="() => hide('o4')">Toggle optional 4</Button>
    <Button @click="() => hide('o5')">Toggle optional 5</Button>
  </ButtonGroup>
  <Grid :data-items="data" :columns="columns">
    <template v-slot:empty>
      <td></td>
    </template>
    <template v-slot:randomString="{ props }">
      <td>{{ props.dataItem.randomString || 'some-string' }}</td>
    </template>
  </Grid>
</template>

<style scoped>
.k-button-group {
  margin-bottom: 1em;
  margin-left: 1em;
}

.k-button-group:first-child {
  margin-left: 0;
}
</style>
