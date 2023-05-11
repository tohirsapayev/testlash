<template>
  <div class="header">
    <div class="search">      
      <input type="text" placeholder="Izlash">
    </div>
    <div class="search__btn">
      <button :class="`btn ${btnClass=='all'?'active':''}`" 
      @click="btnClass = 'all', all()">Barchasi</button>
      <button :class="`btn ${btnClass == 'done'?'active':''}`" 
      @click="btnClass = 'done', done()">Bajarilgan</button>
      <button :class="`btn ${btnClass == 'wait'?'active':''}`" 
      @click="btnClass = 'wait', wait()">Bajarilmagan</button>
    </div>
  </div>
</template>

<script>
export default {
  props:['uri'],
  data() {
    return {
      btnClass: 'all',
    }
  },
  methods:{
    all(){
      fetch(this.uri)
      .then(res => res.json())
      .then(allUsers => {
        this.$emit('all', allUsers)
      })
    },
    done(){
      let done = [];
      fetch(this.uri)
      .then(res => res.json())
      .then(users => {
        users.forEach(user => {
          if(user.main__status){
            done.push(user);
          }
        });
        this.$emit('done', done)
      })
    },
    wait(){
      let wait = [];
      fetch(this.uri)
      .then(res => res.json())
      .then(users => {
        users.forEach(user => {
          if(!user.main__status){
            wait.push(user);
          }
        });
        this.$emit('wait', wait)
      })
    }
  }

}
</script>

<style lang="scss">
@import '@/styles/header.scss'
</style>