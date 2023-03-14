
<template>
  <div class="input-wrapper">
    <p>Please enter the founder's unique ID to get started.</p>
    <label for="founder-id-input">
      Founder ID:
      <input id="founder-id-input" type="text" v-model="founderId" placeholder="Any characters will do..." />
    </label>
    <p v-if="founder">
      You entered the founder ID '{{ founderId }}'… this seems like a good place to show their data…
      <br />
      Founder's name: {{ founder.name.first }} {{ founder.name.last }}
    </p>
    <div v-if="avatarUrl">
      <img :src="avatarUrl" alt="Founder avatar" />
    </div>
    <p v-if="company">
      The founder founded the company "{{ company.name }}" in {{ company.year }}.
    </p>
  </div>
    <!-- <br>
    <br>
    <button class="my-button" @click="demographicData">Random user Data</button>
    <br>
    <br>
    <button class="my-button" @click="avatorData">Avator Data</button>
    <br>
    <br>
    <button class="my-button" @click="companyData">Compan Founder Name</button> -->
</template>

<script>
// import axios from 'axios';
export default {
  data() {
    return {
      founderId: '',
      founder: null,
      avatarUrl: null,
      company: null,
    };
  },
  watch: {
    founderId(newVal) {
      if (newVal) {
        this.fetchFounderData(newVal);
        this.fetchFounderAvatar(newVal);
        this.fetchFounderCompany(newVal);
      } else {
        this.founder = null;
        this.avatarUrl = null;
        this.company = null;
      }
    },
  },
  methods: {
    async fetchFounderData(id) {
      try {
        const response = await fetch(`https://randomuser.me/api/?seed=${id}`);
        const data = await response.json();
        this.founder = data.results[0];
        console.log(this.founder)
      } catch (error) {
        console.error(error);
      }
    },
    fetchFounderAvatar(id) {
      this.avatarUrl = `https://robohash.org/${id}.png`;
      console.log(this.avatarUrl)
    },
    async fetchFounderCompany(id) {
      try {
        const response = await fetch(`https://fakerapi.it/api/v1/companies?_seed=${id}`);
        const data = await response.json();
        this.company = data.data[0];
        console.log(this.company)
      } catch (error) {
        console.error(error);
      }
    },
    //for usiing other apis
    // demographicData() {
    //   axios.get('https://randomuser.me/api/')
    //     .then(response => {
    //       console.log(response.data);
    //     })
    //     .catch(error => {
    //       console.log(error);
    //     });
    // },

    // avatorData() {
    //   axios.get('https://robohash.org/')
    //     .then(response => {
    //       console.log(response.data);
    //     })
    //     .catch(error => {
    //       console.log(error);
    //     });
    // },
    // companyData() {
    //   axios.get('https://fakerapi.it/')
    //     .then(response => {
    //       console.log(response.data);
    //     })
    //     .catch(error => {
    //       console.log(error);
    //     });
    // }
  }
};


</script>
<style scoped>
.my-button {
    background-color: rgb(32, 192, 109);
    color: white;
    font-weight: bold;
    padding: 10px;
    border-radius: 5px;
    padding-right: 4px;
  }
  .input-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
}

label {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 1em 0;
}

input {
  padding: 0.5em;
  border: 2px solid #ccc;
  border-radius: 4px;
  font-size: 1em;
  width: 100%;
  max-width: 20em;
}

input:focus {
  outline: none;
  border-color: #0077ff;
  box-shadow: 0 0 0 2px #0077ff33;
}
</style>