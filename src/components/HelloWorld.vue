<template>
  <div>
    <div class="board" style="width:500px;background: gray;"  ref="mine">
      <div class="home">
        <grid-layout
          :layout="layoutData"
          :col-num="2"
          :row-height="layoutHeight"
          :is-draggable="dialogVisible"
          :is-resizable="dialogVisible"
          :is-mirrored="false"
          :vertical-compact="true"
          :margin="[10, 10]"
          :use-css-transforms="true"
        > 
          <grid-item v-for="(item,index) in layoutData" 
                    :x="item.x"
                    :y="item.y"
                    :w="item.w"
                    :h="item.h"
                    :i="item.i"
                    :key="item.i"
                      @resize="resizeEvent"
                    @move="moveEvent"
                    @resized="resizedEvent"
                    @moved="movedEvent"
          >
            {{item.t}}
          </grid-item>
        </grid-layout>
      </div>
    </div> 
 <div>  
   <button @click="download()" >截图</button>
   <a :href= "downImg" class="downLoadimg"  download="下载图片的名字dfg" ><img  :src="downImg" /></a>
 </div> 
  </div>
</template>

<script>
  import html2canvas from "html2canvas"
  import layoutData from '@/virtualData/layoutData.json'
  import VueGridLayout from 'vue-grid-layout' 
  const GridLayout = VueGridLayout.GridLayout
  const GridItem = VueGridLayout.GridItem
 
  export default {
    data() {
      return {
        // 布局数据
        layoutData: [], 
        layoutConfig: {
          height: 50, // 默认高度
          dialogVisible: false // 是否可拖拽或改变大小
        },
        dialogVisible:true,
        layoutHeight:50, 

        downImg:'',
       datalist:[
           {text:'1'},
           {text:'2'},
           {text:'3'},
            {text:'4'},
             {text:'5'},
        ] 
      }
    },
    components: {
      GridLayout,
      GridItem,
      html2canvas, 
    },
    methods: {
      download(){
          var this1=this;
          setTimeout(function () {
             　　 html2canvas(this1.$refs.mine,{
                      backgroundColor: null
                    }).then((canvas) => {
                      let dataURL = canvas.toDataURL("image/png");
                      this1.downImg=dataURL;
                      console.log(dataURL)
                    });

                  },1000)
      }, 
    // 图片上传服务器
　　sendUrl () {
　　　　// 如果图片需要formData格式,就自己组装一下,主要看后台需要什么参数
　　　   // const formData = new FormData()
　　　　// formData.append('base64', this.company.fileUrl)
　　　　// formData.append('userId', 123)
　　　　// formData.append('pathName', 'pdf')
　　　　this.$ajax({
　　　　　　url: apiPath.common.uploadBase,
　　　　　　method: 'post',
　　　　　　data: this.htmlUrl
　　　　}).then(res => {
　　　　　　if (res.code && res.data) {
 
　　　　　　}
　　　　})
　　},







       init() {
         let isMoveArg = sessionStorage.getItem('isMoveArg')
         if(!isMoveArg){ 
            this.layoutData = layoutData.mainData
         }else{
            this.layoutData =JSON.parse(sessionStorage.getItem("layoutData"))
         } 
        },
        moveEvent: function(i, newX, newY){
          sessionStorage.setItem("isMoveArg", 'true')
          sessionStorage.setItem("layoutData", JSON.stringify(this.layoutData)) 

        },
        resizeEvent: function(i, newH, newW){
          sessionStorage.setItem("layoutData", JSON.stringify(this.layoutData)) 
        },
        movedEvent: function(i, newX, newY){
          sessionStorage.setItem("layoutData", JSON.stringify(this.layoutData)) 
        },
        resizedEvent: function(i, newH, newW, newHPx, newWPx){
          sessionStorage.setItem("layoutData", JSON.stringify(this.layoutData)) 
        }, 
    },
    created() {
      this.init()  
    }, 
  }

</script>


<style scoped>
.vue-grid-item {
  background: #999;
}
.vue-grid-item>.vue-resizable-handle{  
      background-size: 0% !important;
}  
.downCon{
  width: 500px;
  margin: 10px;
}

</style> 