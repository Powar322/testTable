<template>
  <a-table :columns="columns" :data-source="data" :expand-column-width="100">
    <template #bodyCell="{ record, column }">
      <a-date-picker
        v-if="column.key === 'DateDo'"
        :value="dayjs(record[column.dataIndex])"
        @change="(value: Dayjs) => editDo(record.key, value)"
      />

      <a-date-picker
        v-if="column.key === 'DateArivInPTO'"
        :value="dayjs(record[column.dataIndex])"
        :disabled="record.loading"
        @change="(value: Dayjs) => editArriv(record.key, value)"
      />
      <a-select
        v-if="column.key === 'ManOVED'"
        ref="select"
        v-model:value="record[column.dataIndex]"
        style="width: 10vw"
        :disabled="record.loading"
        @focus="focus"
        @change="edit(record.key, record[column.dataIndex]), load(record.key)"
      >
        <a-select-option v-for="manag in managers" :value="manag.manager">{{
          manag.manager
        }}</a-select-option>
      </a-select>
      <a-select
        v-if="column.key === 'Declar'"
        ref="select"
        v-model:value="record[column.dataIndex]"
        style="width: 10vw"
        :disabled="record.loading"
        @focus="focus"
        @change="edit(record.key, record[column.dataIndex]), load(record.key)"
      >
        <a-select-option v-for="declarant in Declarants" :value="declarant.declarant">{{
          declarant.declarant
        }}</a-select-option>
      </a-select>
    </template>
    <template #expandedRowRender="{ record }">
      <a-table :columns="innerColumns" :data-source="record.innerData" :pagination="false">
        <template #bodyCell="{ column, record }">
          <a-checkbox
            v-if="column.key === 'morfRaht'"
            v-model:checked="record[column.dataIndex]"
            :change="edit2(record.key, record[column.dataIndex])"
          />
          <a-checkbox
            v-if="column.key === 'insuranse'"
            v-model:checked="record[column.dataIndex]"
            :change="edit2(record.key, record[column.dataIndex])"
          />
          <a-checkbox
            v-if="column.key === 'TOPayment'"
            v-model:checked="record[column.dataIndex]"
            :change="edit2(record.key, record[column.dataIndex])"
          />
          <a-input-number
            v-if="column.key === 'literT'"
            v-model:value="record[column.dataIndex]"
            :min="0"
            :controls="false"
            :change="edit2(record.key, record[column.dataIndex])"
          />
          <a-input-number
            v-if="column.key === 'literN'"
            v-model:value="record[column.dataIndex]"
            :min="0"
            :controls="false"
            :change="edit2(record.key, record[column.dataIndex])"
          />
          <a-input-number
            v-if="column.key === 'literD'"
            v-model:value="record[column.dataIndex]"
            :min="0"
            :controls="false"
            :change="edit2(record.key, record[column.dataIndex])"
          />
        </template>
      </a-table>
    </template>
    <template #expandColumnTitle>
      <span style="color: red">More</span>
    </template>
  </a-table>
</template>
<script lang="ts" setup>
import { ref, reactive } from 'vue'
import type { Ref, UnwrapRef } from 'vue'
import dayjs, { Dayjs, type ConfigType } from 'dayjs'
import { cloneDeep } from 'lodash-es'
interface DataItem {
  key: number
  client: string
  KTK: string
  spec: string
  DateDo: Dayjs
  DateArivInPTO: Dayjs
  ManOVED: string
  Declar: string
  innerData: [
    {
      key: number
      morfRaht: boolean
      insuranse: boolean
      TOPayment: boolean
      literT: number
      literN: number
      literD: number
    }
  ]
}
const editableData: UnwrapRef<Record<string, DataItem>> = reactive({})

const managers = ref([{ manager: 'Какой-то менеджер' }, { manager: 'Крутой менеджер' }])
const Declarants = ref([{ declarant: 'Декларант123' }, { declarant: 'НЕдлекра321' }])

