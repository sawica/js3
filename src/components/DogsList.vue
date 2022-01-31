<template>
<div style="display: inline-flex">
  <div>
    <h1>Pieski</h1>
    <table>
      <thead>
      <tr>
        <th>id</th>
        <th>imie</th>
        <th>data urodzenia</th>
        <th>właściciel</th>
      </tr>
      </thead>
      <tbody v-for="dog of dogs" :key="dog.id">
      <tr>
        <th>    {{ dog.id }} </th>
        <th> {{ dog.name }} </th>
        <th> {{ dog.birthdate }} </th>
        <th v-if="users.filter(user => user.userId === dog.userId)"> {{ dog.userId}} </th>
        // nie wiem jak tu wyciagnac wartosc user.firstName czy cos
        <th> <button :value="dog" @click="edit = dog">edit</button></th>
        <th> <button :value="dog.id" @click="remove = dog.id">delete</button></th>
      </tr>
      </tbody>
      <tfoot>
      <tr @keyup.enter="addDog">
        <th>
          dodaj
        </th>
        <th>
          <input
            type="text"
            v-model="dogName"
            aria-label=""
            placeholder="imię">
        </th>
        <th>
          <input
            type="date"
            v-model="dogBirthdate"
            aria-label=""
            placeholder="data urodzenia">
        </th>
      </tr>
      </tfoot>
    </table>
  </div>
</div>
</template>

<script>
import axios from 'axios'

const baseURL = 'http://localhost:8082/dogs'
const baseUserURL = 'http://localhost:8082/users'

let id = 1

export default {
  name: 'DogsList',
  data () {
    return {
      dogs: [],
      dogName: '',
      dogId: id++,
      dogBirthdate: '',
      users: []
    }
  },
  async created () {
    try {
      const res = await axios.get(baseURL)
      this.dogs = res.data
      const resUsers = await axios.get(baseUserURL)
      this.users = resUsers.data
      id = this.dogs.reduce((prev, current) => (prev.id > current.id) ? prev : current, 1)
    } catch (e) {
      console.error(e)
    }
  },
  methods: {
    async addDog () {
      try {
        const res = await axios.post(baseURL, { id: this.id, name: this.dogName, birthdate: this.dogBirthdate })

        this.dogs = [...this.dogs, res.data]

        this.dogName = ''
        this.dogBirthdate = ''
      } catch (e) {
        console.error(e)
      }
    },
    async edit (dog) {
      document.getElementById('edit').hidden = false
      try {
        // const res = await axios.post(baseURL, { id: this.id, name: this.dogName, birthdate: this.dogBirthdate })
        dog.name = this.dogName
        dog.birthdate = this.dogBirthdate
        // this.dogs = [...this.dogs, res.data]

        this.dogName = ''
        this.dogBirthdate = ''
      } catch (e) {
        console.error(e)
      }
      document.getElementById('edit').hidden = true
    },
    remove (id) {
      this.dogs = this.dogs.filter(dog => dog.id !== id)
    }
  }
}
</script>

<style scoped>
header {
  width:100%;
  height: 80px;
  display: flex;
  justify-content: center;
  align-items: center;
}
nav {
  display: flex;
  justify-content: center;
  align-items: center;
  border: double darkgray;
  /*border: double #FFDB84;*/
  /*border: dotted darkgray;*/
  width: 100%;
  min-height: 50px;
  font-size: x-large;
  font-family: FreeMono, monospace;
}
p {
  padding: 10px;
}
section {
  /*background: rgb(255, 255, 255);*/
  min-height: 450px;
  width:100%;
  height: 100%;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}
table {
  alignment: center;
  /*font-family: FreeMono, monospace;*/
  font-variant: normal;
}
label {

}
tr {
  margin: auto;
  height: 40px;
  width: 150px;
  text-align: center;
}
tbody>tr:hover{
  /*background: antiquewhite;*/
  background-color: #FFDB84;
}
th {
  padding: 15px;
  text-align: center;
}

td {
  padding: 15px;
}
input {
  padding: 5px;
}
label {
  padding: 5px;
}
button {
  padding: 5px 15px ;
  border-radius: 6px;
  border: 1px outset;
  background-color: white;
  color: black;
  text-align: center;
  font-family: FreeMono, monospace;
  font-variant: small-caps;
}
button:hover {
  color: white;
}
.chooseButton {
  background-color: white;
  border-color: lightgreen;
}
.chooseButton:hover {
  background-color: lightgreen;
}
.resumeButton {
  background-color: white;
  border-color: #FF633C ;
  font-family: sans-serif, sans-serif;
  font-weight: bolder;
  font-size: smaller;
}
.resumeButton:hover {
  background-color: #FF633C;
}
.createButton {
  font-size: x-large;
  background: #FFC400;
  margin: 30px;
  width: 100px;
}
.switchButton {
  font-size: x-large;
  /*background: #15B091;*/
  border-color: lightgreen ;
  margin: 10px;
  width: 100px;
}
form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
div {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  /*padding: 15px;*/
  /*margin: 50px;*/
  font-family: FreeMono, monospace;
  font-variant: small-caps;

}
.addPanel {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  /*border: 1px solid darkgray;*/
  border: 1px solid #FFDB84;
}
.editPanel {
  background-color: #FFDB84;
  padding: 15px;
}
footer {
  display: flex;
  justify-content: center;
  align-items: center;
  /*border: dotted darkgray;*/
  /*border: double #FFDB84;*/
  border: double darkgray;
  min-height: 50px;
  font-family: "Fira Code Medium", monospace;
}
h1 {
  text-decoration: underline;
}

li {
  color: white;
}

input {
  width: 100%;
  padding: 1rem;
  border-radius: 0.4rem;
  border: 1px solid #fd9644;
  margin-bottom: 2rem;
  font-size: 1.5rem;
}
/*table {*/
/*  width: 100%;*/
/*  padding: 1rem;*/
/*  border-radius: 0.4rem;*/
/*  border: 1px solid #fd9644;*/
/*  margin-bottom: 2rem;*/
/*  font-size: 1.5rem;*/
/*  font-family: "Fira Code Light",monospace;*/
/*  background-color: whitesmoke;*/
/*}*/
/*#edit {*/
/*margin: auto 20px auto;*/
/*}*/
</style>

}
