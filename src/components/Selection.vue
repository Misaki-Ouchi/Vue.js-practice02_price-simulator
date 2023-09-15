<script setup lang="ts">
import { ref, reactive } from "vue";
import Question from "./Question.vue";
import NormalButton from "./ButtonNormal.vue";

defineProps<{
  value: {};
}>();
defineEmits(["clickEvent"]);
const data = reactive({
  deSelected: false,
  isClicked: false
  })
const getValue = (event) => {
    console.log(event.target.dataset.value)
}

</script>
<template>
  <Question>{{ value.title }}</Question>
  <div class="selection">
    <template v-for="(item, idx) in value.items">
      <Normal-Button
        :msgType1="item.msgType1"
        :msgType2="item.msgType2"
        :isRecommend="item.isRecommend"
        :isSale="item.isSale"
        :class="{ deSelected: data.deSelected }"
        :data-value="item.msg[0]"
        @click="data.deSelected = true, getValue($event), $emit('clickEvent', { que: value.title, ans: item.msg[0] })"
      >
        <template #icon>
          <!-- <svg></svg> -->
        </template>
        <template #detail>{{ item.msg[0] }}</template>
        <template #detail-sub>{{ item.msg[1] }}</template>
        <template #recommend></template>
        <template #sale></template>
        <template #prePrice>税込{{ item.prePrice }}円</template>
        <template #salePrice>税込{{ item.salePrice }}円</template>
      </Normal-Button>
    </template>
  </div>
</template>

<style scoped>
.selection {
  display: flex;
  flex-direction: column;
}
</style>
