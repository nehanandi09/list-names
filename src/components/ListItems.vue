<template>
  <div>
    <ul>
      <li v-for="data in newData" :key="data.id">
        <div v-if="data.isEditing">
          <input ref="name" v-model="tempName" />
          <button type="submit" @click="submit(data.id)">Submit</button>
        </div>
        <div v-else>
          <p :ref="data.id" :id="data.id">{{ data.name }}</p>
          <button @click.prevent="copyName(data.id)">Copy Name</button>
          <button v-if="!data.readOnly" @click="editName(data.id)">
            Edit name
          </button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import myData from "../data/data.json";

export default {
  name: "ListItems",
  mounted() {
    myData.forEach((element) => {
      element.isEditing = false;
    });
    this.initialData = myData;
  },
  data() {
    return {
      tempName: "",
      initialData: [],
    };
  },
  computed: {
    newData() {
      let initData = this.initialData;
      let sortedArray = initData.sort((a, b) =>
        a.name.toLowerCase() < b.name.toLowerCase() ? -1 : 1
      );
      return sortedArray;
    },
  },
  methods: {
    copyName(id) {
      let name = document.getElementById(id).innerText;
      navigator.clipboard
        .writeText(name)
        .then(() => {
          alert(name + " has been copied to clipboard");
        })
        .catch((err) => {
          alert("Could not copy text: ", err);
        });
    },
    editName(id) {
      let ind = this.initialData.findIndex((x) => x.id === id);
      this.initialData[ind].isEditing = !this.initialData[ind].isEditing;
    },
    submit(id) {
      let ind = this.initialData.findIndex((x) => x.id === id);
      if (this.tempName !== "") {
        this.initialData[ind].name = this.tempName;
        this.initialData[ind].isEditing = false;
      } else {
        alert("Null string cannot be accepted");
      }
      this.tempName = "";
    },
  },
};
</script>

<style scoped>
ul {
  list-style: none;
}
</style>
