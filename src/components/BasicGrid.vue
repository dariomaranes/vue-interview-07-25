<script setup lang="ts">
import { type ColDef, type ICellEditorParams } from 'ag-grid-community'
import { AgGridVue } from 'ag-grid-vue3'
import { ref } from 'vue'
import employees from '../constants/employees.json'
import JobDropdown from '@/components/JobDropdown.vue'

const columnDef = ref<ColDef[]>([
  {
    field: 'employeeId',
    headerName: 'Employee ID',
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
    valueFormatter: (params) => params.value || 'Select a job function 🔽',
    cellEditorParams: (params: ICellEditorParams) => {
      return {
        topResults: employees.employees.find(
          (employee) => employee.employeeId === params.data.employeeId,
        )?.jobFunction,
      }
    },
  },
])

const rowDataDef = ref(
  employees.employees.map((employee) => ({ ...employee, jobFunction: undefined })),
)

const gridOptions = {
  defaultColDef: {
    filter: true,
  },
  components: {
    JobDropdown,
  },
  singleClickEdit: true,
}
</script>

<template>
  <AgGridVue
    :column-defs="columnDef"
    :row-data="rowDataDef"
    :grid-options="gridOptions"
    class="general-grid"
  />
</template>
