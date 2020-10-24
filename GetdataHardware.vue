 created() {
    this.getTemp();
    this.setIntervalId = setInterval(() => {
      this.getTemp();
    }, 3000);
  },
  beforeDestroy() {
    clearInterval(this.setIntervalId);
  },
  

  methods: {
    
    async getTemp() {
      // this.loading = true;
      try {
        const dht = await axios.get("http://192.168.137.23/getscale");
        this.temp = dht.data.temp;
        this.hum = dht.data.hum;
        

        this.loading = false;
        console.log("temp:");
        console.log(this.temp);
        console.log("Hum");
        console.log(this.hum);
      } catch (err) {
        console.log(err); // handle errors here...
      }
    },
 
