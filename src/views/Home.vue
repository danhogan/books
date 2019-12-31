<template>
  <v-content>
      <v-text-field v-model="bookTitle" :label="'Title'"></v-text-field>
      <button @click="doSearch">Search</button>
    <v-data-table :headers="headers" :items="returnedData" :items-per-page="10" class="elevation-1">
        <template v-slot:item.test="{ item }">
          <img v-if="item.fields.thumbnail" :src="item.fields.thumbnail.content[0].uri">
        </template>
    </v-data-table>
  </v-content>
</template>

<script>
import axios from "axios";

export default {
  name: "home",
  data() {
    return {
      headers: [
        { text: "Title", value: "fields.titel.content.value" },
        { text: "Author", value: "fields.auteur.content[0].label" },
        { text: "Image", value: "test" },
        { text: "Publish Year", value: "fields.pubjaar.content.label" },
      ],
      returnedData: [],
      bookTitle: "test"
    };
  },
  methods: {
      doSearch(){
          let self = this;

            var bodyFormData = new FormData();
            bodyFormData.set("qs", self.bookTitle);
            bodyFormData.set("prt", "INTERNET");
            bodyFormData.set("var", "portal");
            bodyFormData.set("vestnr", "6000");
            bodyFormData.set("fmt", "json");
            bodyFormData.set("search_in", "iets");
            bodyFormData.set("catalog", "default");
            bodyFormData.set("amount", "10");
            bodyFormData.set("event", "osearch");
            bodyFormData.set("offset", "0");
            bodyFormData.set("sortkey", "titel");
            bodyFormData.set("vcgrpf", "0");
            bodyFormData.set("backend", "wise");
            bodyFormData.set("vcgrpt", "0");
            bodyFormData.set("preset", "all");

            axios({
            method: "post",
            url: "https://acpl.wise.oclc.org//cgi-bin/bx.pl",
            data: bodyFormData,
            headers: { "Content-Type": "multipart/form-data" }
            })
            .then(function(response) {
                console.log(response);
                console.log(response.data.objects);
                self.returnedData = response.data.objects;
            })
            .catch(function(response) {
                console.log(response);
            });
      }
  },
  created() {
    this.doSearch()
  }
};
</script>
