<template>
  <div class="wrap">
    <div class="monthPicker">
      <div class="btn" @click="changeMonth(false)">&lt;</div>
      <div>{{ displayDate.format("YYYY年MM月") }}</div>
      <div class="btn" @click="changeMonth(true)">&gt;</div>
    </div>
    <div class="days" @click="select">
      <div class="weekText" v-for="(item, index) in week" :key="index">
        {{ item }}
      </div>
      <div
        v-for="(item, index) in days"
        :key="index"
        :class="{
          dateItem: item,
          selected:
            selectedDate.format('YYYY-MM-DD') ===
            displayDate.format('YYYY-MM-') + (item < 10 ? '0' + item : item),
        }"
      >
        {{ item }}
      </div>
    </div>
  </div>
  <br />
  <div>选中日期：{{ selectedDate.format("YYYY年MM月DD日") }}</div>
</template>

<script setup>
import dayjs from "dayjs";
import { ref, onBeforeMount } from "vue";
let displayDate = ref(dayjs(dayjs().format("YYYY-MM") + "-01"));
let selectedDate = ref(dayjs());
const week = ["日", "一", "二", "三", "四", "五", "六"];
const days = ref([]);
onBeforeMount(() => {
  updateCalendar();
});
const changeMonth = (next) => {
  if (next) {
    displayDate.value = displayDate.value.add(1, "month");
  } else {
    displayDate.value = displayDate.value.subtract(1, "month");
  }
  days.value.splice(0, days.value.length);
  updateCalendar();
};
const updateCalendar = () => {
  let breakCount = displayDate.value.day();
  while (breakCount > 0) {
    days.value.unshift("");
    breakCount--;
  }
  let daysInMonth = selectedDate.value.daysInMonth();
  let num = 1;
  while (daysInMonth > 0) {
    days.value.push(num);
    daysInMonth--;
    num++;
  }
};
const select = (event) => {
  if (event.target.className !== "dateItem") {
    return;
  }
  const addDay =
    event.target.innerHTML.length < 2
      ? "0" + event.target.innerHTML
      : event.target.innerHTML;
  selectedDate.value = dayjs(displayDate.value.format("YYYY-MM-") + addDay);
};
</script>

<style lang='less' scoped>
.wrap {
  width: 329px;
  background: #131313;
  border-radius: 10px;
  padding: 24px 14px;
  font-size: 14px;
  .monthPicker {
    display: flex;
    align-items: center;
    justify-content: space-between;
    color: #ffffff;
    .btn {
      width: 33px;
      height: 33px;
      line-height: 33px;
      text-align: center;
      border-radius: 50%;
      background: rgba(255, 255, 255, 0.2);
    }
  }
  .days {
    margin-top: 12px;
    display: flex;
    flex-wrap: wrap;
    color: #fff;
    div {
      width: 35px;
      height: 35px;
      text-align: center;
      line-height: 35px;
      margin: 2px 6px;
      border-radius: 8px;
      box-sizing: border-box;
      border: 1px solid #131313;
    }
    .weekText {
      color: #3848d1;
      font-size: 12px;
    }
    .dateItem {
      cursor: pointer;
    }
    .dateItem:hover {
      border: 1px solid #3848d1;
    }
    .selected {
      background-color: #3848d1;
    }
  }
}
</style>