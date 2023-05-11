<template>
  <div class="add">
    <input type="text" placeholder="Sarlavhasi" v-model="user.title">
    <input type="text" placeholder="Ma'sul hodim" v-model="user.name">
    <input type="date" v-model="user.date">
    <button class="add__btn" @click="add()">Kiritish</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      user:{}
    }
  },
  methods:{
    add(){
      this.$emit('add', this.user);
      this.user = {};
    }
  }

}
</script>

<style lang="scss">
@import '@/styles/add.scss'
</style>