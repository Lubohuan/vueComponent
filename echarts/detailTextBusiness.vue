<template>
  <div>
      <div class="flex special" v-for="(item,index) in list">
        <div class="flex-2 ellipsis text-left" style="position:relative;padding-left:20px;" :title="item.name">

            <span class="finishedSpan" :style="{background:item.color}"></span> {{item.name}}
        </div>
        <div class="flex-1 ellipsis text-center text-color-gray">
          <span>{{item.number}}个</span>
        </div>
        <div class="flex-1 ellipsis text-center text-color-gray">
          <span>{{item.rate}}%</span>
        </div>
        <div class="flex-2 ellipsis text-right">{{item.money}}</div>
      </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      data: {
        total: 0,
        finishedTotal: 0,
        finishedRate: "0",
        infinishedTotal: 0,
        infinishedRate: "0"
      },
      list: []
    };
  },

  methods: {
    writeText(list) {
      if (list) {
        this.list = list;
      }
    }
  },
  computed: {},
  created() {},
  mounted() {}
};
</script>



