<template>
  <div >
    <div class="flex">
      <div class="flex-2 ellipsis text-center">总计划产值</div>
      <div class="flex-1"></div>
      <div class="flex-2 ellipsis text-right">{{data.total}}</div>
    </div>
    <div class="flex">
      <div class="flex-2 ellipsis text-center">
        <i class="finished"></i>已完成
      </div>
      <div class="flex-1 ellipsis text-center text-color-gray">
        <span>{{data.finishedRate}}%</span>
      </div>
      <div class="flex-2 ellipsis text-right">{{data.finishedTotal}}</div>
    </div>
    <div class="flex">
      <div class="flex-2 ellipsis text-center">
        <i class="nofinished"></i>未完成
      </div>
      <div class="flex-1 ellipsis text-center text-color-gray">
        <span>{{data.infinishedRate}}%</span>
      </div>
      <div class="flex-2 ellipsis text-right">{{data.infinishedTotal}}</div>
    </div>
    <div class="unit text-right text-color-gray">单位：万元</div>
  </div>
</template>
<script>
export default {
  data() {
    return {
        data:{
          total:0,
          finishedTotal:0,
          finishedRate:'0',
          infinishedTotal:0,
          infinishedRate:'0'
        }
    };
  },

  methods: {
    writeText(data){
      if(data){
        this.data = data
      }
    }
  },
  computed: {},
  created() {},
  mounted() {}
};
</script>



