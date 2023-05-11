<template>
  <div :class="`todo ${obj.main__status?'success':''} $ ${obj.main__status?status=false:status=true}`">
    <div class="todo__text">
      <div class="todo__title">{{obj.title}}</div>
      <div class="todo__data">
        <div class="toto__name">{{ obj.name }}</div>
        <div class="todo__date">{{ obj.date }}</div>
      </div>
    </div>
    <div class="todo__icons">
      <button v-if="status" class="btn btn__success"
      @click="status = !status, changeStatus(obj.id)"><i class="bi bi-check-lg"></i></button>
      <button v-else class="btn btn__warning"
      @click="status = !status, changeStatus2(obj.id)"><i class="bi bi-x-lg"></i></button>
      <button class="btn btn__info"><i class="bi bi-star"></i></button>
      <button @click="del(obj.id)" class="btn btn__danger"><i class="bi bi-trash"></i></button>
    </div>
  </div>
</template>

<script>

export default {
  props:['obj'],
  data() {
    return {
      status:'',
    }
  },
  methods:{
    del(id){
      this.$emit('del',id)
    },
    changeStatus(id){
      this.$emit('changeStatus', id)
    },
    changeStatus2(id){
      this.$emit('changeStatus2', id)
    },
  },
  computed:{
    log(){
      return console.log(this.status)
    }
  }

}
</script>

<style lang="scss">
@import '@/styles/todo.scss'
</style>