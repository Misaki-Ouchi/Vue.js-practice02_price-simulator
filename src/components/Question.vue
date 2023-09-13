<template>
  <div class="question">
    <span>Q</span>
    <p>
      <slot></slot>
    </p>
  </div>
</template>

<style scoped>
.question {
  display: flex;
  padding: 0.5rem 0;
}
span {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 2rem;
  height: 2rem;
  font-size: 1.3rem;
  line-height: 1;
  color: #fff;
  background: rgb(255, 50, 180);
  margin-right: 0.5rem;
}
p {
  font-weight: bold;
  font-size: 1.1rem;
  line-height: 2;
}
</style>
