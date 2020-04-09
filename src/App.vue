<template>
  <div id="app">

  <form @submit.prevent="save">
  <input type="hidden" v-model="form.id"> 
  <input type="text" v-model="form.full_name"> <br>
  <button type="submit" v-show="!updateSubmit" name="button">Simpan</button>
  <button type="submit" v-show="updateSubmit" @click="update(form)" name="button">Update</button>
  </form>

    <h4>List</h4>
    <ul v-for="customer in customers" :key="customer.id"> 
    <li>{{ customer.full_name }}
    <button type="submit" @click="edit(customer)" name="button">Edit</button>
    <button type="submit" name="button">Delete</button>
    </li>
    </ul>

  </div>
</template>
<script>
import axios from 'axios';
export default {
  data() {
    return {
      form:{
        id:'',
        full_name:''
      },
      customers:[],
      updateSubmit: false
    }
  },
  methods: {
    load(){
      axios.get('http://localhost:3000/customers') 
      .then((res) =>{ 
        this.customers = res.data 
        })
      .catch(()=>{
        alert('error load data')
      })
    },
    save(){
      axios.post('http://localhost:3000/customers', this.form)
      .then(() =>{ 
        this.load()
        this.form.full_name =''
        alert('saving...') 
        })
      .catch((err)=>{
        alert(err)
      })
    },
    edit(customer){
      this.updateSubmit = true,
      this.form.id= customer.id,
      this.form.full_name = customer.full_name
    },
    update(form){
      axios.put('http://localhost:3000/customers/' + form.id ,{
        full_name: this.form.full_name
      })
      .then(() =>{ 
        this.load()
        this.form.full_name =''
        this.updateSubmit = false
        alert('updated...') 
        })
      .catch(()=>{
        alert('updated eror...')
      })
    }
  },
  mounted() {
    this.load()
  },
}
</script>

<style>
</style>
