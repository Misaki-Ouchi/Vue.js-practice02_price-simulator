<script setup lang="ts">
import { ref, reactive } from "vue";

defineProps<{
  isSelected: boolean;
  isDeselected: boolean;
  msgType1: boolean;
  msgType2: boolean;
  isRecommend: boolean;
  isSale: boolean;
  selectedData: string;
}>();
defineEmits(['btnClick', 'selected'])

const btnClick = (e) => {
  console.log("a");
  console.log(e.target.dataset.selected);
  let selected = e.target.dataset.selected
};
// button選択で.selected .deselected付与
</script>

<template>
  <button
    :class="{ selected: isSelected, deselected: isDeselected }"
    @click="btnClick"
    :data-selected="selectedData"
  >
    <div class="button-wrap">
      <!-- アイコン -->
      <i>
        <slot name="icon"></slot>
      </i>
      <!-- 中身 1行Ver.-->
      <p class="details" v-if="msgType1">
        <span class="detail selected"><slot name="detail"></slot></span>
        <span class="detail-sub"><slot name="detail-sub"></slot></span>
      </p>
      <!-- 中身 2行Ver.-->
      <p class="details" v-if="msgType2">
        <span class="detail selected"
          ><b><slot name="detail"></slot></b><br
        /></span>
        <span class="detail-sub"><slot name="detail-sub"></slot></span>
      </p>
      <!-- オススメ -->
      <div class="recommend" v-if="isRecommend">
        <div class="recommend-wrap">
          <slot name="recommend">オススメ！</slot>
        </div>
      </div>
      <!-- セール -->
      <div class="sale" v-if="isSale">
        <div class="sale-wrap">
          <p>セール</p>
        </div>
      </div>
      <!-- 価格 -->
      <div class="price" v-if="isSale">
        <p class="prePrice"><slot name="prePrice"></slot></p>
        <p class="salePrice"><slot name="salePrice"></slot></p>
      </div>
    </div>
  </button>
</template>

<style scoped>
b {
  font-weight: bold;
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
button.deselected i {
  background-color: #bbb;
}
.button-wrap {
  display: flex;
  justify-content: space-between;
  align-items: center;
  text-align: left;
  padding: 1rem;
}
.details {
  margin-right: auto;
  margin-left: 1rem;
}
.detail {
  flex: 1;
  font-size: 1rem;
}
.detail-sub {
  font-size: 0.9rem;
}
i {
  width: 3rem;
  height: 3rem;
  background-color: #faa;
}
.recommend,
.sale {
  position: absolute;
  top: -0.1rem;
  right: -0.1rem;
  color: #fff;
}
.recommend {
  background-color: rgb(255, 50, 180);
  border-radius: 0 1rem 0 0;
}
.sale {
  right: 1.7rem;
}
.sale-wrap {
  background-color: rgba(255, 50, 50);
  right: 1rem;
  padding-left: 0.3rem;
}
.recommend-wrap,
.sale-wrap {
  font-weight: bold;
  padding-right: 0.3rem;
}
.sale-wrap > p {
  font-weight: bold;
}
.recommend-wrap {
  position: relative;
}
.recommend-wrap::before {
  content: "";
  position: absolute;
  top: calc(-1rem / 1.15);
  left: calc(-1rem / 1.15);
  border: calc(1rem / 1.14) solid transparent;
  border-left: calc(1rem / 1.14) solid rgb(255, 50, 180);
  transform: rotate(-45deg);
}
.price {
  text-align: right;
}
.prePrice {
  color: #333;
  font-size: 0.75rem;
  text-decoration: line-through;
}
.salePrice {
  color: rgb(255, 50, 180);
  font-size: 0.9rem;
  font-weight: bold;
}
</style>
