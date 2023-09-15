<script setup lang="ts">
import { ref, reactive } from "vue";
import Selection from "./Selection.vue";
import ResultBox from "./ResultBox.vue";

// defineEmits(["selectedItem"]);

const data = reactive({
  trans_name: "next", // スライド用の識別名
  current_slide: 0, // 表示中のスライド
  isDisabledP: true, // 前に戻るボタンdisabled
  sctSelected: false, // 設問(済)の選択肢色変更
  btnSelected: false, // 選択したボタン色変更
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
const aaa = ref([]);

// ボタン選択イベント
const handleEvent = (newData) => {
  // データ格納
  let a = {
    que: newData.que,
    ans: newData.ans,
  };
  let item = selectedItem.value;
  aaa.value.push(a);
  // 選択された内容によって条件分岐（スライド数増）& 選択内容格納リストの中身更新
  // １．電話必要か(音声 + SIM/eSIM)
  if (a.ans === "電話必要") {
    item.dataPlan = "音声";
  }
  if (a.ans === "電話不要") {
    data.current_slide += 2;
  }
  // ２．SIM/eSIM
  if (a.que === "ご利用になるSIMの形状を教えてください") {
    item.dataPlan += a.ans;
  }
  // ３．通話定額
  if (a.que === "通話定額を使いますか？") {
    data.current_slide += 2;
    item.showTelPlan = true;
    item.telPlan = a.ans;
    // 選択リストの金額を取得
    let list = list.value.items;
    for (let i = 0; i < list.length; i++) {
      if (a.ans === list[i].salePrice) {
        item.telCost = list[i].salePrice;
      }
    }
  }
  if (a.ans === "通話定額は使わない") {
    item.showTelPlan = false;
  }
  // ４．SMS使うか
  if (a.ans === "SMSを使う") {
    data.current_slide++;
    item.dataPlan = "SMS";
  }
  // ５．データeSIM使うか
  if (a.que === "データeSIMを使いますか？") {
    item.dataPlan = "データ";
    if (a.ans === "データeSIMを使う") {
      item.dataPlan = "eSIM";
    }
  }
  // ６．使用ギガ容量
  if (a.que === "データ通信は毎月どのくらいご利用ですか？") {
    item.dataVol = a.ans;
    item.dataPlan += a.ans;
  }
  if (a.ans === "2") {
    item.dataCost += 740;
    if (item.dataPlan === "SMS") {
      item.dataCost += 80;
    }
  }
  if (a.ans === "5") {
    item.dataCost += 900;
    if (item.dataPlan === "SMS") {
      item.dataCost += 70;
    }
  }
  if (a.ans === "10") {
    item.dataCost += 1400;
    if (item.dataPlan === "SMS") {
      item.dataCost += 70;
    }
  }
  if (a.ans === "15") {
    item.dataCost += 1730;
    if (item.dataPlan === "SMS") {
      item.dataCost += 50;
    }
  }
  if (a.ans === "20") {
    item.dataCost += 1950;
    if (item.dataPlan === "SMS") {
      item.dataCost += 30;
    }
  }

  // 次の選択肢へ
  next();
  // 選択されなかったボタンの色変更
  data.sctSelected = true;
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
  {{ aaa }}
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
  <ResultBox
    :selectedItem="selectedItem"
    :showTelPlan="selectedItem.showTelPlan"
  >
    <template #dataVol>{{ selectedItem.dataVol }}</template>
    <template #totalCost>{{ selectedItem.totalCost }}</template>
    <template #dataPlan>{{ selectedItem.dataPlan }}</template>
    <template #dataCost>{{ selectedItem.dataCost }}</template>
    <template #telPlan>{{ selectedItem.telPlan }}</template>
    <template #telCost>{{ selectedItem.telCost }}</template>
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
