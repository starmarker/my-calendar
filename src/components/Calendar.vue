<template>
    <div class="calendar-container">
        <el-row class="calendar-header">
            <el-col :span="2">
                <i class="el-icon-arrow-left" @click="changeMonth('prev')"></i>
            </el-col>
            <el-col :span="20">
                {{currentYear}}年<br>
                <small>{{currentMonth+1}}月</small>        
            </el-col>
            <el-col :span="2">
                <i class="el-icon-arrow-right" @click="changeMonth('next')"></i>
            </el-col>
        </el-row>
        <ul class="calendar-week">
            <li v-for="el in weekText" :key="el">{{el}}</li>
        </ul>
        <div class="calendar-week">
            <slot name="content" :datas="days" :firstIndex="firstIndex" :maxDate="monthDays">

            </slot>

                
        </div>               
    </div>
</template>
<script>
import moment from 'moment';
import CalendarContent from './CalendarContent';
moment.locales('zh-CN');
export default {
    name:"calendar",
    data(){
        return {
            currentDate:'1970-01-01',
            days:[],
            weekText:['日',"一","二","三","四","五","六"],
            firstIndex:0,
            lastDay:0,
            monthDays:0,
        }
    },
    props:['caldate','defaultForm'],
    computed:{
        currentYear(){
            return moment(this.currentDate).year();
        },
        currentMonth(){
            return moment(this.currentDate).month();
        }
    },
    created(){
        this.initDate();
        this.createDate()
    },
    methods:{
        initDate(){
            let cur_date=new Date();
            this.currentDate=cur_date;
        },
        changeMonth(change){
            let month_cal=0;
            
            if(change=='prev'){
                console.log(change)
            month_cal= this.currentMonth - 1;
            }else{
            month_cal=this.currentMonth+1 ;
            }
            
            this.currentDate=moment().set({'year': this.currentYear, 'month': month_cal,'date':1}).format('YYYY-MM-DD')
            console.log(this.currentDate)
            this.$emit('monthChange',this.currentDate)
            this.createDate()
        },
        createDate(){
            const dates = []
            const lastDay=moment(this.currentDate).daysInMonth()
            this.lastDay=moment(this.currentDate).date(lastDay).format('YYYY-MM-DD');
            this.monthDays=lastDay;
            for(let i = 0; i < 42; i+=1) {
            const startDate = moment(this.currentDate).date(1);
            this.firstIndex=startDate.days();   
                dates[i]={};         
                dates[i].datestring = startDate.weekday(i).format('YYYY-MM-DD'); 
                
                let matchedArr=this.caldate.filter(item=>{
                //console.log(date1,item.availableDay,moment(date1).isSame(item.availableDay,'date'))
                return moment(dates[i].datestring).isSame(item.availableDay,'day');
                }); 
                if(matchedArr.length>0){
                    dates[i].form=matchedArr[0];
                }else{
                    dates[i].form=JSON.parse(JSON.stringify(this.defaultForm)); 
                }         
            };
            this.days=dates;
            console.log(dates)
        }

    }
}
</script>
<style scoped>
.calendar-week{
    width:98%;
    padding-left:1%;
    list-style-type: none;
    overflow: hidden;
}
.calendar-week li{
    width:14%;
    float:left;
}
.calendar-week li.disabled{
    color:#999;
}
</style>

