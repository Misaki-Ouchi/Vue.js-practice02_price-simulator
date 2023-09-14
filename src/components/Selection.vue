<script setup lang="ts">
import { ref, reactive } from "vue";
import Question from "./Question.vue";
import NormalButton from "./ButtonNormal.vue";

defineProps<{
  value: {};
  isSelected: boolean;
}>();
defineEmits(["clickEvent"]);
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
        :isSelected="isSelected"
        :class="{ selected: isSelected }"
        @click="$emit('clickEvent', { que: value.title, ans: item.msg[0] })"
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
button {
  position: relative;
  cursor: pointer;
  margin-bottom: 1rem;
  border: 2px solid #bbb;
  border-radius: 15px;
  background-color: #eee;
  z-index: 10;
}

button.selected {
  border-color: rgb(255, 50, 180);
  background-color: rgba(255, 50, 180, 0.1);
}
</style>
