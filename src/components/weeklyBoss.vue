
<template>
  <div>
    <h1>周王收益</h1>
    <button type="button" class="btn btn-primary" @click="checkDefficult()">全打最難</button>
    <button type="button" class="btn btn-secondary" @click="clearAll()">清空</button>
    <div class="text-nowrap form-check d-flex" v-for="v in weeklyBoss" :key="v.id">
      <input class="form-check-input" type="checkbox" :id="v.id" v-model="v.select">
      <label class="form-check-label" :for="v.id">{{ v.name }}</label>
      <div v-if="v.info.length > 1">
        <select class="form-select " aria-label="Default select example" v-model="v.index">
          <option v-for="(b, i) in v.info" :value=i>{{ b.difficult }}</option>
        </select>
      </div>
    </div>
    <h1>{{ allValue }}</h1>
  </div>
</template>

<script setup>

import { ref, warn, watch, onMounted, reactive, defineEmits, defineProps } from 'vue';

const emits = defineEmits(['updateData']);
const props = defineProps(['weeklyBossArray']);

const allValue = ref(0);
const weeklyBoss = reactive([

  {
    id: "boss1", name: "炎魔", select: false, index: 0, info: [
      { difficult: "混沌", value: 7059750, max: true }]
  },
  {
    id: "boss2", name: "梅格耐斯", select: false, index: 0, info: [
      { difficult: "困難", value: 8819007, max: true }]
  },
  {
    id: "boss3", name: "希拉", select: false, index: 0, info: [
      { difficult: "困難", value: 6677700, max: true }]
  },
  {
    id: "boss4", name: "拉圖斯", select: false, index: 0, info: [
      { difficult: "混沌", value: 20088150, max: true }]
  },
  {
    id: "boss5", name: "比艾樂", select: false, index: 0, info: [
      { difficult: "混沌", value: 7313306, max: true }]
  },
  {
    id: "boss6", name: "斑斑", select: false, index: 0, info: [
      { difficult: "混沌", value: 7693781, max: true }]
  },
  {
    id: "boss7", name: "血腥皇后", select: false, index: 0, info: [
      { difficult: "混沌", value: 7682035, max: true }]
  },
  {
    id: "boss8", name: "貝倫", select: false, index: 0, info: [
      { difficult: "混沌", value: 9070003, max: true }]
  },
  {
    id: "boss9", name: "皮卡啾", select: false, index: 0, info: [
      { difficult: "混沌", value: 7630700, max: true }]
  },
  {
    id: "boss10", name: "西格諾斯", select: false, index: 0, info: [
      { difficult: "簡單", value: 5307400, max: false },
      { difficult: "普通", value: 8709400, max: true }]
  },
  {
    id: "boss11", name: "史烏", select: false, index: 0, info: [
      { difficult: "普通", value: 27207040, max: false },
      { difficult: "困難", value: 87503781, max: true }]
  },
  {
    id: "boss12", name: "戴米安", select: false, index: 0, info: [
      { difficult: "普通", value: 28843452, max: false },
      { difficult: "困難", value: 80612548, max: true }]
  },
  {
    id: "boss13", name: "守護天使綠水靈", select: false, index: 0, info: [
      { difficult: "普通", value: 39114492, max: false },
      { difficult: "混沌", value: 120450252, max: true }]
  },
  {
    id: "boss14", name: "露希妲", select: false, index: 0, info: [
      { difficult: "簡單", value: 48799200, max: false },
      { difficult: "普通", value: 58457600, max: false },
      { difficult: "困難", value: 97438242, max: true }]
  },
  {
    id: "boss15", name: "威爾", select: false, index: 0, info: [
      { difficult: "簡單", value: 52139000, max: false },
      { difficult: "一般", value: 67421200, max: false },
      { difficult: "困難", value: 109805699, max: true }]
  },
  {
    id: "boss16", name: "戴斯克", select: false, index: 0, info: [
      { difficult: "一般", value: 72293700, max: false },
      { difficult: "混沌", value: 116977942, max: true }]
  },
  {
    id: "boss17", name: "普通真．希拉", select: false, index: 0, info: [
      { difficult: "普通", value: 118489900, max: false },
      { difficult: "困難", value: 138191654, max: true }]
  },
  {
    id: "boss18", name: "頓凱爾", select: false, index: 0, info: [
      { difficult: "普通", value: 77908300, max: false },
      { difficult: "困難", value: 120151104, max: true }]
  },
  {
    id: "boss19", name: "賽蓮", select: false, index: 0, info: [
      { difficult: "普通", value: 145101237, max: false },
      { difficult: "困難", value: 262989400, max: false },
      { difficult: "終極", value: 700000000, max: true }]
  },
  {
    id: "boss20", name: "卡洛斯", select: false, index: 0, info: [
      { difficult: "混沌", value: 300000000, max: true }]
  },
  {
    id: "boss21", name: "咖凌", select: false, index: 0, info: [
      { difficult: "普通", value: 350000000, max: true }]
  },
  {
    id: "boss22", name: "濃姬", select: false, index: 0, info: [
      { difficult: "普通", value: 8953200, max: true }]
  }
]);

watch(props, async () => {
  var i = 0;
  if (props.weeklyBossArray != null) {
    weeklyBoss.forEach(v => {
      if (props.weeklyBossArray.array.includes(v.id)) {
        v.select = true;
        v.index = props.weeklyBossArray.index[i];
                i++
      }
      else {
        v.select = false;
      }
    });
  }
  else {
    weeklyBoss.forEach(v => {
      v.select = false;
    });
  }
});

watch(weeklyBoss, async () => {
  allValue.value = 0;
  weeklyBoss.forEach((temp) => {
    if (temp.select) {
      allValue.value = allValue.value + temp.info[temp.index].value;
    }
  });
  updateData(allValue);
})

function checkDefficult() {
  weeklyBoss.forEach(v => {
    v.info.forEach((w, i) => {
            if (w.max) {
                v.select = true;
                v.index = i;
            }
        });
    
  });
}

function clearAll() {
  weeklyBoss.forEach(v => {
    v.select = false;
  })
}

function updateData(count) {
  const bossArray = [];
  const index = [];
  weeklyBoss.forEach(v => {
    if (v.select == true) {
      bossArray.push(v.id);
      index.push(v.index);
    }
  })
  emits('updateData', bossArray,index, count);
}

</script>