<template>
  <button @click="getUser" class="button">Get Random User</button>
  <div class="profile">

    <div v-if="!mounted" class="loader"></div>

    <div v-else>
      <div class="profile-card">
        <div class="profile-image">
          <img :src="name.image">
        </div>

        <div class="profile-info">
          <h3>{{ name.firstName }} {{ name.lastName }}</h3>
          <div class="addr">{{ name.city }}, {{ name.state }} {{ name.country }}</div>
          <div class="email">{{ name.email }}</div>
        </div>
      </div>
      <div class="short-info">"{{ name.info }}"</div>
    </div>

  </div>
</template>

<script>
  import { reactive, ref, } from '@vue/reactivity'
  import { onMounted } from '@vue/runtime-core'

  export default {
    setup: () => {
      const userData  = ref({})
      const mounted   = ref(false)
      const name      = reactive({
        firstName: null,
        lastName: null,
        image: null,
        email: null,
        city: null,
        state: null,
        country: null,
        info: null
      })

      const getUser = async () => {
        mounted.value       = false
        await fetch('https://randomuser.me/api')
          .then((response) => response.json())
          .then((data) => {
            userData.value  = data.results[0]
            name.firstName  = data.results[0].name.first
            name.lastName   = data.results[0].name.last
            name.image      = data.results[0].picture.large
            name.email      = data.results[0].email
            name.city       = data.results[0].location.city
            name.state      = data.results[0].location.state
            name.country    = data.results[0].location.country
            getLorem()
            mounted.value   = true
          })
      }

      const getLorem = async () => {
        await fetch('http://metaphorpsum.com/paragraphs/1/4')
          .then((response) => response.text())
          .then((data) => name.info = data)
      }

      onMounted(() => getUser())

      return { userData, name, mounted, getUser }
    },
  }
</script>

<style>
  .profile-card {
    display: flex;
    justify-content: flex-start;
  }
  .profile-info h3, .profile-info .addr, .profile-info .email {
    display: flex;
    margin-left: 15px;
  }
  .profile-info h3 { color: #9e9b30 }
  .addr, .email {
    font-size: 13px;
    color: #5e5f61;
  }
  .profile {
    max-width: 700px;
    margin: 0px auto;
    width: 100%;
    min-height: 100px;
    background: white;
    padding: 30px;
    box-sizing: border-box;
    overflow: hidden;
    border-radius: 20px;
  }
  .profile-image img {
    border-radius: 50%;
    border: 5px solid #42b883;
    padding: 5px;
  }
  .short-info {
    margin: 10px 50px;
    font-size: 15px;
    font-style: italic;
    color: #5e5f61;
  }
  .loader {
    border: 3px solid #f3f3f3;
    border-top: 3px solid #4db17a;
    border-radius: 50%;
    width: 20px;
    height: 20px;
    -webkit-animation: spin 0.8s linear infinite;
    animation: spin 0.8s linear infinite;
    margin: 0 auto;
  }
  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }
  .button {
    background-color: #4CAF50;
    border: none;
    color: white;
    padding: 15px 15px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    border-radius: 5px;
    cursor: pointer;
    transition: 1.5s;
    margin-bottom: 25px;
  }
  button:hover {
    background-color: #5aca69;
  }
</style>