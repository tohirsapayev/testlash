<template>
  <div class="main">
    <header-item
    :uri="uri"
    @all="(allUsers)=>all(allUsers)"
    @done="(doneUsers)=>done(doneUsers)"
    @wait="(waitUsers)=>wait(waitUsers)"
    @search="(user) => search(user)"
    />
    <todo-list 
    :users="workers"
    @del="(id)=>removeTodo(id)"
    @changeStatus="(id)=>changeStatus(id)"
    @changeStatus2="(id)=>changeStatus2(id)"
    />
    <add-item
    @add="(user) => addTodo(user)"
    />
  </div>
</template>

<script>
import AddItem from './components/AddItem.vue'
import HeaderItem from './components/HeaderItem.vue'
import TodoList from './components/TodoList.vue'

export default {
  name: 'App',
  components: {
    HeaderItem,
    TodoList,
    AddItem
  },
  data() {
    return {
      uri:"http://localhost:3000/users",
      users:[],
      status: ''
    }
  },
  methods:{
    removeTodo(id){
      let index = this.users.findIndex(todo => todo.id == id)
      if(index !== -1){
        this.users.splice(index, 1);
      }
      fetch(`${this.uri}/${id}`, {
        method:'delete'
      })
    },
    changeStatus(id){
      this.status = true;
      let index = this.users.findIndex(user => user.id == id);
      if(index !== -1){
        let user = this.users[index];
        fetch(`${this.uri}/${id}`,{
          method: 'PUT',
          headers: {
            'Content-Type':'application/json'
          },
          body: JSON.stringify({
            main__status: true,
            name: user.name,
            title: user.title,
            date: user.date
          })
        }).then(res => res.json())
        .finally(
          fetch(this.uri).then(res => res.json())
          .then(users => {
          this.users = users;
          })
        )
      }
    },
    changeStatus2(id){
      let index = this.users.findIndex(user => user.id == id);
      if(index !== -1){
        let user = this.users[index];
        fetch(`${this.uri}/${id}`,{
          method: 'PUT',
          headers: {
            'Content-Type':'application/json'
          },
          body: JSON.stringify({
            main__status: false,
            name: user.name,
            title: user.title,
            date: user.date
          })
        }).then(res => res.json())
        .finally(
          fetch(this.uri).then(res => res.json())
          .then(users => {
          this.users = users;
          })
        )
      }
    },
    addTodo(user){
      if(user.name && user.title && user.date){
        fetch(this.uri,{
          method: 'PoST',
          headers: {
            'Content-Type':'application/json'
          },
          body: JSON.stringify(user)
          }).then(res => res.json())
          .then(user => {
            this.users.push(user);
          })
        }
    },
    all(allUsers){
      this.users = allUsers
    },
    done(doneUsers){
      this.users = doneUsers
    },
    wait(waitUsers){
      this.users = waitUsers
    }
  },
  computed:{
    workers(){
      return this.users
    }
  },
  mounted(){
    fetch(this.uri).then(res => res.json())
    .then(users => {
      this.users = users;
    })
  },
}
</script>

<style lang="scss">
@import '@/styles/app.scss'
</style>
