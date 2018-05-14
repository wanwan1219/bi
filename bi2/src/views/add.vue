<template>
    <el-button type="primary" @click="close">取消</el-button>
</template>

<script>
export default {

  name: 'add',
  porps:['showadd'],
  data () {
    return {

    }
  },
  methods:{
    close(){
        /*this.showadd=false;
        this.$emit('closeadd',this.showadd)*/
        this.$emit('update:showadd',false)
    }
  }
}
</script>

<style lang="scss" scoped>
</style>