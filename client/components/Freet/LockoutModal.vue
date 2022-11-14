<template>
  <main>
    <form 
      class="modalform" 
    >
      <!-- Modal content -->
      <div class="modal-content">
        <h3>You have reached your specified time limit for Fritter.</h3>
        <p>We're happy to see you go-- please come back soon!</p>
        <button
          v-if="$store.state.lockout === 'soft'"
          @click="$emit('dismiss')"
        >
          Thanks. Let me keep scrolling!
        </button>
      </div>
    </form>
  </main>
</template>
  
<script>
export default {
    name: 'LockoutModal'
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