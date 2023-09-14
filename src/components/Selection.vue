<script setup lang="ts">
import { ref, reactive } from "vue";
import Question from "./Question.vue";
import NormalButton from "./ButtonNormal.vue";

defineProps<{
  value: {};
}>();
defineEmits(["clickEvent"]);

</script>
<template>
  <Question>{{ value.title }}</Question>
  <template v-for="item in value.items">
    <div class="selection">
      <Normal-Button
        :isSelected="item.isSelected"
        :msgType1="item.msgType1"
        :msgType2="item.msgType2"
        :isRecommend="item.isRecommend"
        :isSale="item.isSale"
        @click="$emit('clickEvent', {que: value.title, ans: item.msg[0]})"
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
    </div>
  </template>
</template>

<style scoped>
.selection {
  display: flex;
  flex-direction: column;
}
</style>
