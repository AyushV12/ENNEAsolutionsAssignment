<template>
  <div class="hello">
    <div class="header">
    <div class="card shadow buttonContainer">
      <b-button v-on:click="getList" class="button">
        Get Admin List
      </b-button>

    </div>
    </div>
    <div class="card shadow tableContainer">
      <b-input v-model="filterString" class="filterString mt-2 mb-2" card shadow placeholder="Search by name,email or role" >

      </b-input>
      
      <b-table :items="result" striped hover :fields="fields" :filter="filterString" id="table" :current-page="pageNo"
    :per-page="pagesize" @filtered="searchChangeHandler" selectable>
        <template #head(select)>
          <div class="checkBoxContainer">
            <b-form-checkbox
            id="checkboxHeader"
            v-model="statusCheckAllRows"
            v-on:change="selectAllRows"
          >

          </b-form-checkbox>
          </div>

        </template>
        <template #cell(select)="data">
          <div class="checkBoxContainerRows">
            <b-form-checkbox
            id="checkboxRows"
            v-model="statusSelectOne"
            v-on:change="selectDeselectRows(data.item.id)"
          >
   
          </b-form-checkbox>
          </div>

        </template>
        <template #cell(action)>
          <div class="edit&deleteContainer">
            <b-button class="editButtonRow">
              <b-icon icon="pencil-square" scale="1" variant="info"></b-icon>
            </b-button>
            <b-button class="deleteButtonRow ">
              <b-icon icon="trash" scale="1" variant="danger"></b-icon>


            </b-button>
          </div>
        </template>
      </b-table>
      <div class="paginationContainer">
          <b-button class="buttonDeleteAll" variant="danger"> Delete All Selected</b-button>
          <b-pagination class="pagination" v-model="pageNo" :total-rows="resultLength" :per-page="pagesize"
          aria-controls="table" v-on:change="delselectHeader" ></b-pagination >
 

      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import { BootstrapVue, IconsPlugin } from 'bootstrap-vue'

// Import Bootstrap and BootstrapVue CSS files (order is important)
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

// Make BootstrapVue available throughout your project
Vue.use(BootstrapVue)

import { BootstrapVueIcons } from 'bootstrap-vue'
import 'bootstrap-vue/dist/bootstrap-vue-icons.min.css'

Vue.use(BootstrapVueIcons)

// Optionally install the BootstrapVue icon components plugin
Vue.use(IconsPlugin)
import axios from "axios"
// import { filter } from "vue/types/umd"
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
  ,
  data(){
    return(
      {
        result:[],
        filterString:"",
        fields:["select","id","name","email","role","action"],
        pageNo:1,
        pagesize:10,
        resultLength:0,
        statusCheckAllRows:false,
        statusCheckRows:{},
        result1:[]
        

        
      }
    )
  },
  methods:{

    async getList(){
      const res= await axios.get("https://geektrust.s3-ap-southeast-1.amazonaws.com/adminui-problem/members.json")
      console.log(res)
      this.result=res.data
      this.resultLength=this.result.length
      this.result1=this.result
    },
    delselectHeader(){
      this.statusCheckAllRows=false
      this.selectAllRows()
    },
    searchChangeHandler(array,lengthOfArray){
      // var bTable=document.getElementById("table")
      this.result1=array
      this.resultLength=lengthOfArray

      // console.log(this.result,this.result.length)
    },
    selectAllRows(){
      // const page=this.pageNo
      // const page_size=this.pagesize
      // const array=this.result1
      // console.log("array=",this.result1)
      // console.log(`page=${page} & page_size=${page_size}`)
      // count=0
      
      // if(this.statusCheckAllRows===true){
      //   console.log("true")

      // }
      // else{
      //   console.log("false")
      // 

      let i=0
      const checkValue=this.statusCheckAllRows
      const rowsDisplayed=document.getElementsByTagName("tr")
      function check(e){
        e.checked=true
      }
      function deCheck(e){
        e.checked=false
      }
      for (i=1;i<rowsDisplayed.length;i++){
        console.log(rowsDisplayed[i])
        if(checkValue===true){
        rowsDisplayed[i].classList="b-table-row-selected table-active"
        }
        else{
          rowsDisplayed[i].classList=""
        }
        // console.log(rowsDisplayed[i].children[0].children[0].children[0].children[0])
        const tempCheckBox=rowsDisplayed[i].children[0].children[0].children[0].children[0]
        console.log(tempCheckBox)

        tempCheckBox.addEventListener("click",checkValue===true?check(tempCheckBox):deCheck(tempCheckBox))
    }}
    
    ,
    selectDeselectRows(id){
      const valueCheck = this.statusSelectOne

      const indexStart = this.pageNo * 10 - 10
      let row = 1

      for (let i = indexStart; i < indexStart + 10; i++) {
        if (this.result1[i].id === id) {

          break
        }
        row += 1
      }
      const rowsDisplayed = document.getElementsByTagName("tr")
      if (valueCheck === true) {

        rowsDisplayed[row].classList = "b-table-row-selected table-active"
      }
      else {
        rowsDisplayed[row].classList = ""
      }
    
  },



  },
  
  
  mounted(){
    // this.getList()  // after implementation
  }
}

</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.buttonContainer{
  height: 100px;
  width: 50%;
  text-align: center;
  margin: auto;
}
.header{
  text-align: center;
}
.button{
  width: 25%;
  margin: auto;
}
.tableContainer{
  padding: 15px;
  width: 80%;
  margin: auto;
}
.editButtonRow,.deleteButtonRow
{background-color: white;
  border: none;
  padding: none;
  text-align: center;

}
.buttonDeleteAll{
  margin-right: auto;
  border-radius: 50px;
}
.pagination{
  margin: auto;
}
.paginationContainer{
  display: flex;
  flex-direction: row;
}
</style>
