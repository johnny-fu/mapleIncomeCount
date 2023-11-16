

<template>
    <div>
        <h1>角色列表</h1>
        <div class="list-group">
            <button type="button" class="list-group-item list-group-item-action" @click="clickCharacter(i)"
                v-for="(v, i) in characterList" :key="v.id">{{
                    v.name }}</button>
            <button type="button" class="list-group-item list-group-item-action bg-secondary" style="color:white"
                data-bs-toggle="modal" data-bs-target="#addCharacter">+</button>
        </div>
        <h1>{{ allValue }}</h1>
        <input type="text" v-model="currencyValue" class="form-control">
        <h1>{{  Math.round(allValue/currencyValue*10) / 10 }}</h1>
        <div class="modal fade" id="addCharacter" tabindex="-1" aria-labelledby="addCharacter" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="addCharacter">新增角色</h5>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body">
                        <div class="input-group mb-3">
                            <input type="text" v-model="name" class="form-control" placeholder="名稱" aria-label="Username"
                                aria-describedby="basic-addon1">
                        </div>
                        <div class="input-group mb-3">
                            <input type="text" v-model="career" class="form-control" placeholder="職業" aria-label="career"
                                aria-describedby="basic-addon1">
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">關閉</button>
                        <button type="button" class="btn btn-primary" @click="newCharacter()"
                            data-bs-dismiss="modal">新增</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <dailyBoss v-on:update-data="updateDailyBossData" :dailyBossArray="nowCharacterD" />
    <weeklyBoss v-on:update-data="updateWeeklyBossData" :weeklyBossArray="nowCharacterW"/>
</template>

<script setup>
import { ref, warn, watch, reactive, onMounted, defineComponent } from 'vue';
import weeklyBoss from '../components/weeklyBoss.vue';
import dailyBoss from '../components/dailyBoss.vue';

const name = ref();
const career = ref();
const allValue = ref(0);
const characterList = reactive([
    { id: "charter1", name: "簡單炎魔", valueD: 0, valueW: 0, career: "阿戴爾", dailyBossArray: ['boss1', 'boss10'],dailyBossIndex:[1,0], weeklyBossArray: ['boss1', 'boss10'],weeklyBossIndex:[0,0] },
    { id: "charter2", name: "簡單炎魔", valueD: 0, valueW: 0, career: "阿戴爾", dailyBossArray: ['boss1', 'boss12', 'boss11'], dailyBossIndex: [0, 0, 0], weeklyBossArray: ['boss11', 'boss10'],weeklyBossIndex:[0,0] }
]);
const nowCharacterD = ref();
const nowCharacterW = ref();
let i = 0;
const currencyValue = ref(7000000);
watch(characterList, () => {
    allValue.value = 0;
    characterList.forEach((temp) => {
        allValue.value = allValue.value + temp.valueD + temp.valueW;
    });
})

defineComponent({
    components: { 'user-data': dailyBoss }
})
function updateDailyBossData(data,dataindex, count) {
    allValue.value = 0;
    characterList[i].dailyBossArray = data;
    characterList[i].dailyBossIndex = dataindex;
    characterList[i].valueD = count;
}
function updateWeeklyBossData(data,dataindex, count) {
    allValue.value = 0;
    characterList[i].weeklyBossArray = data;
    characterList[i].weeklyBossIndex = dataindex;
    characterList[i].valueW = count;
}

function newCharacter() {
    if (name.value != null) {
        characterList.push({ id: `charter${characterList.length + 1}`, name: name.value, value: 0, career: career.value });
    }
    name.value = null;
    career.value = null;
}

function countValue() {
    allValue.value = 0;
    characterList.forEach((temp) => {
        allValue.value = allValue.value + temp.valueD + temp.valueW;
    });
}

function clickCharacter(index) {
    i = index;
    nowCharacterD.value = {array:characterList[index].dailyBossArray,index:characterList[index].dailyBossIndex};
    nowCharacterW.value = {array:characterList[index].weeklyBossArray,index:characterList[index].weeklyBossIndex};
}

onMounted(() => {
    countValue();
    clickCharacter(0);
})

</script>