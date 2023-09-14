<script setup lang="ts">
import { ref, reactive } from "vue";
import Selection from "./Selection.vue";

const data = reactive({
  trans_name: "next",
  current_slide: 0,
  isDisabledP: true,
  isSelected: false,
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
      },
      {
        msgType1: true,
        msgType2: false,
        msg: ["不要"],
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
]);
// 選択内容格納リスト（仮）
const selectedItem = ref([]);
// ボタン選択イベント
const handleEvent = (newData) => {
  let a = {
    que: newData.que,
    ans: newData.ans,
  };
  // 格納用リストににデータ格納
  selectedItem.value.push(a);
  // 次の選択肢へ
  next();
  data.isSelected = true;
  console.log(data.isSelected);
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
  {{ data.current_slide }}
  <div>
    <transition-group
      :name="data.trans_name"
      tag="div"
      class="slider-outer"
    >
      <template
        v-for="(value, idx) in list"
        :key="idx"
      >
        <div
          v-if="data.current_slide === idx"
        >
          <Selection
            class="slider-inner"
            :class="{ selected: data.isSelected }"
            :key="idx"
            :value="value"
            @clickEvent="handleEvent"
          ></Selection>
        </div>
      </template>
    </transition-group>
  </div>
  <div class="btn">
    <button @click="prev()" :disabled="data.isDisabledP">前の設問に戻る</button>
  </div>
</template>
<style scoped>
.selection.selected i {
  background-color: #bbb;
}
.slider-outer {
  position: relative;
  width: 450px;
  height: 500px;
  overflow: hidden;
  margin: 0 auto 20px;
}
.slider-inner {
  position: absolute;
  width: 500px;
  height: 450px;
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
  transition: all 8s ease-out;
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
