<template>
    <div id="comnCodMgr">
    <p class="conTitle">
      <span>Main Menu</span>
      <span>
        <table
          style="border: 1px #50bcdf"
          width="100%"
          cellpadding="5"
          cellspacing="0"
          border="1"
          align="left"
        >
          <tr style="border: 0px; border-color: blue">
            <td
              width="50"
              height="25"
              style="font-size: 100%; text-align: left"
            >
              <div id="searchArea" class="d-flex justify-content-around">
                

                메뉴 ID
                <input
                  type="text"
                  id="smenuid"
                  name="smenuid"
                  style="width: 10%"
                  class="form-control"
                  v-model="searcharea.smenuid"
                />
                메뉴명
                <input
                  type="text"
                  id="smenuname"
                  name="smenuname"
                  style="width: 15%"
                  class="form-control"
                  v-model="searcharea.smenuname"
                />
                사용 유무
                <select
                  id="susetn"
                  name="susetn"
                  style="width: 10%"
                  v-model="searcharea.suseyn"
                >
                  <option value="">전체</option>
                  <option value="Y">사용</option>
                  <option value="N">미사용</option>
                </select>

                <span class="fr">
                  <a
                    class="btn btn-primary mx-2"
                    id="searchGrpcod"
                    name="btnSearch"
                    @click="mainsearch"
                  >
                    <span>검 색</span>
                  </a>
                  <a class="btn btn-primary mx-2" name="btnNew" @click="newregmain">
                    <span>신규</span>
                  </a>
                  
                </span>
              </div>
            </td>
          </tr>
        </table>
      </span>
    </p>

    <div class="divsubmenu">
      <table class="col">
        <caption>
          caption
        </caption>
        <colgroup>
          <col width="20%" />
          <col width="40%" />
          <col width="20%" />
          <col width="20%" />
        </colgroup>

        <thead>
          <tr>
            <th scope="col">Menu ID</th>
            <th scope="col">Menu 명</th>
            <th scope="col">사용 유무</th>
            <th scope="col">수정</th>
          </tr>
        </thead>
        <tbody>
            <template v-if="mainlist.totalcnt === 0"> 
              <tr >
                 <td colspan="3">조회된 데이터가 없습니다.</td>
              </tr>    
            </template>        
            <template v-else>
              <tr v-for="(item, index) in mainlist.listdata" :key="index">
                 <td>{{item.mnu_id}}</td>
                 <td @click="submenusearch(item.mnu_id,item.mnu_nm)">{{item.mnu_nm}}</td>
                 <td>{{item.use_poa}}</td>
                 <td><a
                    class="btn btn-primary mx-2"
                    id="searchGrpcod"
                    name="btnSearch"
                    @click="mainmenumodify(item.mnu_id)"
                  >
                    <span>수정</span>
                  </a></td>
              </tr>  
            </template>  
        </tbody>
      </table>
    </div>    
    <div id="mainPagination">
      <paginate
        class="justify-content-center"
        v-model="searcharea.currentpage"
        :page-count="mainlist.totalcnt % searcharea.pagesize == 0 ? mainlist.totalcnt / searcharea.pagesize : Math.floor(mainlist.totalcnt / searcharea.pagesize) + 1"
        :page-range="5"
        :margin-pages="0"
        :click-handler="mainsearch"
        :prev-text="'이전'"
        :next-text="'다음'"
        :container-class="'pagination'"
        :page-class="'page-item'"
      >
      </paginate>
    </div>
   <p class="conTitle">
      <span> Sub Menu</span>
      <span>
        <table
          style="border: 1px #50bcdf"
          width="100%"
          cellpadding="5"
          cellspacing="0"
          border="1"
          align="left"
        >
          <tr style="border: 0px; border-color: blue">
            <td
              width="50"
              height="25"
              style="font-size: 100%; text-align: left"
            >
              <div id="searchArea" class="d-flex justify-content-around">
                
  
                <span class="fr">
                  
                  <a class="btn btn-primary mx-2" name="btnNew" @click="newsubreg">
                    <span>신규</span>
                  </a>
                  
                </span>
              </div>
            </td>
          </tr>
        </table>
      </span>
    </p>


    <div class="divsubmenu">
      <table class="col">
        <caption>
          caption
        </caption>
        <colgroup>
          <col width="20%" />
          <col width="20%" />
          <col width="20%" />
          <col width="20%" />
          <col width="20%" />
        </colgroup>

        <thead>
          <tr>
            <th scope="col">Menu ID</th>
            <th scope="col">Menu 명</th>
            <th scope="col">상위 Menu ID</th>
            <th scope="col">상위 Menu 명</th>
            <th scope="col">사용 유무</th>
          </tr>
        </thead>
        <tbody>
              <tr v-if="sublist.totalcnt === 0">
                 <td colspan="5">조회된 데이터가 없습니다.</td>
              </tr>   
              <template v-else>
                <tr v-for="(item, index) in sublist.listdata" :key="index" @click="submenumodify(item.mnu_id)">
                   <td>{{item.mnu_id}}</td>
                   <td>{{item.mnu_nm}}</td>
                   <td>{{item.hir_mnu_id}}</td>
                   <td>{{item.hir_mnu_nm}}</td>
                   <td>{{item.use_poa}}</td>
                </tr>   
              </template>
        </tbody>
      </table>
    </div>    
    <div id="subPagination">
      <paginate
        class="justify-content-center"
        v-model="subsearcharea.currentpage"
        :page-count="sublist.totalcnt % subsearcharea.pagesize == 0 ? sublist.totalcnt / subsearcharea.pagesize : Math.floor(sublist.totalcnt / subsearcharea.pagesize) + 1"
        :page-range="5"
        :margin-pages="0"
        :click-handler="submenunavi"
        :prev-text="'이전'"
        :next-text="'다음'"
        :container-class="'pagination'"
        :page-class="'page-item'"
      >
      </paginate>
    </div>
  </div>
