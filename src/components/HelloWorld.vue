<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <h2>Essential Links</h2>
     <my-calendar class="my-container" :caldate="dayArray" :defaultForm="emptyObj">
        <template slot="content" slot-scope="list">
        
          <div v-for="(el,index) in list.datas" :key="index" class="calendar-item" >
            <div v-if="index>list.firstIndex-1&&index<list.firstIndex+list.maxDate" class="calendar-item-content">
              {{el.datestring}}
                <el-form label-width="0px">
                  <el-form-item>
                   <el-input v-model="el.form.price1" placeholder="单人房价格"></el-input> 
                  </el-form-item>
                  <el-form-item>
                   <el-input v-model="el.form.price2" placeholder="双人房价格"></el-input> 
                  </el-form-item>
                  <el-form-item>
                   <el-input v-model="el.form.numbers" placeholder="剩余数量"></el-input> 
                  </el-form-item>
                  <el-form-item>
                    <el-button-group>
                    <el-button type="primary" @click="save(el)">保存</el-button>
                    <el-button @click="fill(el)">填充</el-button>
                    </el-button-group>
                  </el-form-item> 
                </el-form>
              
            </div>            
          </div>
       </template>          
    </my-calendar> 
  </div>
</template>

<script>
import Calendar from './Calendar';
import moment from 'moment';
export default {
  name: 'HelloWorld',
  components:{
    'myCalendar':Calendar,
  } , 
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      modelObj:{
        price1:1200,
        price2:2000,
        numbers:10,
      },
      emptyObj:{
        price1:null,
        price2:null,
        numbers:null,
      },
      dayArray:[{
        id:1,
        availableDay:'2018-07-02 00:00:00',
        price1:1200,
        price2:2000,
        numbers:10,
      },{
        id:2,
        availableDay:'2018-07-05 00:00:00',
        price1:1200,
        price2:2000,
        numbers:10,
      }],
    }
  },
  mounted(){
    console.log(this.list)
  },
  methods:{
    matchDate(date1){
      let matchedArr=this.dayArray.filter(item=>{
        //console.log(date1,item.availableDay,moment(date1).isSame(item.availableDay,'date'))
        return moment(date1).isSame(item.availableDay,'day');
      }
         
      )
      
      if(matchedArr.length>0){
        console.log(matchedArr)
        return matchedArr[0]
      }else{
        return JSON.parse(JSON.stringify(this.emptyObj));
      }
    },
    save(obj){
      console.log(obj)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.my-container{
  width:100%;
  overflow: hidden;
  padding-left:1%;
}
.my-container .calendar-item{
  width:14%;
  float:left;
  box-sizing: border-box;
  padding:5px;
  

}
.calendar-item-content{
border:1px solid #ddd;
padding:2px;
}
</style>
