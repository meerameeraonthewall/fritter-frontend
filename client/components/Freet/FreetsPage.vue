<!-- Default page that also displays freets -->

<template>
  <main>
    <section v-if="$store.state.username">
      <header>
        <h2>Welcome @{{ $store.state.username }}</h2>
      </header>
      <CreateFreetForm />
      <div 
        id="sessionModal" 
        ref="modalDetails"
      >
        <SessionModal 
          @submit="submitModalData"
        />
      </div>
      <div
        id="lockoutModal"
        ref="lockoutModal"
      >
        <LockoutModal 
          @dismiss="dismissModal"
        />
      </div>
    </section>
    <section v-else>
      <header>
        <h2>Welcome to Fritter!</h2>
      </header>
      <article>
        <h3>
          <router-link to="/login">
            Sign in
          </router-link>
          to create, edit, and delete freets.
        </h3>
      </article>
    </section>
    <section>
      <header>
        <div class="left">
          <h2>
            Viewing all freets
            <span v-if="$store.state.filter">
              by @{{ $store.state.filter }}
            </span>
          </h2>
        </div>
        <div class="right">
          <GetFreetsForm
            ref="getFreetsForm"
            value="author"
            placeholder="🔍 Filter by author (optional)"
            button="🔄 Get freets"
          />
        </div>
      </header>
      <section
        v-if="$store.state.freets.length"
      >
        <FreetComponent
          v-for="freet in $store.state.freets"
          :key="freet.id"
          :freet="freet"
        />
      </section>
      <article
        v-else
      >
        <h3>No freets found.</h3>
      </article>
    </section>
  </main>
</template>

<script>
import FreetComponent from '@/components/Freet/FreetComponent.vue';
import CreateFreetForm from '@/components/Freet/CreateFreetForm.vue';
import GetFreetsForm from '@/components/Freet/GetFreetsForm.vue';
import SessionModal from '@/components/Freet/SessionModal.vue';
import LockoutModal from '@/components/Freet/LockoutModal.vue';

export default {
  name: 'FreetPage',
  components: {FreetComponent, GetFreetsForm, CreateFreetForm, SessionModal, LockoutModal},
  computed: {
        console: () => console,
        window: () => window
      },
  mounted() {
    this.$refs.getFreetsForm.submit();
    this.$store.commit('refreshReacts');
    this.$store.commit('refreshCitations');
    const localStorage = window.sessionStorage;
    // Get the modal
    const modal = document.getElementById("sessionModal");

    // Following code adapted from https://stackoverflow.com/questions/32053505/run-javascript-once-per-session
    if ((typeof localStorage !== 'undefined') &&
    (localStorage.getItem('expiry') === null)) {

      modal.style.display = "block";
    }
  },
  methods: {
      submitModalData(time, lockout) {
        const modal = document.getElementById("sessionModal");
        const localStorage = window.sessionStorage;
        modal.style.display="none";
        const expiry = Date.now() + (Number(time)*60000);
        this.$store.state.time = expiry;
        this.$store.state.lockout = lockout;
        localStorage.setItem('expiry', true);
        if (this.$store.state.lockout !== "none") {
          this.countdown();
        }
      },
      // Following adapted from https://stackoverflow.com/questions/62964228/how-do-i-display-a-live-countdown-then-close-the-window
      async countdown() {
        const endTime = this.$store.state.time;
        while (Date.now() < endTime) {
          await new Promise(resolve => setTimeout(resolve, 1000));
          console.log('countdown again');
        }
        
        const modal = this.$refs.lockoutModal;
        modal.style.display = "block";
      },
      dismissModal() {
        const modal = this.$refs.lockoutModal;
        modal.style.display="none";
      }
  }
};
</script>

<style scoped>
section {
  display: flex;
  flex-direction: column;
}

header, header > * {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

button {
    margin-right: 10px;
}

section .scrollbox {
  flex: 1 0 50vh;
  padding: 3%;
  overflow-y: scroll;
}
#sessionModal {
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
#lockoutModal {
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
</style>
