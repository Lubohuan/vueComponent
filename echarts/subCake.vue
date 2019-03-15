<template>
  <div :id="domId">
    <span v-if="nodata" class="nodata">暂无数据</span>
  </div>
</template>
<script>
export default {
  props: ["domId"],
  data() {
    return {
      nodata: true
    };
  },
  methods: {
    drawCake(config) {
      if (!config) {
        this.nodata = true;
      } else {
        var domId = this.domId;
        var myChart = this.$echarts.init(document.getElementById(domId));
        var data = config
          ? config.optionData
          : [{ value: 0, name: "已完成" }, { value: 0, name: "未完成" }];
        var option = {
          title: [
            {
              text: config ? config.titleText : "title",
              x: "center",
              y: "40%",
              textStyle: {
                fontSize: 15,
                fontWeight: "normal",
                fontStyle: "normal",
                color: "#929292"
              }
            },
            {
              text: config ? config.titleTotal + "万元" : "0万元",
              x: "center",
              y: "50%",
              textStyle: {
                fontSize: 15,
                fontWeight: "normal",
                fontStyle: "normal",
                color: "#333333"
              }
            }
          ],
          tooltip: {
            trigger: "item",
            formatter: "{a} <br/>{b}: {c}万元" //"{a} <br/>{b}: {c}万元 ({d}%)"
          },
          color: config
            ? config.colorArray
            : [
                "#1890FF",
                "#2FC25B",
                "#FACC14",
                "#F04864",
                "#8543E0",
                "#13C2C2"
              ],
          grid: {
            left: "0",
            right: "0",
            bottom: "0",
            top: "0"
          },

          series: [
            {
              name: config ? config.seriesName : "",
              type: "pie",
              radius: ["70%", "90%"], //改变圆环的大小
              startAngle: 90,
              avoidLabelOverlap: false,

              label: {
                normal: {
                  show: false,
                  position: "center"
                },
                emphasis: {
                  show: false,
                  textStyle: {
                    fontSize: "30",
                    fontWeight: "bold"
                  }
                }
              },
              labelLine: {
                normal: {
                  show: false
                }
              },
              itemStyle: {
                normal: {
                  label: {
                    show: true,
                    //	                            position:'inside',
                    formatter: "{b} : {c} ({d}%)"
                  },
                  borderColor: "#ffffff",
                  borderWidth: 0//是否有间隔
                },
                labelLine: { show: true }
              },
              data: data
              //data:[{value: 12,name:'已完成'},{value: 15,name: "未完成"}]
            }
          ]
        };
        // 使用刚指定的配置项和数据显示图表。
        myChart.setOption(option);
        window.addEventListener("resize", () => {
          myChart.resize();
        });
      }
    }
  },
  computed: {},
  created() {},
  mounted() {}
};
</script>



