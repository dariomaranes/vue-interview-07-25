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
    valueGetter: (params) => params.data.jobFunction?.function || '-',
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

// const gridOptions = ref([iconSetMaterial])

const jobDropDown = ref({ JobDropdown })
</script>

<template>
  <AgGridVue
    :column-defs="columnDef"
    :row-data="rowDataDef"
    class="general-grid"
    :components="jobDropDown"
  />
</template>
