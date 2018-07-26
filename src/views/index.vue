<template>
    <div>
      <Header></Header>
      <Content :getData="getData"></Content>
      <List :arr="arr" @changeArr="updateArr" :getData="getData"></List>
    </div>
</template>

<script>
    import axios from 'axios'
    import Header from '@/components/Header'
    import Content from '@/components/Content'
    import List from '@/components/List'

    export default {
        name: "index",
        components: {
          Header,
          Content,
          List
        },
        data() {
          return {
            arr: [{
              title: "吃饭",
              isDone: false
            }]
          }
        },
        methods: {
          updateArr (val) {
            this.arr = val;
          },
          getData () {
            axios.get("/api/todo")
              .then(res => {
                this.arr = res.data.data.map(item => {
                  item.isShow = false;
                  return item;
                });
              })
          }
        },
        mounted () {
          this.getData();
        }
    }
</script>

<style scoped>

</style>
