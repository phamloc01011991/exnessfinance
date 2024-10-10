<script setup>
import { onMounted,ref,reactive,onUnmounted, computed} from 'vue'
import { defineProps } from 'vue'
import request from '../../utils/request'

const props = defineProps({
  name: String,
  data: Object,
  vitri:Number
});

const array = ref()
array.value = props

const value = ref()
const percent = ref()
value.value = props.data.value
percent.value = props.data.change2

function convertString(inputString) {
  var arr = inputString.split("");
  var outputString = arr[0] + arr[1] + arr[2] + "/" + arr[3] + arr[4] + arr[5];
  return outputString;
}

const idInterval = ref('')
const idIntervalRandom = ref('')
function reloadPrice(){
    idIntervalRandom.value = setInterval(()=>{
        // Tạo hàm random ''
        var price = parseFloat(array.value.data.value)
        var percent = parseFloat(array.value.data.change2)

        var per = parseFloat(randomPercentageOffset(percent)).toFixed(2)
        array.value.data.change2 = per > 0 ? "+"+per:per
        value.value = parseFloat(randomPercentageOffset(price))
    },3000)
}

// random nội dung bài 

function randomPercentageOffset(value) {
  var percentage = Math.random() * (0.008 - 0.004) + 0.001;  // Random từ 1% đến 5%
  var offset = value * percentage;
  var randomSign = Math.random() < 0.5 ? -1 : 1;  // Random dấu cộng hoặc trừ
  var randomNumber = randomSign * offset;
  var result = value + randomNumber;
  return result;
}


onMounted(()=>{
    reloadPrice();
})
onUnmounted(()=>{
})


</script>
<template>
    <RouterLink :to="{ name: 'buy', params: { id: array.name } }" class="list-item list-data__title flex">
        <div class="name" style="width:35%"><div style="text-transform: uppercase;">{{ convertString(array.name) == 'AUD/USD' ? 'USD/VND' : convertString(props.name) }}</div>
            <div class="orther">Security data</div>
        </div>
        <div class="price" style="width:33%;">
            <div :class="[array.data.change2 > 0 ? 'pdown' : 'pup']">{{ parseFloat(value).toFixed(2) }}</div>
            <div class="usd-change">{{ parseFloat(value).toFixed(2) }}$</div>
        </div>
        <div class="percent" style="width:32%"><button :class="[array.data.change2 > 0 ? 'btns btn-success' : 'btns btn-error']">{{ parseFloat(array.data.change2).toFixed(2)}}%</button></div>
    </RouterLink>
</template>
<style scoped>
.list-item{
    color: var(--text-color);
    margin-bottom:15px;
    align-items: center;
}
.list-item .price{
    text-align:right;
}
.list-item .orther{
    font-size:11px;
    color: var(--text-sub-color);
}
.list-item .usd-change{
    color: var(--text-sub-color);
    font-size:11px;
}
.list-item .percent{
    text-align: end;
}
.list-item .btns{
    display: inline-block;
    border:none;
    height:32px;
    border-radius:5px;
    width:100px;
    line-height: 30px;
    text-align: center;
    color: #fff;
}
.list-item .btn-success{
    background:#24af6c;
}
.list-item .btn-error{
    background:#f14545;
}

</style>