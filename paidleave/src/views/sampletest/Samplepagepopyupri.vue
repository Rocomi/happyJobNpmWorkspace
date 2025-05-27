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
                <strong style="font-size: 30px">권한 관리</strong>
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
                v-model="props.mainmenuid"
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
                v-model="props.submenuid"
              />
            </td>

            <th scope="row">Menu 명<span class="font_red">*</span></th>
            <td >
              <input
                type="text"
                class="form-control"
                style="width: 200px"
                name="sub_mnu_nm"
                id="sub_mnu_nm"
                v-model="props.submenunm"
              />
            </td>

          </tr>
          <tr>
            <th scope="row">권한<span class="font_red">*</span></th>
            <td colspan="3">
              관리자 
              <input
                type="checkbox"
                name="manage"
                id="manage"
                true-value="Y"
                false-value="N"
                v-model="checkinfo.manage"
              />
              일반 사용자
              <input
                type="checkbox"
                name="user"
                id="user"
                true-value="Y"
                false-value="N"
                v-model="checkinfo.user"
              />
            </td>
          </tr>          
        </tbody>
      </table>

      <!-- e : 여기에 내용입력 -->

      <div class="btn_areaC mt30">
        <a class="btn btn-primary" id="btnSaveGrpCod" name="btn" @click="save">
          <span>저장</span>
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

   const props = defineProps(['mainmenuid', 'menunm', 'submenuid', 'submenunm']) // 부모Component에서 받은값
   
   let checkinfo = ref({
      mainmenuid : props.mainmenuid,
      submenuid : props.submenuid,
      manage : "N",
      user : "N",
   });

   const save = () => {
      alert(checkinfo.value.manage);
      alert(checkinfo.value.user);

      let param = new URLSearchParams(Object.entries(checkinfo.value));

      axios.post("/menu/savepermit", param)
           .then((res) => {
                            console.log(res);
                            alert("저장되었습니다.")
                            closeModal();
                            
                          })
           .catch((err) => {
                console.err(err);
           })

   }

   onMounted(() => {
      const param = new URLSearchParams();
      param.append("mainmenuid", props.mainmenuid);
      param.append("submenuid", props.submenuid);

      axios.post("/menu/selectpermmit",param)
           .then((res) => {

                console.log(res);
                res.data.list.map((item) => {
                    if(item.user == "A") {
                      checkinfo.value.manage = item.permit;
                    }
                    if(item.user == "B") {
                      checkinfo.value.user = item.permit;
                    }
                })

                // [{ user : "A" , permit : "Y"},{user : "B" , permit : "N"}]

           }) 
           .catch((err) => {
                console.error(err)
           })  


   });

   const popupclose = () => {
      //props.returnfunc("N");    
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