<template>
    <div :id="domId">
      <span v-if='nodata' class="nodata">
        暂无数据
      </span>
    </div>
</template>
<script>
export default {
  props:['domId'],

  data() {
    return {
      nodata:true
    };
  },
  methods: {
    drawLine(config) {
       if(!config){
        this.nodata=true;
        return
      }
      var domId = this.domId;
      var myChart = this.$echarts.init(document.getElementById(domId));
      var option = {
        tooltip: {
          trigger: "axis",
          axisPointer: {
            // 坐标轴指示器，坐标轴触发有效
            type: "shadow" // 默认为直线，可选为：'line' | 'shadow'
          }
        },
        legend: {
          data: ["计划产值", "完成产值"],
          align: "right",
          right: 10
        },
        grid: {
          left: "20px",
          right: "20px",
          bottom: "20px",
          containLabel: true
        },
        dataZoom: [
          {
            show: true,
            start: 0,
            end: 100
          }
        ],
        color: ["#4FCB74"],
        xAxis: [
          {
            type: "category",
            data: ["新虹桥", "中山公园", "虹桥", "镇宁路", "天山古北"]
          }
        ],
        yAxis: [
          {
            type: "value",
            name: "单位：万元",
            axisLabel: {
              formatter: "{value}"
            }
          }
        ],
        series: [
          {
            name: "完成产值",
            type: "line",
            LineWidth: 10, //柱子宽度
            data: [10, 20, 5, 9, 3]
          }
        ]
      };
      myChart.setOption(option);
      window.addEventListener("resize", () => {
        myChart.resize();
      });
    }
  },
  computed: {},
  created() {},
  mounted() {
   
  }
};
</script>



