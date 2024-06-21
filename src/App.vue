<template>
  <a-table :columns="columns" :data-source="data" :expand-column-width="100">
    <template #bodyCell="{ record, column }">
      <a-date-picker
        v-if="column.key === 'DateDo'"
        :value="dayjs(record[column.dataIndex])"
        @change="(value: Dayjs) => editCol(record.key, value, column.dataIndex)"
      />

      <a-date-picker
        v-if="column.key === 'DateArivInPTO'"
        :value="dayjs(record[column.dataIndex])"
        :disabled="record.loading"
        @change="(value: Dayjs) => editCol(record.key, value, column.dataIndex)"
      />
      <a-select
        v-if="column.key === 'ManOVED'"
        ref="select"
        :value="record[column.dataIndex]"
        style="width: 10vw"
        :disabled="record.loading"
        @focus="focus"
        @change="(value: string) => editCol(record.key, value, column.dataIndex)"
      >
        <a-select-option v-for="manag in managers" :value="manag.manager">{{
          manag.manager
        }}</a-select-option>
      </a-select>
      <a-select
        v-if="column.key === 'Declar'"
        ref="select"
        :value="record[column.dataIndex]"
        style="width: 10vw"
        :disabled="record.loading"
        @focus="focus"
        @change="(value: string) => editCol(record.key, value, column.dataIndex)"
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
            :checked="Boolean(record[column.dataIndex])"
            @change="(checked: Boolean) => edit2(record.key, checked, column.dataIndex)"
          />
          <a-checkbox
            v-if="column.key === 'insuranse'"
            v-model:checked="record[column.dataIndex]"
          />
          <a-checkbox
            v-if="column.key === 'TOPayment'"
            v-model:checked="record[column.dataIndex]"
          />
          <a-input-number
            v-if="column.key === 'literT'"
            :value="record[column.dataIndex]"
            :min="0"
            :controls="false"
            @change="(value: number) => edit2(record.key, value, column.dataIndex)"
          />
          <a-input-number
            v-if="column.key === 'literN'"
            :value="record[column.dataIndex]"
            :min="0"
            :controls="false"
            @change="(value: number) => edit2(record.key, value, column.dataIndex)"
          />
          <a-input-number
            v-if="column.key === 'literD'"
            :value="record[column.dataIndex]"
            :min="0"
            :controls="false"
            @change="(value: number) => edit2(record.key, value, column.dataIndex)"
          />
        </template>
      </a-table>
    </template>
    <template #expandColumnTitle>
      <span style="color: red">More</span>
    </template>
  </a-table>
  <a-button @click="importData()">get new data</a-button>
</template>
<script lang="ts" setup>
import { ref } from 'vue'
import type { Ref } from 'vue'
import dayjs, { Dayjs, type ConfigType } from 'dayjs'
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

const dataImp = ref([
  {
    key: 0,
    client: `АЭРО-ТРЕЙД ООО`,
    KTK: 'OOCU7811227',
    spec: '10.3.4.5654',
    DateDo: ref<Dayjs>(dayjs('2024-05-06')),
    DateArivInPTO: ref<Dayjs>(dayjs('2022-05-06')),
    ManOVED: 'Елецких Мария',
    Declar: 'Декларант 1',
    innerData: [
      {
        key: 11,
        morfRaht: false,
        insuranse: false,
        TOPayment: false,
        literT: 0,
        literN: 0,
        literD: 0
      }
    ]
  },
  {
    key: 1,
    client: `АЭРО-ТРЕЙД ООО`,
    KTK: 'TRHU4300411',
    spec: '10.3.4.5654',
    DateDo: ref<Dayjs>(dayjs('2024-05-06')),
    DateArivInPTO: ref<Dayjs>(dayjs('2022-05-06')),
    ManOVED: 'Елецких Мария',
    Declar: 'Декларант 2',
    innerData: [
      {
        key: 22,
        morfRaht: false,
        insuranse: false,
        TOPayment: false,
        literT: 0,
        literN: 0,
        literD: 0
      }
    ]
  }
])
const data = ref(JSON.parse(localStorage.getItem('dannye') || '{}'))
function editCol(key: number, it: any, col: string) {
  console.log(it)
  console.log(data.value[key])
  data.value.filter((item: any) => key === item.key)[key][col] = it
  localStorage.setItem('dannye', JSON.stringify(data.value))

  data.value[key].loading = true
  setTimeout(() => {
    data.value[key].loading = false
  }, 2000)
}
const focus = () => {
  console.log('focus')
}

function edit2(key: number, it: any, col: string) {
  console.log(key, it)
  Object.assign((data.value[0].innerData.filter((item: any) => key === item.key)[0][col] = it))
  localStorage.setItem('dannye', JSON.stringify(data.value))
}

function importData() {
  localStorage.setItem('dannye', JSON.stringify(dataImp.value))
}
</script>
