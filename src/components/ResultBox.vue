<script setup lang="ts">
import { ref, reactive } from "vue";

defineProps<{
  showTelPlan: boolean;
  isDisOpenBtn: boolean;
}>();
defineEmits(["resetClick", "openBtnClick"]);
</script>
<template>
  <div id="result" class="result">
    <div class="result-wrap">
      <h2>診断結果</h2>
      <div class="result-box">
        <div class="campaigns">
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
        </div>
        <!-- /.campaigns -->
        <button
          @click="$emit('openBtnClick')"
          class="openBtn"
          :disabled="isDisOpenBtn"
        >
          内訳
        </button>
        <div class="resultDetail">
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
            <button @click="$emit('resetClick')" class="resetBtn">×</button>
          </div>
        </div>
        <!-- /.resultDetail -->
      </div>
      <!-- /.result-box -->
    </div>
  </div>
</template>
<style scoped>
.result {
  min-width:500px;
}
.result-wrap {
  width: 90%;
  margin: 2rem auto;
  background-color: #b74;
  padding: 1rem;
  text-align: center;
  height: fit-content;
  overflow: hidden;
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
  transition: .4s;
}
.campaigns {
  position: relative;
  padding-right: 20%;
  background-color: #ffe;
  z-index: 2;
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
  top: 3rem;
  right: 1rem;
  padding-bottom: 1rem;
  z-index: 2;
}
.openBtn::after {
  content: "";
  position: absolute;
  bottom: -0.2rem;
  right: 0;
  display: inline-block;
  width: 0.8rem;
  height: 0.8rem;
  border-top: 1px solid #333;
  border-left: 1px solid #333;
  transform: rotate(-135deg) translate(1rem);
  transition: transform 0.4s, bottom 0.4s;
}
.openBtn.opened::after {
  transform: rotate(45deg) translate(-1rem);
  bottom: -0.8rem;
}
.openBtn[disabled]::after {
  opacity: 0.3;
}
.resultDetail {
  display: flex;
  position: relative;
  justify-content: space-between;
  align-items: center;
  background-color: #fff;
  border-top: 1px solid #b74;
  border-bottom: 1px solid #b74;
  z-index: -1;
  transition: 0.4s;
}
.resultDetail.opened {
  z-index: 1;
  height: fit-content;
  transition: 0.4s;
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
