<script setup lang="ts">
import { ref, reactive } from "vue";

defineProps<{
  showTelPlan: boolean;
  isDisOpenBtn: boolean;
  showDetail: boolean;
}>();
defineEmits(["resetClick", "openBtnClick"]);

</script>
<template>
  <div id="result" class="result">
    <div class="result-wrap">
      <h2>診断結果</h2>
      <div class="result-box">
        <div class="campaign">
          <p class="campaign-title">キャンペーン適用で</p>
          <div class="campaign-box">
            <div class="campaign-item">
              <span>回線数</span>
              <div class="campaign-data">
                <p>1</p>
                <span>回線</span>
              </div>
            </div>
            <div class="campaign-item">
              <span>データ容量</span>
              <div class="campaign-data">
                <p><slot name="dataVol"></slot></p>
                <span>GB</span>
              </div>
            </div>
            <div class="campaign-item">
              <span>月額</span>
              <div class="campaign-data cost-data">
                <p><slot name="totalCost"></slot></p>
                <span>円</span>
              </div>
            </div>
          </div>
        </div>
        <!-- /.campaign -->
        <button @click="$emit('openBtnClick')" class="openBtn" :disabled="isDisOpenBtn">
          内訳
        </button>
        <div v-show="showDetail" class="detail">
          <div class="detail-title"><p>内訳</p></div>
          <div class="detail-number"><div>1</div></div>
          <div class="detail-plans">
            <div class="detail-plan1">
              <p><slot name="dataPlan"></slot>ギガプラン</p>
              <p><slot name="dataCost"></slot>円</p>
            </div>
            <div v-show="showTelPlan" class="detail-plan2">
              <p><slot name="telPlan"></slot></p>
              <p><slot name="telCost"></slot>円</p>
            </div>
          </div>
          <div class="btn-wrap">
            <button @click="$emit('resetClick')" class="resetBtn">
              ×
            </button>
          </div>
        </div>
        <!-- /.detail -->
      </div>
      <!-- /.result-box -->
    </div>
  </div>
</template>
<style scoped>
.result-wrap {
  width: 90%;
  margin: 2rem auto;
  background-color: #b74;
  padding: 1rem;
  text-align: center;
}
h2 {
  color: #fff;
  font-weight: bold;
  padding: 0.3rem;
  border-bottom: 1px solid #fff;
  margin-bottom: 0.3rem;
}
.result-box {
  position: relative;
  padding: 0.5rem;
  background-color: #ffe;
}
.campaign {
  width: 80%;
}
.campaign-title {
  color: #fff;
  background-color: rgb(255, 50, 180);
  border-radius: 0.3rem;
}
.campaign-box {
  display: flex;
  flex: 1;
  justify-content: space-around;
  width: 100%;
  padding: 0.3rem;
}
.campaign-item {
  width: calc(100% / 3);
  color: #400;
  font-weight: bold;
  margin: 0.4rem 0;
}
.campaign-item:not(:last-child) {
  border-right: 1px solid #b74;
}
.campaign-item span {
  width: 100%;
  background-color: #fff;
  font-weight: bold;
  font-size: 0.9rem;
}
.campaign-data p {
  display: inline;
}
.cost-data {
  color: rgb(255, 50, 180);
}
.campaign-data span {
  background-color: transparent;
}
.campaign-item p {
  font-size: 1.2rem;
}
.openBtn {
  position: absolute;
  right: 1rem;
  top: 4rem;
}
.detail {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #fff;
  border-top: 1px solid #b74;
  border-bottom: 1px solid #b74;
}
.detail-title {
  width: 15%;
}
.detail-title p {
  font-weight: bold;
  color: #b74;
  background-color: #fed;
  padding: 2rem 0;
}
.detail-number {
  display: flex;
  justify-content: center;
  width: 10%;
  background-color: #eed;
  padding: 2rem 1rem;
}
.detail-number div {
  background-color: #b74;
  color: #fff;
  width: 2rem;
}
.detail-plans {
  display: flex;
  flex-direction: column;
  width: 65%;
  height: 100%;
}
.detail-plan1,
.detail-plan2 {
  display: flex;
  justify-content: space-between;
}
.detail-plan2 {
  border-top: 1px dotted #b74;
}
.detail-plan1 p,
.detail-plan2 p {
  padding: 0.5rem;
}
.btn-wrap {
  width: 10%;
}
</style>
