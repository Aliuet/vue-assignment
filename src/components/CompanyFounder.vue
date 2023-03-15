
<template>
  <div class="container">
    <div class="main-body">
      <div class="row">
        <div class="title">
          <h4 class="align-items-center">Please enter the founder's unique ID to get started.</h4>
        </div>
      </div>
      <div class="row">
        <div class="col-md-6 mx-auto">
          <hr>
          <label for="founder-id-input">Founder ID:<span class="text-danger"> * </span></label>
          <input
            id="founder-id-input"
            type="text"
            class="form-control mt-2"
            v-model="founderId"
            placeholder="Enter founder id"
          />
        </div>
      </div>
      <div class="row">
        <div class="col col-md-6 mx-auto">
          <br/>
          <h5 v-if="founder">
            You entered the founder ID <strong> '{{ founderId }}'</strong><br>this seems like a good place to show their data
          </h5>
        </div>
      </div>
      <div class="row gutters-sm">
        <div class="col-md-4 mb-3" v-if="avatarUrl">
          <div class="card">
            <div class="card-body">
              <div class="d-flex flex-column align-items-center text-center">
                <img  class="rounded-circle " width="150" :src="avatarUrl" alt="Founder avatar" />
                <div class="mt-3">
                  <h4>{{ founder.name.first }} {{  founder.name.last }}</h4>
                  <p class="text-secondary mb-1">{{company.name}}</p>
                  <p class="text-muted font-size-sm">{{ company.website }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="col-md-8" v-if="founder">
          <div class="card">
            <div class="card-body">
              <div class="row">
                <div class="col-sm-3">
                  <h6 class="mb-0">Full Name</h6>
                </div>
                <div class="col-sm-9 text-secondary">
                  {{ founder.name.first }} {{ founder.name.last }}
                </div>
              </div>
              <hr>
              <div class="row">
                <div class="col-sm-3">
                  <h6 class="mb-0">Email</h6>
                </div>
                <div class="col-sm-9 text-secondary">
                  {{ founder.email }}
                </div>
              </div>
              <hr>
              <div class="row">
                <div class="col-sm-3">
                  <h6 class="mb-0">Phone</h6>
                </div>
                <div class="col-sm-9 text-secondary">
                  {{ founder.phone }}
                </div>
              </div>
              <hr>
              <div class="row">
                <div class="col-sm-3">
                  <h6 class="mb-0">Gender</h6>
                </div>
                <div class="col-sm-9 text-secondary">
                  {{ founder.gender }}
                </div>
              </div>
              <hr>
              <div class="row">
                <div class="col-sm-3">
                  <h6 class="mb-0">Country</h6>
                </div>
                <div class="col-sm-9 text-secondary">
                  {{ founder.location.country }}
                </div>
              </div>
              <div class="row">
                <div class="col-sm-3">
                  <h6 class="mb-0">City</h6>
                </div>
                <div class="col-sm-9 text-secondary">
                  {{ founder.location.city }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
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

  }
};

</script>