<template>
  <div id="app">
    <div id="global">
            <div id='calc_part'>
                <div id="result_screen">
                    
                  <textarea readonly="readonly" class="cal_form" :style="result_size">{{cal_result}}</textarea>
                  <textarea readonly="readonly" class="my_result" :style="big_result_size">{{big_result}}</textarea>
                </div>
              <draw_canvas></draw_canvas><!--画布-->
            </div>
            <div id="history_meue" >

              <div class='his_item' v-for="item in show_icon_list">
                <textarea readonly="readonly" class="page-day" :style="item[3]">{{item[0]}}</textarea>
                <textarea readonly="readonly" class="page-my" :style="item[2]">{{item[1]}}</textarea>
              </div>

            </div>
        </div>
  </div>
</template>

<script>
  import draw_canvas from "./examples/draw_canvas";
  export default {
    name: 'App',
    components : {draw_canvas},
    data:function() {
      return {
        icon_list:[],
        show_icon_list:[],
        icon_index:0,
        cal_result:'',
        big_result:'',
        result_size:'',
      }
    },

    methods: {
      update:function(item,to_init=false){//[结果，算式]
        //var item=['2','1+1=2']
        this.cal_result=item[1]
        this.big_result=item[0]
        this.big_result_size="font-size:"+Math.floor(window.screen.availHeight*0.175-55)+"px;color:white;"
        this.result_size="font-size:"+Math.floor(window.screen.availHeight*0.075-35)+"px;color:white"
        if(to_init){
            this.big_result_size=this.result_size
            return
        }
        if(this.icon_index==10){
          var tmp_list=JSON.parse(JSON.stringify(this.icon_list));
          this.icon_list=[]
          for(var i=0;i<9;i++){
            this.icon_list.push(tmp_list[i+1])
          }
        }
        /*var item_style="font-size:"+Math.max(Math.min(Math.floor(window.screen.availHeight*0.1-30),Math.floor((window.screen.availWidth*0.3-6)*0.7)/item[1].length),Math.floor((window.screen.availHeight*0.1-30)/2))+"px;overflow-y:hidden"*/
        var item_style="font-size:"+Math.floor(window.screen.availHeight*0.1-30)+"px;"
        var result_style="font-size:"+Math.floor(window.screen.availHeight*0.1-30)+"px;"
        item.push(item_style)
        item.push(result_style)
        this.icon_list.push(item)
        if(this.icon_index!=10){
          this.icon_index+=1
        }
        //var tmp_list=JSON.parse(JSON.stringify(this.icon_list));
        this.show_icon_list=[]
        for(var i =0;i<this.icon_list.length;i+=1){
          this.show_icon_list.push(this.icon_list[this.icon_list.length-i-1])
        }
        //console.log(this.icon_list)
      }
    },
    mounted: function() {
      this.update(['(*^_^*)            \n此处显示计算结果喔    ','这边为你保存历史记录哟------>'],true)
      this.update(['1','2132'])
    },

}


</script>

<style>
            /*定义全局的div布局*/
            div#global{
                /*设置宽度，设置百分比可以根据浏览器自适应宽度*/
                width: 100%;
                height: calc(100vh);
                /*设置背景颜色，方便区分布局*/
                background-color: silver;
            }
            textarea{
                border:none;
                resize:none;
                word-wrap: normal;
                overflow-y:hidden;
               
            }
            #history_meue{
                width: 30%;
                height:100%;
                box-sizing: border-box;
                float: right;
                border-left: 3px solid silver;
                background-color: #999999;
            }
            #calc_part{
                width: 70%;
                height:100%;
                box-sizing: border-box;
                float: left;
                border-right: 3px solid silver;

            }
            .page-day{

        /*line-height: 42px;*/
        float: left;
        /*background-color: #CCFF99;*/
        /*padding-right: 5%;*/
        height: 100%;
        width: 30%;
        font-family: "Times New Roman";
        font-style:italic;
        border-width: 0px;
        background-color: #999999;

    }
    .his_item{
      /*padding-top: 3%;*/
      /*padding-bottom: 3%;*/
      font-size: 50px;
      width: 100%;
      /*float: right;*/
      height: 10%;
      /*border-right: 3px solid;
      border-top: 3px solid;*/
      border-style: inset;
      border-right: medium none;
      border-left: medium none;
      position:relative;

    }
    .page-my{
        float: right;
        background-color: #999999;
        /*font-size: calc(21vw/4);*/
        font-size: 30px;
        height: 100%;
        width: 70%;
        padding-right: 3px;
        position:absolute;
        bottom:0px;
        right: 3px;
        /*background-color: #CCFF99;*/
        border-width: 0px;
    }
    .my_result{
              /*font-size: 52px;*/
                background-color: black;
                text-align: right;
                font-family: 'CharlisSILTalic';
                padding-top: 6px;
                width: 100%;
                height: 70%;
                border-width: 0px;
                /*padding-bottom: 3px;*/
            }
            .cal_form{
              /*font-size: 52px;*/
                background-color: black;
                text-align: right;
                font-family: 'CharlisSILTalic';
                padding-top: 6px;
                width: 100%;
                height: 30%;
                border-width: 0px;
                /*padding-bottom: 3px;*/
            }
            #result_screen{
                width: 100%;
                height: 25%;
                background-color: black;
                border-bottom: 3px solid silver;
                /*float: top;*/
            }
            #draw_canvas{
                width: 100%;
                height: 75%;
                border-top: 3px solid silver;

                /*float: button;*/
            }

        </style>
