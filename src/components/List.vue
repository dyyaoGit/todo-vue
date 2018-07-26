<template>
  <ul class="lists">
    <li v-for="(item,index) in currentArr" :class="{done: item.isDone}">
      <span style="color: red;" @dblclick="handleDouble(index)" v-show="!item.isShow">
        {{item.title}}
      </span>
      <input type="text" v-model="item.title"
             v-show="item.isShow"
             @blur="handleBlur(index)"
             @keyup.enter="handleKeyup($event)"
             autofocus>
      <input type="checkbox" v-model="item.isDone" @change="changeStatus(item)">
      <button @click="removeSelf(item._id)">
        删除
      </button>
    </li>
  </ul>
</template>

<script>
  import axios from 'axios'

  export default {
    props: {
      arr: {
        type: Array
      },
      getData: {
        type: Function
      }
    },
    data () {
      return {
        currentArr: this.arr
      }
    },
    watch: {
      arr (val) {
        this.currentArr = val;
      },
      currentArr (val) {
        this.$emit("changeArr", val)
      }
    },
    methods: {
      changeStatus (item) {
        let isDone = item.isDone?1:0;
        let title = item.title;
        let id = item._id;
        axios.patch(`/api/todo/${id}`, {
          isDone,
          title
        }).then(res => {
          if(res.data.code == 200){
            alert("修改成功！");
            this.getData()
          }
        })
      },
      removeSelf (id) {
        axios.delete(`/api/todo/${id}`).then(res => {
          if(res.data.code == 200){
            alert("删除成功!")
            this.getData();
          }
          else {
            alert("删除失败!")
          }
        })
      },
      handleDouble (index) {
        this.currentArr[index].isShow = true;
      },
      handleBlur (index) {
        this.currentArr[index].isShow = false;
        this.changeStatus(this.currentArr[index]);
      },
      updateTitle (id,title) {

      },
      handleKeyup ($event) {
        $event.target.blur();
      }
    }
  }
</script>

<style scoped>
  .lists {
    width: 980px;
    margin: 20px auto 0;
    border-bottom: 1px solid #f1f1f1;
  }

  .lists li {
    line-height: 40px;
    height: 40px;
    color: #333;
    font-size: 16px;

  }
  .lists li.done {
    text-decoration: line-through;
  }
</style>
