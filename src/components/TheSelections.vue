<script setup lang="ts">
import { ref, reactive } from "vue";
import Selection from "./Selection.vue";

const data = reactive({
  trans_name: "next",
  current_slide: 0,
  isDisabledP: true,
});
const list = [
  {
    title: "電話番号（090等）は必要ですか？",
    items: [
      {
        msgType1: true,
        msgType2: false,
        msg: ["必要", "（お乗り換えの方はこちら）"],
        isRecommend: false,
        isSale: false,
      },
      {
        msgType1: true,
        msgType2: false,
        msg: ["不要"],
        isRecommend: false,
        isSale: false,
      },
    ]
  },
  {
    title: "ご利用になるSIMの形状を教えてください",
    items: [
      {
        msgType1: true,
        msgType2: false,
        msg: ["SIMカード", "（カード型）"],
        isRecommend: false,
        isSale: false,
      },
      {
        msgType1: true,
        msgType2: false,
        msg: ["eSIM※"],
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
        msg: ["通話定額5分+", "1回5分以内の国内通話無料"],
        isRecommend: false,
        isSale: true,
        prePrice: "500",
        salePrice: "90",
      },
      {
        msgType1: false,
        msgType2: true,
        msg: ["通話定額10分+", "1回10分以内の国内通話無料"],
        isRecommend: false,
        isSale: true,
        prePrice: "700",
        salePrice: "290",
      },
      {
        msgType1: false,
        msgType2: true,
        msg: ["かけ放題+", "無制限で国内通話無料"],
        isRecommend: false,
        isSale: true,
        prePrice: "1400",
        salePrice: "990",
      },
      {
        msgType1: true,
        msgType2: false,
        msg: ["通話定額は使わない"],
        isRecommend: false,
        isSale: false,
      },
    ],
  },
];
// 選択内容格納リスト（仮）
const selectedItem = ref([]);
// ボタン選択イベント
const handleEvent = (newName) => {
  let a = {
    que: newName.que,
    ans: newName.ans,
  };
  selectedItem.value.push(a);
  next();
};
const prev = () => {
  data.trans_name = "prev";
  data.current_slide--;
  data.isDisabledN = false;
  if (data.current_slide <= 0) {
    data.current_slide = 0;
    data.isDisabledP = true;
  }
};
const next = () => {
  data.trans_name = "next";
  data.current_slide++;
  data.isDisabledP = false;
  if (data.current_slide >= list.length - 1) {
    data.current_slide = list.length - 1;
    // 下にスライド
  }
};
// listにisSelected入れて、ボタン押したらtrue/false格納
</script>
<template>
  {{ selectedItem }}
  <div class="slider-outer">
    <transition-group :name="data.trans_name">
      <div class="slider-inner" :key="idx" v-for="(value, idx) in list">
        <Selection
          :key="idx"
          v-if="data.current_slide === idx"
          :value="value"
          @clickEvent="handleEvent"
        ></Selection>
      </div>
    </transition-group>
  </div>
  <div class="btn">
    <button @click="prev()" :disabled="data.isDisabledP">prev</button>
  </div>
</template>
<style scoped>
.slider-outer {
  position: relative;
  width: 450px;
  height: 450px;
  overflow: hidden;
  margin: 0 auto 20px;
}
.slider-inner {
  position: absolute;
  width: 450px;
  height: 450px;
}
.btn {
  position: relative;
  margin: 0 auto 20px;
  width: 90px;
}
.next-enter-active,
.next-leave-active,
.prev-enter-active,
.prev-leave-active {
  transition: all 0.8s ease-out;
}
.next-enter {
  transform: translateX(450px);
}
.next-enter-to {
  transform: translateX(0);
}
.next-leave {
  transform: translateX(0);
}
.next-leave-to {
  transform: translateX(-450px);
}
.prev-enter {
  transform: translateX(-450px);
}
.prev-enter-to {
  transform: translateX(0);
}
.prev-leave {
  transform: translateX(0);
}
.prev-leave-to {
  transform: translateX(450px);
}
</style>