</template>

<script setup>

   import { onMounted, ref } from "vue";
   import axios from "axios";
   import Paginate from "vuejs-paginate-next";
   import { openModal } from "jenesius-vue-modal";
   import Samplepagepopypmain from "./Samplepagepopypmain.vue" 
   import Samplepagepopypsub from "./Samplepagepopypsub.vue"

   let searcharea = ref({
         smenuid : "",
         smenuname : "",
         suseyn: "",
         currentpage : 1,
         pagesize : 5,
         level : 0});

   let mainlist = ref({
      listdata : [],
      totalcnt : 0,
   });     

   let popupsaveyn = ref("N");
   let subpopupsaveyn = ref("N");
   
   let subsearcharea = ref({
         smenuid : "",
         smenunm : "",
         currentpage : 1,
         pagesize : 5,
         level : 1});

   let sublist = ref({
      listdata : [],
      totalcnt : 0,
   });   

   const mainsearch = async () => {
       const param = new URLSearchParams(Object.entries(searcharea.value));
    
       await axios.post("/menu/mainmenusearch",param)
                  .then((res) => {
                            console.log(res.data);
                            mainlist.value.listdata = res.data.mainmenulist;
                            mainlist.value.totalcnt = res.data.totalcnt;
                      })
                  .catch((err) => {
                            console.log(err);
         });
   }

  const popupreturn = (returnval) => {
       popupsaveyn.value = returnval;
  }

  const subpopupreturn = (returnval) => {
       subpopupsaveyn.value = returnval;
  } 

  let popupparam = ref({
    title: "",
    menucd : "",
    action : "",
    returnfunc : popupreturn,
  })
  
  let popupparamsub = ref({
    title: "",
    menucd : "",
    menunm : "",
    submenucd : "",
    action : "",
    returnfunc : subpopupreturn,
  })
  
   
  const newregmain = async () => {
    popupparam.value.title = "Main Menu 등록";
    popupparam.value.menucd = "";
    popupparam.value.action = "I";

    const popupvar = await openModal(Samplepagepopypmain, popupparam.value);

    popupvar.onclose = () => {

      if (popupsaveyn.value === "Y") {
        searcharea.value.currentpage = 1;
        mainsearch();
      }
    };

  }

  const mainmenumodify = async (mainmenucd) => {
    popupparam.value.title = "Main Menu 수정";
    popupparam.value.menucd = mainmenucd;
    popupparam.value.action = "U"; 
    
    const popupvar = await openModal(Samplepagepopypmain, popupparam.value);

    popupvar.onclose = () => {

      if (popupsaveyn.value === "Y") {
        searcharea.value.currentpage = 1;
        mainsearch();
      }
    };

  }



  const submenunavi = () => {
     submenusearch(subsearcharea.value.smenuid, subsearcharea.value.smenunm);
  }

  const submenusearch = async (mainmenucd, mainmenunm) => {

       subsearcharea.value.smenuid = mainmenucd;
       subsearcharea.value.smenunm = mainmenunm;

       const param = new URLSearchParams(Object.entries(subsearcharea.value));
    
       await axios.post("/menu/submenusearch",param)
                  .then((res) => {
                            console.log(res.data);
                            sublist.value.listdata = res.data.submenulist;
                            sublist.value.totalcnt = res.data.totalcnt;
                      })
                  .catch((err) => {
                            console.log(err);
         });
  }


  const newsubreg = async () => {

    if(subsearcharea.value.smenuid === "") {
      alert("상위 메뉴를 선택해 주세요.");
      return;
    }

    popupparamsub.value.title = "Sub Menu 등록";
    popupparamsub.value.menucd = subsearcharea.value.smenuid;
    popupparamsub.value.menunm = subsearcharea.value.smenunm;
    popupparamsub.value.submenucd = "";
    popupparamsub.value.action = "I";

    const popupvar = await openModal(Samplepagepopypsub, popupparamsub.value);

    popupvar.onclose = () => {
      if (subpopupsaveyn.value === "Y") {
        subsearcharea.value.currentpage = 1;
        submenusearch(subsearcharea.value.smenuid);
      }
    };


  }

  const submenumodify = async (submenu) => {

    popupparamsub.value.title = "Sub Menu 수정";
    popupparamsub.value.menucd = subsearcharea.value.smenuid;
    popupparamsub.value.menunm = subsearcharea.value.smenunm;
    popupparamsub.value.submenucd = submenu;
    popupparamsub.value.action = "U";

    const popupvar = await openModal(Samplepagepopypsub, popupparamsub.value);

    popupvar.onclose = () => {
      if (subpopupsaveyn.value === "Y") {
        subsearcharea.value.currentpage = 1;
        submenusearch(subsearcharea.value.smenuid);
      }
    };    
  }

   onMounted(() => {
       mainsearch();
   })

</script>