<template>
  <div class="list">
    <todo-item v-for="user in users"
    :key="user.id"
    :obj="user"
    @del="(id)=>removeTodo(id)"
    @changeStatus="(id)=>changeStatus(id)"
    @changeStatus2="(id)=>changeStatus2(id)"
    />
  </div>
</template>

<script>
import TodoItem from './TodoItem.vue'
export default {
  components: { TodoItem },
  props:['users'],
  methods: {
    removeTodo(id){
      this.$emit('del', id);
    },
    changeStatus(id){
      this.$emit('changeStatus',id)
    },
    changeStatus2(id){
      this.$emit('changeStatus2',id)
    }
  },
}
</script>

<style lang="scss">
@import '@/styles/list.scss'
</style>