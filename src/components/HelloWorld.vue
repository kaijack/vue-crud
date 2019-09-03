<template>
  <div class="container">
    <br />
    <div class="row">
      <div class="col">
        <h3>List Product</h3>
      </div>
      <div class="col">
        <b-button class="float-right" variant="info">Action</b-button>
      </div>
    </div>

    <br />
    <div>
      <div>
        <form @submit.prevent="add">
          <input type="hidden" v-model="form.product_id" />
          <input type="text" v-model="form.product_name" />
          <button type="submit" v-show="!updateSubmit">add</button>
          <button type="button" v-show="updateSubmit" @click="update(form)">Update</button>
        </form>
        <ul>
          <li v-for="user in users" :key="user.product_id">
            <span>{{user.product_name}}</span> &#160;
            <button @click="edit(user)">Edit</button> ||
            <button @click="del(user)">Delete</button>
          </li>
        </ul>
      </div>
    </div>
    <br />
  </div>
</template>

<script>
/* eslint-disable */

import axios from "axios";
export default {
  data() {
    return {
      form: {
        product_id: "",
        product_name: ""
      },
      users: "",
      updateSubmit: false
    };
  },
  mounted() {
    this.load();
  },
  methods: {
    load() {
      axios
        .get("http://localhost:3000/api/products")
        .then(res => {
          this.users = res.data.response;
        })
        .catch(err => {
          console.log(err);
        });
    },
    add() {
      axios.post("http://localhost:3000/api/products/", this.form).then(res => {
        this.load();
        this.form.product_name = "";
      });
    },
    edit(user) {
      this.updateSubmit = true;
      this.form.product_id = user.product_id;
      this.form.product_name = user.product_name;
    },
    update(form) {
      return axios
        .put("http://localhost:3000/api/products/" + form.product_id, {
          product_name: this.form.product_name
        })
        .then(res => {
          this.load();
          this.form.product_id = "";
          this.form.product_name = "";
          this.updateSubmit = false;
        })
        .catch(err => {
          console.log(err);
        });
    },
    del(user) {
      axios
        .delete("http://localhost:3000/api/products/" + user.product_id)
        .then(res => {
          this.load();
          let index = this.users.indexOf(form.product_name);
          this.users.splice(index, 1);
        });
    }
  }
};
</script>