<template>
  <div class="chooser-component">
    <ul class="chooser-list">
     <li v-for="(item,index) in selections" @click="addItem(index)" :class="{'active':list[index]}">{{item.label}}</li>
    </ul>
  </div>
</template>

<script>
  import Vue from 'vue';
  import _ from 'lodash'
  export default{
      props :{
          selections:{
              type: Array,
              default: [
                {
                  label: '客户版',
                  value: 0
                }
              ]
          }
      },
    data(){
          return{
              list: [],
              selectionList: []
          }
    },
    methods: {
          addItem(index){
              if(!this.list[index]){
                  Vue.set(this.list,index,true)
                  this.selectionList.push(index)
              }else{
                Vue.set(this.list,index,false)
                this.selectionList = _.remove(this.selectionList,(n)=>{
                    return n!==index
                })
              }
              let objArray = _.map(this.selectionList,(n)=>{
                return this.selections[n];
              })
            this.$emit('on-change',objArray)
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
