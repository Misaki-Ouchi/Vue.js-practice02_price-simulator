<script setup lang="ts">
import { ref } from "vue";
import Selection from "./Selection.vue";
const list = ref([
  {
    title: "電話番号（090等）は必要ですか？",
    items: [
      {
        msgType1: true,
        msgType2: false,
        msg: ['必要', '（お乗り換えの方はこちら）'],
        isRecommend: false,
        isSale: false,
      },
      {
        msgType1: true,
        msgType2: false,
        msg: ['不要'],
        isRecommend: false,
        isSale: false,
      },
    ],
  },
  {
    title: "ご利用になるSIMの形状を教えてください",
    items: [
      {
        msgType1: true,
        msgType2: false,
        msg: ['SIMカード', '（カード型）'],
        isRecommend: false,
        isSale: false,
      },
      {
        msgType1: true,
        msgType2: false,
        msg: ['eSIM※'],
        isRecommend: false,
        isSale: false,
      },
    ],
  },
  {
    title: "通話定額を使いますか？",
    items: [
      {
        msgType1: false,
        msgType2: true,
        msg: ['通話定額5分+', '1回5分以内の国内通話無料'],
        isRecommend: false,
        isSale: true,
        prePrice: "500",
        salePrice: "90",
      },
      {
        msgType1: false,
        msgType2: true,
        msg: ['通話定額10分+', '1回10分以内の国内通話無料'],
        isRecommend: false,
        isSale: true,
        prePrice: "700",
        salePrice: "290",
      },
      {
        msgType1: false,
        msgType2: true,
        msg: ['かけ放題+', '無制限で国内通話無料'],
        isRecommend: false,
        isSale: true,
        prePrice: "1400",
        salePrice: "990",
      },
      {
        msgType1: true,
        msgType2: false,
        msg: ['通話定額は使わない'],
        isRecommend: false,
        isSale: false,
      },
    ],
  },
]);
</script>
<template>
  <template v-for="value in list">
    <Selection
      :value="value"
    ></Selection>
  </template>
</template>
