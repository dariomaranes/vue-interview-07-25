<script setup lang="ts">
import {
  AllCommunityModule,
  ModuleRegistry,
  type ColDef,
  type ICellEditorParams,
} from 'ag-grid-community'
import { AgGridVue } from 'ag-grid-vue3'
import { ref } from 'vue'
import employees from '../constants/employees.json'
import JobDropdown from '@/components/JobDropdown.vue'

ModuleRegistry.registerModules([AllCommunityModule])

const columnDef = ref<ColDef[]>([
  {
    field: 'employeeId',
  },
  {
    field: 'employeeName',
  },
  {
    field: 'businessTitle',
    headerName: 'Business/Job Title',
  },
  {
    field: 'jobFunction',
    headerName: 'Job Function (AP)',
    editable: true,
    cellEditor: 'JobDropdown',
    cellEditorPopup: true,
    cellEditorPopupPosition: 'under',
    valueGetter: (params) => params.data.jobFunction?.function,
    cellEditorParams: (params: ICellEditorParams) => {
      return {
        topResults: employees.employees.find(
          (employee) => employee.employeeId === params.data.employeeId,
        )?.jobFunction,
      }
    },
  },
])

const rowData = ref(
  employees.employees.map((employee) => ({ ...employee, jobFunction: undefined })),
)

// const gridOptions = ref([iconSetMaterial])

const components = ref({ JobDropdown })
</script>

<template>
  <div class="grid-container">
    <AgGridVue
      :column-defs="columnDef"
      :row-data="rowData"
      class="general-grid"
      :components="components"
    />
  </div>
</template>
