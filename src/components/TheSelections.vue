<script setup lang="ts">
import { ref, reactive } from "vue";
import Selection from "./Selection.vue";
import ResultBox from "./ResultBox.vue";

// defineEmits(["selectedItem"]);

const data = reactive({
  trans_name: "next", // スライド用の識別名
  current_slide: 0, // 表示中のスライド
  prev_slide: [], // 一つ前にスライドした数
  isDisabledP: true, // 前に戻るボタンdisabled
  sctSelected: false, // 設問(済)の選択肢色変更
  btnSelected: false, // 選択したボタン色変更
  resultsShow: false, // 結果表示/非表示
  isDisOpenBtn: true, // 内訳ボタンdisabled
  showDetail: false, // 結果の内訳表示/非表示
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
        value: "通話定額 5分+",
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
        value: "通話定額 10分+",
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
        value: "かけ放題+",
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
// 選択内容格納リスト
const selectedItem = ref({
  dataVol: "-", // データ容量
  totalCost: "-", // 合計月額
  dataPlan: "", // プラン名: (音声SIM/eSIM)/(SMS)/(データ(eSIM))〇ギガプラン
  dataCost: 0, // プラン金額
  showTelPlan: false, // 通話定額プラン有無
  telPlan: "", // 通話定額プラン名
  telCost: "", // 通話定額プラン金額
});
const newList = ref([]);

// ボタン選択イベント
const handleEvent = (newData) => {
  // データ格納
  let a = {
    que: newData.que,
    ans: newData.ans,
  };
  newList.value.push(a);
  let item = selectedItem.value;
  let newItem = newList.value[newList.value.length - 1];
  // 選択された内容によって条件分岐（スライド数増）& 選択内容格納リストの中身更新
  // １．電話必要か(音声 + SIM/eSIM)
  if (newItem.ans === "電話必要") {
    item.dataPlan = "音声";
    data.prev_slide.push(0);
  }
  if (newItem.ans === "電話不要") {
    data.current_slide += 2;
    data.prev_slide.push(2);
  }
  // ２．SIM/eSIM
  if (newItem.que === "ご利用になるSIMの形状を教えてください") {
    item.dataPlan += newItem.ans;
    data.prev_slide.push(0);
  }
  // ３．通話定額
  if (newItem.que === "通話定額を使いますか？") {
    data.current_slide += 2;
    data.prev_slide.push(2);
    item.showTelPlan = true;
    item.telPlan = newItem.ans;
    // 選択リストの金額を取得
    let listVal = list.value;
    for (let i = 0; i < listVal.length; i++) {
      for (let j = 0; j < listVal[i].items.length; j++) {
        if (newItem.ans === listVal[i].items[j].value) {
          item.telCost = listVal[i].items[j].salePrice;
        }
      }
    }
  }
  if (newItem.ans === "通話定額は使わない") {
    item.showTelPlan = false;
  }
  // ４．SMS使うか
  if (newItem.ans === "SMSを使う") {
    data.current_slide++;
    data.prev_slide.push(1);
    item.dataPlan = "SMS";
  }
  if (newItem.ans === "SMSは使わない") {
    data.prev_slide.push(0);
  }
  // ５．データeSIM使うか
  if (newItem.que === "データeSIMを使いますか？") {
    item.dataPlan = "データ";
    data.prev_slide.push(0);
    if (newItem.ans === "データeSIMを使う") {
      item.dataPlan = "eSIM";
    }
  }
  // ６．使用ギガ容量
  if (newItem.ans === "2") {
    item.dataCost = 740;
    if (item.dataPlan === "SMS") {
      item.dataCost = Number(item.dataCost) + 80;
    }
  }
  if (newItem.ans === "5") {
    item.dataCost = 900;
    if (item.dataPlan === "SMS") {
      item.dataCost = Number(item.dataCost) + 70;
    }
  }
  if (newItem.ans === "10") {
    item.dataCost = 1400;
    if (item.dataPlan === "SMS") {
      item.dataCost = Number(item.dataCost) + 70;
    }
  }
  if (newItem.ans === "15") {
    item.dataCost = 1730;
    if (item.dataPlan === "SMS") {
      item.dataCost = Number(item.dataCost) + 50;
    }
  }
  if (newItem.ans === "20") {
    item.dataCost = 1950;
    if (item.dataPlan === "SMS") {
      item.dataCost = Number(item.dataCost) + 30;
    }
  }
  // 最後の選択肢
  if (newItem.que === "データ通信は毎月どのくらいご利用ですか？") {
    item.dataVol = newItem.ans;
    // 合計月額
    item.totalCost = item.dataCost + Number(item.telCost);
  }
  // 選択されなかったボタンの色変更
  data.sctSelected = true;
  // 次の選択肢へ
  next();
};
// 前の設問に戻る
const prev = () => {
  data.trans_name = "prev";
  data.current_slide -= data.prev_slide.pop() + 1;
  data.isDisabledN = false;
  if (data.current_slide <= 0) {
    data.current_slide = 0;
    // ボタン無効化
    data.isDisabledP = true;
  }
};
// 次の設問に移る
const next = () => {
  data.trans_name = "next";
  data.current_slide++;
  data.isDisabledP = false;
  if (data.current_slide > list.value.length - 1) {
    data.current_slide = list.value.length - 1;
    // 下にスライド
    // 一番上に移動する要素取得
    let elm = document.querySelector("#result");
    let h = elm.getBoundingClientRect().top;
    scrollBy({ top: h, behavior: "smooth" });
    data.isDisOpenBtn = false;
  }
};
const openBtnEvent = () => {
  data.showDetail = !data.showDetail;
};
// リセットイベント
const resetEvent = () => {
  selectedItem.value = {
    dataVol: "-",
    totalCost: "-",
    dataPlan: "",
    dataCost: 0,
    showTelPlan: false,
    telPlan: "",
    telCost: "",
  };
  newList.value = [];
  data.showDetail = false;
  data.isDisOpenBtn = true
  // 最初の選択肢に戻る
  data.trans_name = "prev";
  data.current_slide = 0;
  // スクロール
  let elm = document.querySelector("#question");
  let s = window.scrollY;
  scrollTo({ top: s, behavior: "smooth" });
  let h = elm.getBoundingClientRect().top
  scrollTo({ top: h, behavior: "smooth" });
  console.log(elm, h)
  console.log(window.pageYOffset)
};
</script>
<template>
  <div class="slider-outer">
    <transition-group :name="data.trans_name" v-for="(value, idx) in list">
      <div class="slider-inner" :key="idx" v-show="data.current_slide == idx">
        <Selection
          :key="idx"
          :value="value"
          :btnSelected="data.btnSelected"
          @clickEvent="handleEvent"
        ></Selection>
      </div>
    </transition-group>
  </div>
  <div class="btn">
    <button @click="prev()" :disabled="data.isDisabledP">前の設問に戻る</button>
  </div>
  <ResultBox
    :selectedItem="selectedItem"
    :showTelPlan="selectedItem.showTelPlan"
    :isDisOpenBtn="data.isDisOpenBtn"
    :showDetail="data.showDetail"
    @openBtnClick="openBtnEvent"
    @resetClick="resetEvent"
  >
    <template v-show="data.resultsShow" #dataVol>{{
      selectedItem.dataVol
    }}</template>
    <template v-show="data.resultsShow" #totalCost>{{
      selectedItem.totalCost
    }}</template>
    <template v-show="data.resultsShow" #dataPlan>{{
      selectedItem.dataPlan + selectedItem.dataVol
    }}</template>
    <template v-show="data.resultsShow" #dataCost>{{
      selectedItem.dataCost
    }}</template>
    <template v-show="data.resultsShow" #telPlan>{{
      selectedItem.telPlan
    }}</template>
    <template v-show="data.resultsShow" #telCost>{{
      selectedItem.telCost
    }}</template>
  </ResultBox>
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
