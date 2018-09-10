<template>
  <div class="hello">
    <input v-model="search" placeholder="Buscar..." />
    <br>
    <input v-model="name" @keyup.enter="add()" /><br>
    <strong>Digite um item acima e aperte Enter...</strong>
    
    <div class=list v-for="(item, index) in listnames" :key="index">
      {{item}} <span @click="delItem(index)">X</span><br>
    </div>
  </div>
</template>

<script>
import localforage from 'localforage'
export default {
  name: "HelloWorld",
  data() {
    return {
      search: "",
      name: "",
      names: []
    };
  },
  computed: {
    listnames() {
      return this.names.filter(
        row => row.toLowerCase().indexOf(this.search) >= 0
      );
    }
  },
  methods:{
    add() {
      this.names.push(this.name);
      this.name = "";
      localforage.setItem("Participantes", this.names);
    },
    delItem(index){
      this.names.splice(index,1)
      localforage.setItem("Participantes", this.names);
    }
  },
  mounted() {
    localforage.getItem('Participantes').then((value) => {
      if(!value) return
      this.names = value;
      console.log(value);
    }).catch(function(err) {
      // This code runs if there were any errors
      console.log(err);
    });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
input {
  margin: 5px 0;
}
.list {
  margin-top: 20px;
}
</style>