const columns = [
  { title: 'Клиент', dataIndex: 'client', key: 'client' },
  { title: 'КТК', dataIndex: 'KTK', key: 'KTK' },
  { title: 'Спец', dataIndex: 'spec', key: 'spec' },
  { title: 'Дата ДО', dataIndex: 'DateDo', key: 'DateDo' },
  { title: 'Дата прибытия', dataIndex: 'DateArivInPTO', key: 'DateArivInPTO' },
  { title: 'Менеджер ОВЭД', dataIndex: 'ManOVED', key: 'ManOVED' },
  { title: 'Декларант', dataIndex: 'Declar', key: 'Declar' }
]
const innerColumns = [
  { title: 'МФ', dataIndex: 'morfRaht', key: 'morfRaht', width: 20 },
  { title: 'Страхование', dataIndex: 'insuranse', key: 'insuranse', width: 20 },
  { title: 'Таможенные платежи', dataIndex: 'TOPayment', key: 'TOPayment', width: 20 },
  { title: 'Т', dataIndex: 'literT', key: 'literT', width: 20 },
  { title: 'Н', dataIndex: 'literN', key: 'literN', width: 20 },
  { title: 'Д', dataIndex: 'literD', key: 'literD' }
]

//const data = ref([
//  {
//    key: 0,
//    client: `АЭРО-ТРЕЙД ООО`,
//    KTK: 'OOCU7811227',
//    spec: '10.3.4.5654',
//    DateDo: ref<Dayjs>(dayjs('2024-05-06')),
//    DateArivInPTO: ref<Dayjs>(dayjs('2022-05-06')),
//    ManOVED: 'Елецких Мария',
//    Declar: 'Декларант 1',
//    innerData: [
//      {
//        key: 11,
//        morfRaht: false,
//        insuranse: false,
//        TOPayment: false,
//        literT: 0,
//        literN: 0,
//        literD: 0
//      }
//    ]
//  },
//  {
//    key: 1,
//    client: `АЭРО-ТРЕЙД ООО`,
//    KTK: 'TRHU4300411',
//    spec: '10.3.4.5654',
//    DateDo: ref<Dayjs>(dayjs('2024-05-06')),
//    DateArivInPTO: ref<Dayjs>(dayjs('2022-05-06')),
//    ManOVED: 'Елецких Мария',
//    Declar: 'Декларант 2',
//    innerData: [
//      {
//        key: 11,
//        morfRaht: false,
//        insuranse: false,
//        TOPayment: false,
//        literT: 0,
//        literN: 0,
//        literD: 0
//      }
//    ]
//  }
//])
//localStorage.setItem('dannye', JSON.stringify(data.value))
const data = ref(JSON.parse(localStorage.getItem('dannye') || '{}'))
function editDo(key: number, it: any) {
  editableData[key] = cloneDeep(data.value.filter((item: any) => key === item.key)[0])
  editableData[key].DateDo = data.value.filter((item: any) => key === item.key)[0].DateDo = it
  console.log(editableData[key])
  Object.assign(
    data.value.filter((item) => key === item.key),
    it
  )
  localStorage.setItem('dannye', JSON.stringify(data.value))
}

function editArriv(key: number, it: any) {
  editableData[key] = cloneDeep(data.value.filter((item: any) => key === item.key)[0])
  editableData[key].DateArivInPTO = data.value.filter(
    (item: any) => key === item.key
  )[0].DateArivInPTO = it
  console.log(editableData[key])
  Object.assign(
    data.value.filter((item) => key === item.key),
    editableData[key]
  )
  localStorage.setItem('dannye', JSON.stringify(data.value))
}
const focus = () => {
  console.log('focus')
}

function edit(key: number, it: any) {
  console.log(it)
  editableData[key] = cloneDeep(data.value.filter((item: any) => key === item.key)[0])
  console.log(editableData[key])
  Object.assign(
    data.value.filter((item) => key === item.key),
    it
  )
  localStorage.setItem('dannye', JSON.stringify(data.value))
}
function edit2(key: number, it: any) {
  Object.assign(data.value[0].innerData.filter((item) => key === item.key)[0], it)
  localStorage.setItem('dannye', JSON.stringify(data.value))
}
function load(key: number) {
  data.value[key].loading = true
  setTimeout(() => {
    data.value[key].loading = false
  }, 2000)
}
</script>
