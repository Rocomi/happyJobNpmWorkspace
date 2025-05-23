<template>
     <dl id="submenuWrap">
      <dd class="content"></dd>
      <!-- s : 여기에 내용입력 -->
      <table id="grpInfo" style="width: 850px">
        <caption>
          caption
        </caption>
        <colgroup>
          <col width="150px" />
          <col width="*" />
          <col width="150px" />
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
            <th scope="row">Main Menu ID<span class="font_red">*</span></th>
            <td >
              <input
                type="text"
                class="form-control"
                style="width: 200px"
                name="mnu_id"
                id="mnu_id"
                v-model="props.menucd"
                readonly
              />
            </td>
            <th scope="row">Main Menu 명<span class="font_red">*</span></th>
            <td >
              <input
                type="text"
                class="form-control"
                style="width: 200px"
                name="mnu_nm"
                id="mnu_nm"
                v-model="props.menunm"
                readonly
              />
            </td>
          </tr>  
          <tr>
            <th scope="row">Menu ID<span class="font_red">*</span></th>
            <td >
              <input
                type="text"
                class="form-control"
                style="width: 200px"
                name="sub_mnu_id"
                ref="subnebutag"
                id="sub_mnu_id"
                :readonly="paction === 'U' ? true : false"
                v-model="submenuotem.sub_mnu_id"
              />
              <a class="btn btn-primary" id="btnidcheck" name="btnidcheck" @click="submenuidcheck" v-if="props.action === 'I'">
                <span>check</span>
              </a>
            </td>

            <th scope="row">Menu 명<span class="font_red">*</span></th>
            <td >
              <input
                type="text"
                class="form-control"
                style="width: 200px"
                name="sub_mnu_nm"
                id="sub_mnu_nm"
                v-model="submenuotem.sub_mnu_nm"
              />
            </td>

          </tr>
          <tr>
            <th scope="row">URL<span class="font_red">*</span></th>
            <td colspan="3">
              <input
                type="text"
                class="form-control"
                name="url"
                id="url"
                v-model="submenuotem.url"
              />
            </td>
          </tr>
          <tr>
            <th scope="row">사용 유무<span class="font_red">*</span></th>
            <td colspan="3">
              <select id="use_poa" name="use_poa" v-model="submenuotem.use_poa">
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
        <a class="btn btn-danger mx-2" v-show="flagconfig.deleteflaf" @click="submenudelete">
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

   const props = defineProps(['title', 'menucd', 'menunm', 'submenucd', 'action','returnfunc']) // 부모Component에서 받은값

   let paction = ref(props.action);

   const flagconfig = ref({
      menuidcheck :false,
      deleteflag : false,

   });

   const subnebutag = ref();

   const submenuotem = ref({
         sub_mnu_id : "",
         sub_mnu_nm : "",
         url : "",
         use_poa : "",
   });

   const submenuidcheck = () => {
      const param = new URLSearchParams();
      param.append("mnu_id", submenuotem.value.sub_mnu_id);

      axios.post("/menu/submenuidcheck",param)
           .then((res) => {
               console.log(res.data);

               alert(res.data.resultMsg);

               if(res.data.result === "Y") {
                 flagconfig.value.menuidcheck = true;               
               }
           })
           .catch((err) => {
               console.log(err);
           })

   }

   const submenuselect = () => {

      const param = new URLSearchParams();
      param.append("mnu_id", submenuotem.value.sub_mnu_id);

      axios.post("/menu/submenuselect",param)
           .then((res) => {
               console.log(res.data);
               submenuotem.value.sub_mnu_nm = res.data.submenuinfo.mnu_nm;
               submenuotem.value.url = res.data.submenuinfo.mnu_url;
               submenuotem.value.use_poa = res.data.submenuinfo.use_poa;
              
           })
           .catch((err) => {
               console.log(err);
           })
   }

   const submenudelete = () => {
      console.log("삭제");

       paction.value = "D";
       save();

   }
    
   const save = () => {

      const param = new URLSearchParams(Object.entries(submenuotem.value));
      param.append("action", paction.value);
      param.append("mnu_id", props.menucd);

      axios.post("/menu/submenusave",param)
           .then((res) => {
               console.log(res.data);

               alert(res.data.resultMsg);

               if(res.data.result === "Y") {
                  props.returnfunc("Y");
                  closeModal();
               } else {
                  alert(res.data.resultMsg);
               }
           })
           .catch((err) => {
               console.log(err);
           })

   }

   onMounted(() => {

    subnebutag.value.focus();

    if(props.action === "I") {
      flagconfig.value.deleteflaf = false;
    } else {
      flagconfig.value.deleteflaf = true;

      submenuotem.value.sub_mnu_id = props.submenucd;

      submenuselect();

    }
     //console.log(props.title);
     //console.log(props.menunm);
     //console.log(props.action);
     

   });

   const popupclose = () => {
      props.returnfunc("N");    
      closeModal();

   }

</script>

<style>
#grpInfo {
  border-collapse: separate;
  border-spacing: 20px;
}
#submenuWrap {
  background-color: #fff;
  padding: 3rem;
  border-radius: 10px;
  border: 2px solid rgb(59, 59, 59);
}
</style>