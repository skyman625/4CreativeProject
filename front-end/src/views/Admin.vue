<template>
<div class="admin">
<h1>Upload or edit music recommendations</h1>
<br>
<hr>
<div class="heading">
<img src="/music.png">
<h2>Upload a music recommendation</h2>
</div>
<div class="add">
<div class="form">
  <input v-model="title" placeholder="Album Name">
  <input v-model="artist" placeholder="Artist Name">
  <input v-model="description" placeholder="Comments">
  <p></p>
  <p>Album art:</p>
  <input type="file" name="photo" @change="fileChanged">
  <button @click="upload">Upload</button>
</div>
<div class="upload" v-if="addItem">
  <img src="/check.png">
</div>
</div>
<br>

<br>
<hr>
<div class="heading">
<img src="/music.png">
     <h2>Edit/Remove a recommendation</h2>
   </div>
   <div class="edit">
     <div class="form">
       <input v-model="findTitle" placeholder="Search">
       <div class="suggestions" v-if="suggestions.length > 0">
         <div class="suggestion" v-for="s in suggestions" :key="s.id" @click="selectItem(s)">{{s.title}}
         </div>
       </div>
     </div>
     <div class="upload" v-if="findItem">
       <input v-model="findItem.title">
       <input v-model="findItem.artist">
       <input v-model="findItem.description">
       <p></p>
       <img :src="findItem.path" />
     </div>
     <div class="actions" v-if="findItem">
       <button @click="deleteItem(findItem)">Delete</button>
       <button @click="editItem(findItem)">Edit</button>
     </div>
   </div>
   <br>
   <br>
   <hr>

</div>
</template>

<style scoped>
img {
  height: 30px;
  width: auto;
}
.image h2 {
  font-style: italic;
  font-size: 1em;
}
.image h3 {
  font-style: italic;
  font-size: 0.7em;
  color: #808080;
}

.heading {
  display: flex;
  margin-bottom: 20px;
  margin-top: 20px;
}

.heading h2 {
  margin-top: 8px;
  margin-left: 10px;
}

.add,
.edit {
  display: flex;
}

.circle {
  border-radius: 50%;
  width: 18px;
  height: 18px;
  padding: 8px;
  background: #333;
  color: #fff;
  text-align: center
}

/* Form */
input,
textarea,
select,
button {
  font-family: 'Montserrat', sans-serif;
  font-size: 1em;
}

.form {
  margin-right: 50px;
}

/* Uploaded images */
.upload h2 {
  margin: 0px;
}


.upload img {
  max-width: 300px;
}

/* Suggestions */
.suggestions {
  width: 200px;
  border: 1px solid #ccc;
}

.suggestion {
  min-height: 20px;
}

.suggestion:hover {
  background-color: #5BDEFF;
  color: #fff;
}
</style>

<script>
import axios from 'axios';
export default {
  name: 'Admin',
  data() {
    return {
      title: "",
      artist: "",
      description: "",
      file: null,
      addItem: null,
      items: [],
      findTitle: "",
      findItem: null,
    }
  },
  computed: {
    suggestions() {
      let items = this.items.filter(item => item.title.toLowerCase().startsWith(this.findTitle.toLowerCase()));
      return items.sort((a, b) => a.title > b.title);
    }
  },
  created() {
    this.getItems();
  },
  methods: {
    fileChanged(event) {
      this.file = event.target.files[0]
    },
    async upload() {
      try {
        const formData = new FormData();
        formData.append('photo', this.file, this.file.name)
        let r1 = await axios.post('/api/photos', formData);
        let r2 = await axios.post('/api/items', {
          title: this.title,
          artist: this.artist,
          description: this.description,
          path: r1.data.path
        });
        this.addItem = r2.data;
      } catch (error) {
        console.log(error);
      }
    },
    async getItems() {
      try {
        let response = await axios.get("/api/items");
        this.items = response.data;
        return true;
      } catch (error) {
        console.log(error);
      }
    },
    selectItem(item) {
        this.findTitle = "";
        this.findItem = item;
      },
      async deleteItem(item) {
      try {
        await axios.delete("/api/items/" + item._id);
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
        console.log(error);
      }
    },
    async editItem(item) {
      try {
        await axios.put("/api/items/" + item._id, {
          title: this.findItem.title,
          artist: this.findItem.artist,
          description: this.findItem.description,
        });
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
        console.log(error);
      }
    },
  }
}
</script>
