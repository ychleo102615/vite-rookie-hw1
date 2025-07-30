<script setup>

import { ref } from 'vue'

const titles = [
  "品項",
  "描述",
  "價格",
  "庫存",
]

const menuList = ref([
  ["珍珠奶茶",   "香濃奶茶搭配",           50, 20 ],
  ["冬瓜檸檬",   "清新冬瓜配上新鮮檸檬",   45, 18 ],
  ["翡翠檸檬",   "綠茶與檸檬的完美結合",   55, 34 ],
  ["四季春茶",   "香醇四季春茶",           45, 10 ],
  ["阿薩姆奶茶", "阿薩姆紅茶搭配香醇鮮奶", 50, 25 ],
  ["檸檬冰茶",   "檸檬與冰茶的清新組合",   45, 20 ],
  ["芒果綠茶",   "芒果與綠茶的獨特風味",   55, 18 ],
  ["抹茶拿鐵",   "抹茶與鮮奶的絕配",       60, 20 ],
]);

const editingIndex = ref(-1);

function decrement(index) {
  if (menuList.value[index][3] > 0) {
    menuList.value[index][3]--;
  }
}

const limit = 35;

function increment(index) {
  if (menuList.value[index][3] < limit) {
    menuList.value[index][3]++;
  }
}

function startEditing(index) {
  editingIndex.value = index;
}

function stopEditing() {
  editingIndex.value = -1;
}

</script>

<template>
  <table>
    <thead>
      <th
        v-for="title in titles"
        :key="title"
        scope="col"
        >
        {{ title }}
      </th>
    </thead>
    <tbody>
      <tr
        v-for="(item, index) in menuList"
        :key="index"
        >
        <td>
          <div>
            <span v-if="editingIndex != index" @dblclick="startEditing(index)">{{ item[0] }}</span>
            <div v-else>
              <input
                v-model="item[0]"
                @keyup.enter="stopEditing"
                @keyup.esc="stopEditing"
              />
              <button @click="stopEditing">完成</button>
            </div>
          </div>
        </td>
        <td><small>{{ item[1] }}</small></td>
        <td>{{ item[2] }}</td>
        <td>
          <button
            @click="decrement(index)"
            :disabled="item[3] <= 0"
          >-</button>
          {{ item[3] }} {{ item[3] >= limit ? '(已達上限)' : '' }}
          <button
            @click="increment(index)"
            :disabled="item[3] >= limit"
          >+</button>
        </td>
      </tr>
    </tbody>
  </table>
  <p>雙擊品項名稱可編輯</p>
</template>

<style scoped></style>
