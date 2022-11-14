<template>
  <main>
    <form 
      class="modalform"  
      @submit.prevent="getFormValues"
    >
      <!-- Modal content -->
      <div class="modal-content">
        <h3>Welcome to Fritter!</h3>
        <p>How long do you intend to join us right now?</p>
        <label for="time">Minutes:</label>
        <input 
          id="time"
          type="number" 
          name="time" 
          min="1" 
          max="120"
        >

        <p>Please select your preferred lockout method.</p>
        <input 
          id="soft"
          type="radio"  
          name="lockout" 
          value="soft"
        >
        <label for="soft">Soft-- Notify me when my time is up, but let me keep scrolling.
        </label><br>
        <input 
          id="hard"
          type="radio"  
          name="lockout" 
          value="hard"
        >
        <label for="hard">Hard-- Cut me off when my time is up!</label><br>
        <input 
          id="none" 
          type="radio"  
          name="lockout" 
          value="none"
        >
        <label for="none">Don't bother me with these limits!</label>

        <br><br>
        <input 
          type="submit" 
          value="Submit Settings"
          name="submit"
        >
      </div>
    </form>
  </main>
</template>

<script>
  export default {
    name: 'SessionModal',
    data () {
      time: ''
      lockout: ''
    },
    methods: {
      getFormValues (submitEvent) {
        this.time = submitEvent.target.elements.time.value;
        this.lockout =submitEvent.target.elements.lockout.value;
        this.$emit('submit', this.time, this.lockout);
      }
    }
}
</script>

<style scoped>
/* The Modal (background) */
.modal {
  display: none; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0,0,0); /* Fallback color */
  background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

/* Modal Content/Box */
.modal-content {
  background-color: #fefefe;
  margin: 15% auto; /* 15% from the top and centered */
  padding: 20px;
  border: 1px solid #888;
  width: 80%; /* Could be more or less, depending on screen size */
}

/* The Close Button */
.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}
</style>