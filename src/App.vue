<template>
  <div class="bkg" :style="bkgStyle">
    <template v-if="status == 'whitelist'" >
      <div v-for="position in emojiPositions" class="party-emoji" :style="styleEmoji(position)" :key="position.key">
        🎉 
      </div>
    </template>
    <div id="app" class="grid-container">
      <div class="input-group">
        <input class="address-input" type="text" v-model="address" placeholder="Enter an address" />
        <button @click="validateAddress" style="margin-bottom: 16px">Submit</button>
        <div v-if="status != ''" class="status">
          <div v-if="status == 'whitelist'">Congrats, it's time to PARTY! You made it on the whitelist!</div>
          <div v-if="status == 'waitlist'">Hey OG, you did not get whitelisted, but you're on the waitlist if anyone on the WL misses the mint!</div>
          <div v-if="status == 'reject'">Sorry farmor, this wallet isn't whitelisted</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

function randRange(a, b) {
  return Math.random() * (b - a) + a;
}

function generateEmojiPositions(){
  return new Array(30).fill(null).map(() => ({ 
  'top': randRange(0, window.innerHeight-100), 
  'left': randRange(0, window.innerWidth-100), 
  'key': Math.random()
  }));
}


export default {
  name: 'App',
  data() {
    return {
      address: '',
      status: '', // 'whitelist', 'waitlist', 'reject'
      whitelisted: [
'sei1234',
'sei12345',
'whitelist',
      ],
      waitlisted: [
'sei23554',
'sei49590',
'waitlist'
        ],
      emojiPositions : generateEmojiPositions()
    };
  },
  computed: {
    bkgStyle(){
      if (this.status == 'reject'){
        return {  filter: 'grayscale(100%)'};
      }
      return {};
    }
  },
  methods: {
    validateAddress() {
      const address = this.address.trim();
      if(this.whitelisted.includes(address)){
        this.status = 'whitelist';
      } else if (this.waitlisted.includes(address)){
        this.status = 'waitlist';
      } else {
        this.status = 'reject';
      }
    },
    styleEmoji(position){
      return {
        'top': position.top + 'px',
        'left': position.left + 'px'
      }
    }
  },
};
</script>

<style>
/* Remove default margins and set the custom background */
html, body, #app {
  margin: 0;
  padding: 0;
  height: 100%;
  font-family: sans-serif ;
}
.party-emoji {
  position:absolute;
  font-size: 4em;
  z-index: 1;
}
.bkg {
height: 100%; /* Fill the entire viewport vertically */
width: 100%; /* Fill the entire viewport horizontally */
background-image: url('./assets/desktop.png');
background-repeat: repeat;
}
.grid-container {
  display: grid;
  place-items: center; /* Shortcut for align-items and justify-items */
}
.grid-container *{
  z-index: 2;
}
.status {
  text-align: center;
  line-height: 1.2em;
  font-weight: bold;
  background-color: #f1f0eb;
  padding: 8px 10%;
  border: 4px solid #000;
  max-width: 400px;
}
.input-group {
  display: flex;
  flex-direction: column; /* Adjusted to column layout */
  align-items: center; /* Center items horizontally */
}
.input-group input[type="text"], .input-group button {
  padding: 10px;
  font-size: 16px;
  margin: 5px 0; /* Add a little space between elements */
  border: 2px solid #ccc;
  border-radius: 4px;
  width: 80%; /* Adjust width to look better on column layout */
}
.input-group button {
  background-color: #4CAF50; /* Green */
  color: white;
  border: none;
  max-width: 120px;
}
/* Add classes for valid and invalid input states */
input.valid {
  border-color: green;
}
input.invalid {
  border-color: red;
}
.address-input:focus{
  border-color: limegreen !important;
}
.address-input{
  min-width: 240px;
  text-align: center;
}
.address-input::placeholder{
  text-align: center;
}
/* Style for the validation message */
.validation-message {
  margin-top: 20px;
  font-size: 16px;
}
.success {
  color: green;
}
.error {
  color: red;
}

</style>