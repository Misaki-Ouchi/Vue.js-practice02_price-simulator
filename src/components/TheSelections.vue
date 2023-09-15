<script setup lang="ts">
import { ref, reactive } from "vue";
import Selection from "./Selection.vue";

const data = reactive({
  trans_name: "next",
  current_slide: 0,
  isDisabledP: true,
  sctSelected: false,
  btnSelected: false,
});
const list = ref([
  {
    title: "電話番号（090等）は必要ですか？",
    items: [
      {
        msgType1: true,
        msgType2: false,
        msg: ["必要", "（お乗り換えの方はこちら）"],
        isRecommend: false,
        isSale: false,
        isSelected: false,
        value: "電話必要",
      },
      {
        msgType1: true,
        msgType2: false,
        msg: ["不要"],
        isRecommend: false,
        isSale: false,
        isSelected: false,
        value: "電話不要",
      },
    ],
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
        isSelected: false,
        value: "SIM",
      },
      {
        msgType1: true,
        msgType2: false,
        msg: ["eSIM※"],
        isRecommend: false,
        isSale: false,
        isSelected: false,
        value: "eSIM",
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
        isSelected: false,
        value: "通話定額5分",
      },
      {
        msgType1: false,
        msgType2: true,
        msg: ["通話定額10分+", "1回10分以内の国内通話無料"],
        isRecommend: false,
        isSale: true,
        prePrice: "700",
        salePrice: "290",
        isSelected: false,
        value: "通話定額10分",
      },
      {
        msgType1: false,
        msgType2: true,
        msg: ["かけ放題+", "無制限で国内通話無料"],
        isRecommend: false,
        isSale: true,
        prePrice: "1400",
        salePrice: "990",
        isSelected: false,
        value: "かけ放題",
      },
      {
        msgType1: true,
        msgType2: false,
        msg: ["通話定額は使わない"],
        isRecommend: false,
        isSale: false,
        isSelected: false,
        value: "通話定額は使わない",
      },
    ],
  },
  {
    title: "SMSを使いますか？",
    items: [
      {
        msgType1: true,
        msgType2: false,
        msg: ["SMSを使う"],
        isRecommend: false,
        isSale: false,
        isSelected: false,
        value: "SMSを使う",
      },
      {
        msgType1: true,
        msgType2: false,
        msg: ["SMSは使わない"],
        isRecommend: false,
        isSale: false,
        isSelected: false,
        value: "SMSは使わない",
      },
    ],
  },
  {
    title: "データeSIMを使いますか？",
    items: [
      {
        msgType1: true,
        msgType2: false,
        msg: ["データeSIMを使う"],
        isRecommend: false,
        isSale: false,
        isSelected: false,
        value: "データeSIMを使う",
      },
      {
        msgType1: true,
        msgType2: false,
        msg: ["データeSIMは使わない"],
        isRecommend: false,
        isSale: false,
        isSelected: false,
        value: "データeSIMは使わない",
      },
    ],
  },
  {
    title: "データ通信は毎月どのくらいご利用ですか？",
    items: [
      {
        msgType1: true,
        msgType2: false,
        msg: ["家のWi-Fi利用が中心", "外出先でネットはあまり使わない"],
        isRecommend: false,
        isSale: false,
        isSelected: false,
        value: "2",
      },
      {
        msgType1: true,
        msgType2: false,
        msg: ["外出先のSNS利用やネット閲覧が中心"],
        isRecommend: true,
        isSale: false,
        isSelected: false,
        value: "5",
      },
      {
        msgType1: true,
        msgType2: false,
        msg: ["外出先でゲームアプリを使う"],
        isRecommend: false,
        isSale: false,
        isSelected: false,
        value: "10",
      },
      {
        msgType1: true,
        msgType2: false,
        msg: ["外出先で動画を視聴しネットもよく使う"],
        isRecommend: false,
        isSale: false,
        isSelected: false,
        value: "15",
      },
      {
        msgType1: true,
        msgType2: false,
        msg: ["外出先でギガ（データ量）を気にせず使いたい"],
        isRecommend: false,
        isSale: false,
        isSelected: false,
        value: "20",
      },
    ],
  },
]);
// 選択内容格納リスト（仮）
const selectedItem = ref([]);
// ボタン選択イベント
const handleEvent = (newData) => {
  // 格納用リストににデータ格納
  let a = {
    que: newData.que,
    ans: newData.ans,
  };
  selectedItem.value.push(a);
  // 選択されなかったボタンの色変更
  data.sctSelected = true;
  // 選択された内容によって条件分岐（スライド数増）
  if (a.ans === "電話不要") {
    data.current_slide += 2;
  }
  if (a.ans === "SMSを使う") {
    data.current_slide++;
  }
  if (a.que === "通話定額を使いますか？") {
    data.current_slide += 2;
  }
  // 次の選択肢へ
  console.log(a.que);
  next();
  console.log(data.current_slide);
};
// 前の設問に戻る
const prev = () => {
  data.trans_name = "prev";
  data.current_slide--;
  data.isDisabledN = false;
  if (data.current_slide <= 0) {
    data.current_slide = 0;
    // ボタン無効化
    data.isDisabledP = true;
    // 選択データ初期化
    selectedItem.value = [];
  }
  // 一つ前の選択データ削除
  selectedItem.value.pop();
};
// 次の設問に移る
const next = () => {
  data.trans_name = "next";
  data.current_slide++;
  data.isDisabledP = false;
  if (data.current_slide >= list.value.length - 1) {
    data.current_slide = list.value.length - 1;
    // 下にスライド
  }
};
</script>
<template>
  {{ selectedItem }}
  <div class="slider-outer">
    <transition-group :name="data.trans_name" v-for="(value, idx) in list">
      <div class="slider-inner" :key="idx" v-show="data.current_slide == idx">
        <Selection
          :key="idx"
          :value="value"
          :btnSelected="data.btnSelected"
          :sctSelected="data.sctSelected"
          @clickEvent="handleEvent"
        ></Selection>
      </div>
    </transition-group>
  </div>
  <div class="btn">
    <button @click="prev()" :disabled="data.isDisabledP">前の設問に戻る</button>
  </div>
</template>
<style scoped>
.slider-outer {
  position: relative;
  width: 450px;
  height: 550px;
  overflow: hidden;
  margin: 0 auto 20px;
}
.slider-inner {
  position: absolute;
  top: 0;
  width: 450px;
  height: 550px;
}
.btn {
  position: relative;
  margin: 0 auto 20px;
  width: 8rem;
}
.next-enter-active,
.next-leave-active,
.prev-enter-active,
.prev-leave-active {
  transition: all 0.5s ease-out;
}
.next-enter-from {
  transform: translateX(450px);
}
.next-enter-to {
  transform: translateX(0px);
}
.next-leave-from {
  transform: translateX(0);
}
.next-leave-to {
  transform: translateX(-450px);
}
.prev-enter-from {
  transform: translateX(-450px);
}
.prev-enter-to {
  transform: translateX(0);
}
.prev-leave-from {
  transform: translateX(0);
}
.prev-leave-to {
  transform: translateX(450px);
}
</style>
