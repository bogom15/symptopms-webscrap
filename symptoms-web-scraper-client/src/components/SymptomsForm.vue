<template>
  <div class="container h-100">
    <div class="row">
      <div class="col-md-8 mx-auto">
        <div class="card">
          <div class="card-body">
            <h4 class="card-title fs-5 fw-bold">Please explain your symptoms</h4>
            <form @submit.prevent="symptomsPost">
              <div class="container">
                <div class="row mt-5 mb-3">
                  <textarea class="form-control" v-model="userText" rows="5" required></textarea>
                </div>
                <div class="row" v-html="symptomsHtml"></div>
              </div>
              <div class="mb-3">
                <button onclick="" type="submit" class="btn">Submit</button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: "SymptompsForm",
  data(){
    return {
      userText: '',
      symptomsHtml: ''
    }
  },
  methods: {
    containsAnyLetter(str) {
      return /[a-zA-Z]/.test(str);
    },

    async symptomsPost(){
      const userText = this.userText
      if(this.containsAnyLetter(userText))
      {
        const formData = {content : userText}
        const headers = {"Content-Type": "application/json",'Accept': 'application/json'}

        await axios.post('http://localhost:3000/filter-symptoms/',formData, { headers })
            .then(res=>this.highLightText(res.data.response))
            .catch(error=>console.log(error))
      }
    },

    highLightText(responseList){
      let displayText = '<div class="card px-0"><p class="font-weight-lighter fs-5 text-start px-3">'
      let foreachText = this.userText
      responseList.forEach((responseItem)=>{
        foreachText = foreachText.split(responseItem).join(' <span class="badge bg-warning text-dark">'+responseItem+'</span> ')
      })
      displayText += foreachText
      displayText += '</p></div>'
      this.symptomsHtml = displayText
    },



  }//methods

}//export
</script>

<style scoped>
@import url(https://fonts.googleapis.com/css?family=Open+Sans);

*, *::before, *::after {
  box-sizing: border-box;
}

body {
  margin: 30px;
  background-color: #f0f0f0;
}

textarea {
  padding: 10px;
  font: 20px/28px 'Open Sans', sans-serif;
  letter-spacing: 1px;
}


textarea {
  display: block;
  z-index: 2;
  margin: 0;
  border: 2px solid #74637f;
  border-radius: 0;
  color: #444;
  background-color: transparent;
  overflow: auto;
  resize: none;
  transition: transform 1s;
}


button {
  display: block;
  width: 50%;
  margin: 30px auto 0;
  padding: 10px;
  border: none;
  border-radius: 6px;
  color: #fff;
  background-color: #74637f;
  font: 18px 'Opens Sans', sans-serif;
  letter-spacing: 1px;
  appearance: none;
  cursor: pointer;
}
</style>