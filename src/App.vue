<template>
<div class="container">
Message :   <textarea v-model="message" type="text" />
To (separated by line break) :   <textarea v-model="to" type="text" />
<button v-on:click="()=>{send_sms(message,to)}" >Send</button>
</div>
</template>

<script>
const axios = require('axios').default;
export default {
  name: 'App',
  data() {
    return {
      message: "type your message here",
      to: "0691049548"
    }
  },

  methods: {
    send_sms:  async (message, to)  => {

      const toArray = to.split(/\r?\n|\r|\n/g).map(x =>  {return{ 'to': '+212' + x.substring(1) }});
   
      const data = JSON.stringify({
                "messages": [
                    {
                        "destinations": toArray,
                        "from": "Anmodo Tr",
                        "text": message
                    }
                ]
            })

      axios.post( "https://gy28xe.api.infobip.com/sms/2/text/advanced", data, {
          headers: {
              'authorization': 'App 4e435e68ac754b53d7486cf765eb540b-506f41ac-4e01-40a5-b5bf-2f6a593103e5',
              'content-Type': 'application/json',
              'accept': 'application/json'
          },
      })      
      .then((response) => {
        console.log(response)
      })
      .catch((error) => {
          console.log(error)
      })
    }
  },

  mounted () {
    
    // this.send_sms(this.message, this.to)
    
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  margin: 0 auto;
  width: 60%;
  display: flex;
  flex-direction: column;
}

@media only screen and (max-width: 600px) {
  .container{
     width: 85%;
  }
}

textarea{
  height: 100px;
}

button {
  height: 43px;
    width: 200px;
    margin: 18px auto;
}
</style>
