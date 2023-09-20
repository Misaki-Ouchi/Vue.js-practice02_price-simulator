<script setup lang="ts">
import { ref, reactive, onMounted } from "vue";
import Question from "./Question.vue";
import NormalButton from "./ButtonNormal.vue";

defineProps<{
  value: {};
}>();
defineEmits(["clickEvent"]);

const data = reactive({
  sctSelected: false
})
</script>
<template>
  <Question>{{ value.title }}</Question>
  <div
  class="selection"
  >
    <template v-for="(item, idx) in value.items">
      <Normal-Button
        :id="item.value"
        :msgType1="item.msgType1"
        :msgType2="item.msgType2"
        :msgType3="item.msgType3"
        :isRecommend="item.isRecommend"
        :isSale="item.isSale"
        :data-ans="item.value"
        ref="buttons"
        @btnClick="btnClick, $emit('clickEvent', { que: value.title, ans: item.value })"
      >
        <template #icon>
          <!-- <svg></svg> -->
        </template>
        <template #detail>{{ item.msg[0] }}</template>
        <template #detail-sub>{{ item.msg[1] }}</template>
        <template #detail-sub2>{{ item.msg[2] }}</template>
        <template #recommend></template>
        <template #sale></template>
        <template #prePrice>税込{{ item.prePrice }}円</template>
        <template #salePrice>税込{{ item.salePrice }}円</template>
      </Normal-Button>
    </template>
  </div>
</template>

<style>
.selection {
  display: flex;
  flex-direction: column;
}
.selection:has(button.btnSelected) button i {
  background-color: #bbb;
}
button.btnSelected {
  border-color: rgb(255, 50, 180);
  background-color: rgba(255, 50, 180, 0.1);
}
button.btnSelected i{
  background-color: #faa !important;
}
button.btnSelected .details .detail{
  font-weight: bold;
  color: rgb(255, 50, 180);
}
</style>
