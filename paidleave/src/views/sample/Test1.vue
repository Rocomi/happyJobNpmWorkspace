<template>
  <div class="container">
    <h1>Use Memo</h1>
    <h2>{{ msg }}</h2>

    <div class="input-group">
      <input 
        type="text" 
        v-model="inputValue" 
        @keyup.enter="fn_confirm"
        placeholder="입력하고 엔터 또는 버튼 클릭!"
      />
      <button @click="fn_confirm">🌤 확인</button>
    </div>

    <h4>Title history</h4>
    <div class="history" v-html="inhtml"></div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

let msg = ref("이게 뭐노진짜");
let inputValue = ref("");
let inhtml = ref("");

const fn_confirm = () => {
  if (!inputValue.value.trim()) return; // 공백 방지
  msg.value = inputValue.value;
  inhtml.value += "<div>" + inputValue.value + "</div>";
  inputValue.value = "";
};
</script>

<style scoped>
.container {
  margin: 40px auto;
  padding: 30px;
  background: #ddffff93;
  border-radius: 20px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
  text-align: center;
  font-family: 'Segoe UI', sans-serif;
}

h1 {
  color: #0300aa;
  margin-bottom: 20px;
}

.input-group {
  display: flex;
  gap: 10px;
  justify-content: center;
  margin-bottom: 20px;
}

input {
  padding: 10px 15px;
  border: 2px solid #90caf9;
  border-radius: 12px;
  outline: none;
  font-size: 16px;
  background-color: #ffffff;
  transition: 0.2s;
  width: 60%;
}

input:focus {
  border-color: #42a5f5;
  box-shadow: 0 0 5px #90caf9;
}

button {
  padding: 10px 15px;
  background-color: #81d4fa;
  border: none;
  border-radius: 12px;
  color: #0d47a1;
  font-size: 16px;
  cursor: pointer;
  transition: 0.3s;
}

button:hover {
  background-color: #4fc3f7;
  transform: scale(1.05);
}

.history {
  background-color: #ffffffaa;
  padding: 10px;
  border-radius: 10px;
  text-align: center;
  font-size: 15px;
  max-height: 200px;
  overflow-y: auto;
  color: #000000;
}
</style>
