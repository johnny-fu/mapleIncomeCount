
<template>
    <div>
        <h1>日王收益</h1>
        <button type="button" class="btn btn-primary" @click="checkDefficult()">全打最難</button>
        <button type="button" class="btn btn-danger" @click="checkRed()">紅石</button>
        <button type="button" class="btn btn-secondary" @click="clearAll()">清空</button>
        <div class="text-nowrap form-check d-flex" v-for="v in dailyBoss" :key="v.id">
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
const props = defineProps(['dailyBossArray']);
const allValue = ref(0);
const dailyBoss = reactive([
    {
        id: "boss1", name: "炎魔", select: false, index: 0, info: [
            { difficult: '簡單', value: 115800, red: false, max: false },
            { difficult: '普通', value: 354800, red: true, max: true }]
    },
    {
        id: "boss2", name: "梅格耐斯", select: false, index: 0, info: [
            { difficult: "簡單", value: 418300, red: false, max: false },
            { difficult: "普通", value: 1501700, red: true, max: true }]
    },
    {
        id: "boss3", name: "卡翁", select: false, index: 0, info: [
            { difficult: "一般", value: 724200, red: false, max: true }]
    },
    {
        id: "boss4", name: "森蘭丸", select: false, index: 0, info: [
            { difficult: "普通", value: 375500, red: false, max: false },
            { difficult: "困難", value: 1543700, red: false, max: true }]
    },
    {
        id: "boss5", name: "比艾樂", select: false, index: 0, info: [
            { difficult: "普通", value: 560800, red: true, max: true }]
    },
    {
        id: "boss6", name: "斑斑", select: false, index: 0, info: [
            { difficult: "普通", value: 560800, red: true, max: true }]
    },
    {
        id: "boss7", name: "血腥皇后", select: false, index: 0, info: [
            { difficult: "普通", value: 560800, red: true, max: true }]
    },
    {
        id: "boss8", name: "貝倫", select: false, index: 0, info: [
            { difficult: "普通", value: 560800, red: true, max: true }]
    },
    {
        id: "boss9", name: "闇黑龍王", select: false, index: 0, info: [
            { difficult: "簡單", value: 511000, red: false, max: false },
            { difficult: "普通", value: 586600, red: false, max: false },
            { difficult: "混沌", value: 783300, red: true, max: true }]
    },
    {
        id: "boss10", name: "阿卡伊農", select: false, index: 0, info: [
            { difficult: "簡單", value: 667400, red: false, max: false },
            { difficult: "普通", value: 1460300, red: true, max: true }]
    },
    {
        id: "boss11", name: "凡雷恩", select: false, index: 0, info: [
            { difficult: "簡單", value: 612900, red: false, max: false },
            { difficult: "普通", value: 844700, red: false, max: false },
            { difficult: "困難", value: 1419500, red: true, max: true }]
    },
    {
        id: "boss12", name: "皮卡啾", select: false, index: 0, info: [
            { difficult: "普通", value: 813700, red: true, max: true }]
    },
    {
        id: "boss13", name: "培羅德", select: false, index: 0, info: [
            { difficult: "頂級", value: 1, red: false, max: true }]
    }
]);

watch(props, async () => {
    var i = 0;
    if (props.dailyBossArray != null) {
        dailyBoss.forEach(v => {
            if (props.dailyBossArray.array.includes(v.id)) {
                v.select = true;
                v.index = props.dailyBossArray.index[i];
                i++
            }
            else {
                v.select = false;
            }
        });
    }
    else {
        dailyBoss.forEach(v => {
            v.select = false;
        });
    }
});

watch(dailyBoss, async () => {
    allValue.value = 0;
    dailyBoss.forEach((temp) => {
        if (temp.select) {
            allValue.value = allValue.value + temp.info[temp.index].value;
        }
    });
    updateData(allValue);
});

function checkDefficult() {
    dailyBoss.forEach(v => {
        v.info.forEach((w, i) => {
            if (w.max) {
                v.select = true;
                v.index = i;
            }
        });
    });
}

function checkRed() {
    dailyBoss.forEach(v => {
        v.info.forEach((w, i) => {
            if (w.red) {
                v.select = true;
                v.index = i;
            } else {
                v.select = false;
            }
        });
    });
}
function clearAll() {
    dailyBoss.forEach(v => {
        v.select = false;
    });
}

function updateData(count) {
    const bossArray = [];
    const index = [];
    dailyBoss.forEach(v => {
        if (v.select == true) {
            bossArray.push(v.id);
            index.push(v.index);
        }
    });
    emits('updateData', bossArray, index, count);
}

</script>