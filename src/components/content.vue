<template>
    <div class="content">
      <input type="text" v-model="title" @keyup.enter="handleAdd">
      <button @click="handleAdd">
        添加
      </button>
    </div>
</template>

<script>
    import axios from 'axios'

    export default {
        props: {
          getData: {
            type: Function
          }
        },
        data () {
          return {
            title: ""
          }
        },
        methods: {
          handleAdd () {
            if(!this.title.trim()){
              alert("请输入todo后点击添加")
            }
            else {
              let title = this.title;

              axios.post(`/api/todo`,{title}).then(res => {
                if(res.data.code == 200){
                  alert("添加成功");
                  this.title = "";
                  this.getData();
                }
                else {
                  alert("添加失败");
                }
              })
            }
          }
        }
    }
</script>

<style>
  .content {
    display: flex;
    width: 980px;
    margin: 20px auto 0;
  }
  .content input {
    outline: none;
    line-height: 40px;
    height: 40px;
    border: 1px solid #eee;
    border-radius: 4px;
    padding: 0 5px;
    color: #555;
  }
  button {
    -webkit-appearance:none;
    border-color: #409eff;
    line-height: 40px;
    height: 40px;
    color: #fff;
    background: #409eff;
    border-radius: 4px;
    padding: 0 20px;
    outline: none;
    box-shadow: none;
    font-size: 16px;
    color: #fff;
    margin-left: 10px;
  }
</style>
