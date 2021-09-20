<template>
  <div>
    <div class="form-group">
      <label>Name</label>
      <input
        type="text"
        id="name"
        placeholder="enter name"
        class="form-control"
        v-model="item.name"
      />
      <div class="form-group">
        <label>Phone</label>
        <input
          type="text"
          id="phone"
          placeholder="enter name"
          class="form-control"
          v-model="item.phone"
        />
      </div>
      <button @click="save" class="btn btn-primary">Save</button>
      <div class="col-md-8 offset-md-3" v-if="lists.length > 0">
        <h1 class="text-center">Telephone Numbers</h1>
        <ul class="list-group">
          <li class="list-group-item" v-for="item in lists" :key="item.id">
            <b>Name:</b> {{ item.name }} <b>Phone:</b> {{ item.phone }}

            <span class="float-right">
              <button class="btn btn-primary">View</button>
              <button @click="deletetel(item.id)" class="btn btn-danger">Delete</button>
            </span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "Directory",
  data() {
    return {
      lists: [],
      item: {
        name: "",
        phone: "",
      },
      temp_id: null,
      isEditing:false

    };
  },
  mounted() {
    this.fetchall();
  },

  methods: {
    fetchall() {
      axios.get("/api/tel").then((response) => {
        this.lists = response.data;
      });
    },
    save() {
      try {
        axios.post("/api/tel", this.item).then((responce) => {
          this.fetchall();
        });
      } catch (e) {
        console.log(e);
      }
    },
    deletetel(id) {
      try {
        axios.delete(`api/tel/${id}`).then((response) => this.fetchall());
      } catch {
        console.log(e);
      }
    },
    edittel(tel){
        this.item ={
            name:tel.name,
            phone:tel.phone
            
        }
       this.temp_id =tel.name
       
    }
  },
};
</script>
<style scoped>
</style>