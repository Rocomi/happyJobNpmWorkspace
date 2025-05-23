<template>
  <h1>{{msg}}</h1>
  <button :id="customid" @click="fn_click">버튼</button>
  <div id= "truediv" v-if="flag"> true </div>
  <div id= "falsediv" v-else> false </div>
  <div v-text=intext></div>
  <div v-html=inhtml></div>
  <input type="text" v-model="inputValue"/>
  <button @click="fn_confirm">버튼</button>{{ inputValue }}
  <div>
    <table>
      <tbody>
        
        <!-- 두 결과 다 동일함 -->
        <template v-for="(item, index) in arrVar" :key="index">
          <tr>
            <td>{{ item.id }}</td>
            <td>{{ item.name }}</td>
          </tr>
        </template>

        <!-- 두 결과 다 동일함 -->
          <tr v-for="(item, index) in arrVar" :key="index">
            <td>{{ item.id }}</td>
            <td>{{ item.name }}</td>
          </tr>
          
      </tbody>
    </table>
  </div>
  <div>
    {{ user.name }} <br> {{ user.age }}
  </div>
   <div>
    {{ fullname }}{{ lastname }}  <button @click="changefirst">버튼</button>
  </div>
</template>

<script setup>
import { ref, onMounted, destroyed, watch, computed } from 'vue';

 let msg = ref("안녕하세요");
 let flag = ref(true);
 let intext = ref("우리나라만세");
 let inhtml = ref("<input id='aaa' type='text' />");
 let customid = ref("customid");
 let inputValue = ref("initValue");
 let arrVar = ref([ {id:'1', name:'홍길동'}, {id:'2', name:'정길동'}, {id:'3', name:'김길동'}, {id:'4', name:'엄길동'} ])
  
 let firstname = ref("엄");
 let lastname = ref("희윤");

 let user = ref({
    name : "엄희윤",
    age : 30
 })

 const fn_click = (e) => {

    msg.value = "독도는 우리 땅";

    flag.value = !flag.value;

    if(flag.value) {
      intext.value = "우리나라만세";
      msg.value = "우리나라만세";

      inhtml.value = "<input id='aaa' type='text' />"
      customid.value = "customid";
    }else {
      intext.value = "독도는 우리땅";
      msg.value = "독도는 우리 땅";

      inhtml.value = "<select id='sel'><option value=''>전체</option><option value='1'>one</option><option value='2'>two</option></select>"
      customid.value = "false coustomid";
    }
  }
  //  alert(e.target.id);
  const fn_confirm = (e) => {
     alert(inputValue.value)
  }

  watch([flag, msg], ([flagnewValue, msgnewValue], [flagoldValue,msgoldValue]) => {
    console.log("newValue flag::" + flagnewValue, msgnewValue)
    console.log("oldValue flag::" + flagoldValue,msgoldValue)
  })

  watch(() => user, (newValue, oldValue) => {
    console.log("user newVlaue::", newValue);
    console.log("user newValue name ::", newValue.value.name);
    console.log("user oldValue::", oldValue.value.name);

  }, { deep: true });

// 캐싱 : Vue는 computed가 참조하는 값이 변경되지 않으면 이전 계산 결과를 재사용합니다.
// 예를들어 firstNmae이나 lastname이 변경되지 않는다면 fullname을 다시 계산하지 않습니다.
 const fullname = computed(() =>{
   return `${firstname.value}`;
 });

 const changefirst = () => {
  firstname.value = "염";
 }

  // {Immediate : true} 옵션을 주면 최초에 한번 실행됨

  // compute : function 처음부터 실행
  //           계산된 결과를 Cash 결과를 가지고 실행
  //           a = 3 , b = 5  a + b   8리턴
  //           a = 7로 변경하면     15 리턴

</script>