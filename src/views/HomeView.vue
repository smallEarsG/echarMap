<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <!-- 1. 需要先指定宽高 ；2. ref标记是为了应和vue获取dom的方式。 -->
    <div style="width: 800px; height: 600px" ref="chartsDOM"></div>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

import getGuangXiMap from "@/api/getChinaMap";
import * as echarts from "echarts";
export default {
  name: "HomeView",
  components: {},
  methods(){
   
},
  mounted() {
    function randomPieSeries(center, radius) {
    const data = ['A', 'B', 'C', 'D'].map((t) => {
      return {
        value: Math.round(Math.random() * 100),
        name: 'Category ' + t
      };
    })
  }
    // 初始化统计图对象
    var myChart = echarts.init(this.$refs["chartsDOM"]);
    // 显示loading动画
    myChart.showLoading();
    getGuangXiMap.then((res) => {
      // 得到结果后，关闭动画
      myChart.hideLoading();
      // 注册地图(数据放在axios返回对象的data中哦)
      echarts.registerMap("china", res.data);
      var option = {
        series: [
          {
            colorBy :'data',
            name: "中国地图",
            type: "map",
            map: "china", // 这个是上面注册时的名字哦，registerMap（'这个名字保持一致'）
            label: {
              // show: true
            },
            data: [
              { name: "广西省", value: 12001},
              { name: "湖南省", value: 12001 },
              { name: "广东省", value: 12001 },
              { name: "来宾市", value: 123333301 },
            ],
            itemStyle: {
                normal: {//未选中状态
                    borderWidth:2,//边框大小
                    borderColor:'lightgreen',
                    areaColor: 'orange',//背景颜色
                    label: {
                        show: false//显示名称
                    }
                },
                emphasis: {// 也是选中样式
                    borderWidth:2,
                    borderColor:'#fff',
                    areaColor: '#333',
                    label: {
                        show: false,
                        textStyle: {
                            color: '#fff'
                        }
                    }   
                 }    
                },
          },
        ],
        
    //     series: [
    //   // randomPieSeries([-86.753504, 33.01077], 15),
    //   // randomPieSeries([-116.853504, 39.8], 25),
    //   // randomPieSeries([24.7, 114.2], 30),
    //   // randomPieSeries(
    //   //   // it's also supported to use geo region name as center since v5.4.1
    //   //   +echarts.version.split('.').slice(0, 3).join('') > 540
    //   //     ? 'Maine'
    //   //     : // or you can only use the LngLat array
    //   //       [-69, 45.5],
    //   //   12
    //   // )
    // ],
        title: {
              text: '中国地图'
        },
        tooltip: {
          // trigger: 'item',
          // enterable:true,
          // renderMode:'richText',
          formatter: '{a}: {c}<br />{b}: {c};',
         showDelay: 0,
        transitionDuration: 0.2,
        triggerOn:'click',
        backgroundColor :'rgba(50,50,50,0.7)',
        borderColor : '#fff',
        textStyle:{
          color : '#fff'
        }
        },
        visualMap: {
          min: 1111,
          max: 333332,
          realtime: false,
          calculable: true,
          inRange: {
            color: ['lightskyblue', 'yellow', 'orangered']
          },
          show:true
          
        },
        // backgroundColor:"red"
      };
      myChart.setOption(option);
    });
  },
};
</script>
