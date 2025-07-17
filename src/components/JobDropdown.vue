<script setup lang="ts">
import { computed, ref } from 'vue'
import jobFunctions from '../constants/jobFunctions.json'
import { type ICellEditorParams } from 'ag-grid-community'

interface JobFunction {
  jobFunctionId: number
  function: string
}

interface TopResults extends JobFunction {
  matchingScore: number
}

const props = defineProps<{ params: ICellEditorParams & { topResults: TopResults[] } }>()

const selectedValue = ref()

console.log('selectedValueeeee', selectedValue)

const searchQuery = ref('')

const topResults = ref(props.params.topResults)
const jobFunctionData: JobFunction[] = jobFunctions.jobFunctions

const filteredList = computed(() =>
  jobFunctionData.filter(
    (jobFun) =>
      jobFun.function.toLocaleLowerCase().includes(searchQuery.value.toLocaleLowerCase()) &&
      !topResults.value.some((option) => option.jobFunctionId === jobFun.jobFunctionId),
  ),
)

const filteredTopResults = computed(() =>
  topResults.value.filter((jobFun) =>
    jobFun.function.toLocaleLowerCase().includes(searchQuery.value.toLocaleLowerCase()),
  ),
)

console.log('filteredList', filteredList)
console.log('filteredTopResults', filteredTopResults)

const handleOnClick = (job: JobFunction) => {
  selectedValue.value = job
  props.params.stopEditing()
}

const getValue = () => selectedValue.value

defineExpose({ getValue })
</script>
<template>
  <div class="dropdown-container">
    <input type="text" placeholder="Search" v-model="searchQuery" class="search-input" />
    <hr />
    <div>
      <p class="section-title" v-show="filteredTopResults.length">Top 3 AI Results</p>
      <ul>
        <li
          v-for="job in filteredTopResults"
          :key="job.jobFunctionId"
          @click="handleOnClick(job)"
          class="job-item"
        >
          {{ job.function }}
        </li>
      </ul>
    </div>
    <hr />
    <div>
      <p class="section-title">Job Function (AP)</p>
      <ul>
        <li
          v-for="job in filteredList"
          :key="job.jobFunctionId"
          @click="handleOnClick(job)"
          class="job-item"
        >
          {{ job.function }}
        </li>
      </ul>
    </div>
    <hr />
  </div>
</template>
