<template>
  <form @submit.prevent="translateIt" class="well">
    <textarea
      v-model="searchText"
      cols="30"
      rows="5"
      class="form-control"
      placeholder="Çevirmek istediğiniz kelime/cümle yazınız."
    ></textarea>
    <!-- <p>{{ searchText }}</p> -->
    <select class="form-control" v-model="translateTo">
      <option v-for="(lang, key) in languages" :value="key" :key="lang.id">{{lang}}</option>
    </select>
    <br />
    <div class="text-left">
      <strong>Çevrilecek Dil : </strong> {{languages[translateTo]}}
    </div>
    <br />
    <button type="submit" class="btn btn-primary btn-block">
      Çevir Gelsin!
    </button>
  </form>
</template>

<script>
import axios from "axios"
export default {
  data() {
    return {
      searchText : "",
      languages : {},
      translateTo : ""
    }
  },
  methods : {
      translateIt(){
        //   alert(this.searchText)
        // alert(this.translateTo)
        // let url = "https://translate.yandex.net/api/v1.5/tr.json/translate?key=trnsl.1.1.20200216T202741Z.b1db3e1a124036e8.40cabf0d744bc86212f77547f02ceb5c26086076&text=" + this.searchText + "&lang=en";
        let url = "https://translate.yandex.net/api/v1.5/tr.json/translate?key=trnsl.1.1.20200216T202741Z.b1db3e1a124036e8.40cabf0d744bc86212f77547f02ceb5c26086076&text=" + this.searchText + "&lang=" + this.translateTo;
axios.get(url)
    .then(response => {
        // console.log(response)

        //historye veri göndermek için
        // console.log(response)
        //şimdi burada bize lazım olan bilgilerden ilki diller
        // // response.data.lang bize lang: "tr-en" veriyor bunu split edelim
        // response.data.lang.split("-")[0] bu bize ilk elemanın yani fromLang ın kısaltmasını verir, bunu this.Languages[xxxx] ile kullanırsak dilin kendisini alırız
 
        let translatePack = {
            fromLang : this.languages[response.data.lang.split("-")[0]],
            toLang : this.languages[this.translateTo],
            fromText : this.searchText,
            toText : response.data.text[0]
        }
console.log(translatePack);

        // this.$emit("translatedEvent", response.data.text[0])
        //şimdi bu translatePack i emitleyeceğiz ki App.vue da yakalanabilsin
        this.$emit("translatedEvent", translatePack)
        this.searchText = ""
    })
    .catch(err => console.log(err))
      }
  },
  created(){
    //   alert("created açlıştı")
      axios.get("https://translate.yandex.net/api/v1.5/tr.json/getLangs?key=trnsl.1.1.20200216T202741Z.b1db3e1a124036e8.40cabf0d744bc86212f77547f02ceb5c26086076&ui=tr")
      .then(response => {
        //   console.log(response)
        //   console.log(response.data.langs)
          this.languages = response.data.langs
      })
  }
};
</script>

<style></style>
