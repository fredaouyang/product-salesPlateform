<template>
  <div class="chooser-component">
    <ul class="chooser-list">
      <li v-for="(item, index) in selections" @click="addItem(index)" :title="item.label" :class="{ 'active': list[index] }">{{ item.label }}</li>
    </ul>
  </div>
</template>

<script>
import Vue from 'vue';

export default {
  props: {
    selections: {
      type: Array,
      default: [{
        label: 'test',
        value: 0
      }]
    }
  },
  data() {
    return {
      list: [],
      selectionsList: []
    };
  },
  methods: {
    addItem(index) {
      // 注意，由于vue直接修改数组里的某个参数，或者改变数组的length，data是变了，但是不会引起页面的重新渲染，
      // 这时候就需要使用Vue.set或splice来引起页面的渲染，由于数组一开始是空数组，所以splice不行，改用Vue.set。
      // 根据这个list来判断数据有没有被勾上，和设置style。
      // _.remove，_.map是lodash插件的API用法
      if (!this.list[index]) {
        Vue.set(this.list, index, true);
        this.selectionsList.push(index);
        console.log(index)
      } else {
        Vue.set(this.list, index, false);
        _.remove(this.selectionsList, (n) => {
          return n === index;
        });
      }
      console.log(this.list)
      var selectionsListEmit = _.map(this.selectionsList, (n) => {
        return this.selections[n];
      });
      this.$emit('on-change', selectionsListEmit);
    }
  }
}
</script>

<style scoped>
.chooser-component {
  position: relative;
  display: inline-block;
}

.chooser-list li {
  display: inline-block;
  border: 1px solid #e3e3e3;
  height: 25px;
  line-height: 25px;
  padding: 0 8px;
  margin-right: 5px;
  border-radius: 3px;
  text-align: center;
  cursor: pointer;
}

.chooser-list li.active {
  border-color: #4fc08d;
  background: #4fc08d;
  color: #fff;
}
</style>
