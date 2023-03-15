
<template>
  <div class="container">
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

    <div class="row" >
      <div class="col-md-6 mx-auto">
        <hr>
        <u>
          <h4 class="d-flex align-items-center justify-content-between mt-2">
                Founder Details:
          </h4>
        </u>
      </div>
    </div>
    <div v-if="founder">
      <div class="row">
        <div class="col-md-6 mx-auto">
          <div class="table-responsive">
            <table  class="table table-bordered mt-3">
              <thead class="bg-success text-white">
                <tr>
                  <th class="px-1 text-center white-space-nowrap">Name</th>
                  <th class="px-1 text-center white-space-nowrap">Last Name</th>
                  <th class="px-1 text-center white-space-nowrap">Gender</th>
                  <th class="px-1 text-center white-space-nowrap">Email</th>
                  <th class="px-1 text-center white-space-nowrap">City</th>
                  <th class="px-1 text-center white-space-nowrap">Country</th>
                  <th class="px-1 text-center white-space-nowrap">Phone</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td class="px-1 text-center white-space-nowrap">{{ founder.name.first }} {{ founder.name.last }}</td>
                  <td class="px-1 text-center white-space-nowrap">{{ founder.name.last }}</td>
                  <td class="px-1 text-center white-space-nowrap">{{ founder.gender }}</td>
                  <td class="px-1 text-center white-space-nowrap">{{ founder.email }}</td>
                  <td class="px-1 text-center white-space-nowrap">{{ founder.location.city }}</td>
                  <td class="px-1 text-center white-space-nowrap">{{ founder.location.country }}</td>
                  <td class="px-1 text-center white-space-nowrap">{{ founder.phone }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
    <div v-if="avatarUrl">
      <div class="row">
        <div class="col-md-6 mx-auto">
          <hr>
          <u>
            <h4 class="d-flex align-items-center justify-content-between mb-3 mt-2">
                  Avatar Image:
            </h4>
          </u>
          <div class="card card-default">
            <div class="card-body bg-info">
                <img  class="img-fluid w-50px h-50px border p-1 rounded-circle" :src="avatarUrl" alt="Founder avatar" />
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-if="company">
      <div class="row">
        <div class="col-md-6 mx-auto">
          <hr>
          <u>
            <h4 class="d-flex align-items-center justify-content-between mb-3 mt-3">
                 Company Name:
            </h4>
          </u>
        </div>
      </div>
      <div class="row">
        <div class="col-md-6 mx-auto">
          <p class="mt-2"> The founder founded the company:</p>
          <h6>
            <strong>Name: </strong>"{{ company.name }}" <br/>
            <strong>website: </strong>  {{ company.website }}.
          </h6>
          <hr>
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