<template>
     <dl id="mainmenuWrap">
      <dd class="content"></dd>
      <!-- s : 여기에 내용입력 -->
      <table id="grpInfo" style="width: 450px">
        <caption>
          caption
        </caption>
        <colgroup>
          <col width="100px" />
          <col width="*" />
        </colgroup>

        <tbody>
          <tr>
            <td colspan="4" class="text-center">
              <div class="my-4">
                <strong style="font-size: 30px"> {{props.title}}</strong>
              </div>
            </td>
          </tr>          
          <tr>
            <th scope="row">Menu ID<span class="font_red">*</span></th>
            <td >
              <input
                type="text"
                class="form-control"
                style="width: 200px"
                name="mnu_id"
                id="mnu_id"
                ref="edit_mnu_id"
                :readonly="paction === 'U' ? true : false"
                v-model="edititem.mnu_id"
                @input="dupcheckfalse"
              />

            </td>
              <a class="btn btn-primary" id="btnidcheck" name="btnidcheck" v-show="checkflag" @click="menuiddupcheck">
                <span>check</span>
              </a>
          </tr>
          <tr>
            <th scope="row">Menu 명<span class="font_red">*</span></th>
            <td >
              <input
                type="text"
                class="form-control"
                name="mnu_nm"
                id="mnu_nm"
                v-model="edititem.mnu_nm"
              />
            </td>
          </tr>
          <tr>
            <th scope="row">사용 유무<span class="font_red">*</span></th>
            <td colspan="3">
              <select id="use_poa" name="use_poa" v-model="edititem.use_poa">
                 <option value="">선택</option>
                 <option value="Y">사용</option>
                 <option value="N">미사용</option>
              </select>
            </td>
          </tr>
        </tbody>
      </table>

      <!-- e : 여기에 내용입력 -->

      <div class="btn_areaC mt30">
        <a class="btn btn-primary" id="btnSaveGrpCod" name="btn" @click="save">
          <span>저장</span>
        </a>
        <a class="btn btn-danger mx-2" v-show="deleteyn && edititem.submenucnt === 0" @click="deletemainmenu">
          <span>삭제</span>
        </a>
        <a class="btn btn-info mx-2" @click="popupclose">
          <span>닫기</span>
        </a>
      </div>
    </dl>
</template>

<script setup>

   import { ref, defineProps, onMounted, getCurrentInstance} from "vue";
   import axios from "axios";
   import { closeModal } from "jenesius-vue-modal";

   const props = defineProps(['title', 'menucd','action','returnfunc']) // 부모Component에서 받은값

   const { appContext } = getCurrentInstance();
   const $emptyValidation = appContext.config.globalProperties.$emptyValidation;

   let edititem = ref({
       mnu_id : "",
       mnu_nm : "",
       use_poa : "",
       submenucnt : 0,
   });

   let paction = ref(props.action);

   let edit_mnu_id = ref();

   let deleteyn = ref(false);

   let dupcheckyn = ref(false);

   let checkflag = ref(false);

   const dupcheckfalse = () => {
     dupcheckyn.value= false;
   }

   const menuiddupcheck = () => {

     if(edititem.value.mnu_id === "") {
          alert("Mainmenu ID를 입력해 주세요.");
          return;
     }

     let param = new URLSearchParams() 
     param.append("mnu_id", edititem.value.mnu_id);

     axios.post("/menu/mainmenudupcheck",param)
          .then((res) => {
               console.log(res.data);
               alert(res.data.resultMsg);

               if(res.data.result === "Y") {
                    dupcheckyn.value = true;
               }
          })
          .catch((error) => {
               console.log(error);
          });


   }

   const deletemainmenu = () => {
     paction.value = "D";
     save();
   }
  
   const save = () => {

      if(paction.value == "I") {
        if(!dupcheckyn.value) {
          alert("Main Menu ID 중복 체크 해주세요.");
          return;
        }
      }

      let editcheckparam = [
          {checkval : edititem.value.mnu_id, checkmsg : "Main Menu ID를 입력해 주세요"},
          {checkval : edititem.value.mnu_nm, checkmsg : "Main Menu 명를 입력해 주세요"},
          {checkval : edititem.value.use_poa, checkmsg : "사용 유무를 선택해 주세요"},
     ];

     let nullcheck = $emptyValidation(editcheckparam);
 
     // console.log(nullcheck);

     if(!nullcheck) return;
      let param = new URLSearchParams(Object.entries(edititem.value));
      param.append("menucd",props.menucd);  
      param.append("action",paction.value);  

      axios.post("/menu/mainmenusave",param)
      .then((res)=> {
          console.log(res);

          alert(res.data.resultMsg);
   
          props.returnfunc("Y");
          closeModal();
      })
      .catch((error) => {
          console.log(error);
      });      
      

   };

   const popupclose = () => {
     props.returnfunc("N");
     closeModal();
   }

   const searchmainmenudetail = () => {
      let param = new URLSearchParams();
      param.append("mnu_id",props.menucd);  

      axios.post("/menu/mainmenudetail",param)
      .then((res)=> {
          console.log(res);

          edititem.value = {...edititem.value, ...res.data.mainmenuinfo};
      })
      .catch((error) => {
          console.log(error);
      });      


   }

   onMounted(() => {

     console.log("onMounted ", paction.value);

     if(paction.value === "I") { 
          edit_mnu_id.value.focus();
          deleteyn.value = false;  
          checkflag.value = true;

     } else {

          searchmainmenudetail();
     
          deleteyn.value = true; 
          checkflag.value = false;
          edit_mnu_id.value.focus();
     }
   });


</script>

<style>
#grpInfo {
  border-collapse: separate;
  border-spacing: 20px;
}
#mainmenuWrap {
  background-color: #fff;
  padding: 3rem;
  border-radius: 10px;
  border: 2px solid rgb(59, 59, 59);
}
</style>