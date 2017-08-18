<template>
  <div id="app" class="container">
    <h1>Real Time VueJS dan Firebase Database</h1>

    <div class="card">
      <h4 class="card-header">
        Add Customer
      </h4>
      <div class="card-body">
        <p class="card-text">
          <form @submit.prevent="addCustomer">
            <div class="form-group row">
              <label for="name" class="col-sm-2 col-form-label">Name:</label>
              <div class="col-sm-10">
                <input type="text" id="name" class="form-control" v-model="dtCustomer.name">
              </div>
            </div>
            <div class="form-group row">
              <label for="city" class="col-sm-2 col-form-label">City:</label>
              <div class="col-sm-10">
                <input type="text" id="city" class="form-control" v-model="dtCustomer.city">
              </div>
            </div>
            <div class="form-group row">
              <label for="phone" class="col-sm-2 col-form-label">Phone:</label>
              <div class="col-sm-10">
                <input type="text" id="phone" class="form-control" v-model="dtCustomer.phone">
              </div>
            </div> 
            <button type="submit" class="btn btn-primary float-right">Add Customer</button>        
          </form>
        </p>
      </div>
    </div>

    <div class="card">
      <h4 class="card-header">
        Customers Lists
      </h4>
      <div class="card-body">
        <p class="card-text">
          <table class="table">
            <thead class="thead-inverse">
              <tr>
                <th>Name</th>
                <th>City</th>
                <th>Phone</th>
                <th>Remove</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="customer in customers">
                <td>{{ customer.name }}</td>
                <td>{{ customer.city }}</td>
                <td>{{ customer.phone }}</td>
                <td><button class="btn btn-danger" @click="removeCustomer(customer)">Remove</button></td>
              </tr>
            </tbody>
          </table>
        </p>
      </div>
    </div>

  </div>
</template>

<script>
  import Firebase from 'firebase'
  var env = require('../config/dev.env.js')
  let config = {
    // your firebase config snippet 
    apiKey: env.API_KEY,
    authDomain: env.AUTH_DOMAIN,
    databaseURL: env.DATABASE_URL,
    projectId: env.PROJECT_ID,
    storageBucket: env.STORAGE_BUCKET,
    messagingSenderId: env.MESSAGING_SENDER_ID
  }

  let app = Firebase.initializeApp(config);
  let db = app.database();

  let customersRef = db.ref('customers');

  export default {
    name: 'app',
    firebase: {
      customers: customersRef
    },
    data () {
      return {
        dtCustomer: {
          name: '',
          city: '',
          phone: ''
        }
      }
    },
    methods: {
      addCustomer() {
        customersRef.push(this.dtCustomer)
        this.dtCustomer.name = ''
        this.dtCustomer.city = ''
        this.dtCustomer.phone = ''
      },
      removeCustomer(customer) {
        customersRef.child(customer['.key']).remove()
      }
    }
  }
</script>


