<template>
  <div>
    <h1>Właściciele</h1>
    <table>
      <thead>
      <tr>
        <th>id</th>
        <th>imie</th>
        <th>nazwisko</th>
        <th>adres</th>
        <th>wiek</th>
      </tr>
      </thead>
      <tbody v-for="user of users" :key="user.id">
      <tr>
        <th>    {{ user.userId }} </th>
        <th> {{ user.firstName }} </th>
        <th> {{ user.lastName }} </th>
        <th> {{ user.address }} </th>
        <th> {{ user.age }} </th>
        <th> <button >edit</button></th>
        <th> <button >delete</button></th>
      </tr>
      </tbody>
      <tfoot>
      <tr @keyup.enter="addUser">
        <th>
          dodaj
        </th>
        <th>
          <input
            type="text"
            v-model="userName"
            aria-label=""
            placeholder="imię">
        </th>
        <th>
        <input
          type="text"
          v-model="userLastName"
          aria-label=""
          placeholder="nazwisko">
        </th>
        <th>
        <input
          type="text"
          v-model="userAddress"
          aria-label=""
          placeholder="adres">
        </th>
        <th>
          <input
            type="text"
            v-model="userAge"
            aria-label=""
            placeholder="wiek">
        </th>
      </tr>
      </tfoot>
    </table>
    </div>
</template>

<script>
import axios from 'axios'

const baseURL = 'http://localhost:8082/users'

let id = 1

export default {
  name: 'UsersList',
  data () {
    return {
      users: [],
      userId: id++,
      userName: '',
      userLastName: '',
      userAddress: '',
      userAge: ''
    }
  },
  async created () {
    try {
      const res = await axios.get(baseURL)

      this.users = res.data
      id = this.users.reduce((prev, current) => (prev.id > current.id) ? prev : current, 1)
    } catch (e) {
      console.error(e)
    }
  },
  methods: {
    async addUser () {
      try {
        const res = await axios.post(baseURL, {
          firstName: this.userName,
          lastName: this.userLastName,
          address:
          this.userAddress,
          age: this.userAge
        })

        this.users = [...this.users, res.data]

        this.userName = ''
        this.userLastName = ''
        this.userAddress = ''
        this.userAge = ''
      } catch (e) {
        console.error(e)
      }
    }
  }
}
</script>

<style scoped>
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
</style>
