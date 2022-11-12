<!-- Reusable component representing an interface for freetreacts -->

<template>
  <article
    class="reactbar"
  >
    <div class="bar">
      <button 
        class="reactbutton"
        title="Agree"
        @click="addReact(1);"
      >
        {{ $store.state.reacts.filter(react => react.freetId === freetId && react.value === 1).length }}
        👍🏽
      </button>
      <button 
        class="reactbutton"
        title="Disagree"
        @click="addReact(2)"
      >
        {{ $store.state.reacts.filter(react => react.freetId === freetId && react.value === 2).length }}
        👎🏽
      </button>
      <button
        class="reactbutton"
        title="Send Love"
        @click="addReact(3)"
      >
        {{ $store.state.reacts.filter(react => react.freetId === freetId && react.value === 3).length }}
        💞
      </button>
      <button
        class="reactbutton"
        title="Sad"
        @click="addReact(4)"
      >
        {{ $store.state.reacts.filter(react => react.freetId === freetId && react.value === 4).length }}
        😭
      </button>
      <button 
        class="reactbutton"
        title="Angry"
        @click="addReact(5)"
      >
        {{ $store.state.reacts.filter(react => react.freetId === freetId && react.value === 5).length }}
        😡
      </button>
      <button 
        class="reactbutton"
        title="Interesting!"
        @click="addReact(6)"
      >
        {{ $store.state.reacts.filter(react => react.freetId === freetId && react.value === 6).length }}
        💡
      </button>
      <section class="alerts">
        <article
          v-for="(status, alert, index) in alerts"
          :key="index"
          :class="status"
        >
          <p>{{ alert }}</p>
        </article>
      </section>
    </div>
  </article>
</template>
  
  <script>
  export default {
    name: 'ReactBar',
    props: {
      // freetId of the freet the reactbar is attached to
      freetId: {
        type: String,
        required: true
      }
    },
    data() {
      return {
        alerts: {}, // Displays success/error messages encountered during freet modification
        reacts: this.$store.state.reacts
      };
    },
    computed: {
      console: () => console,
      window: () => window
    },

    methods: {
      addReact(value) {
        /**
         * Performs react logic with the button in question
         */
         const params = {
          method: 'POST',
          body: JSON.stringify({reactValue: value}),
          callback: () => {
            this.$store.commit('alert', {
              message: 'Successfully added react!', status: 'success',
            });
          }
        };
        this.request(params);
      },
      async request(params) {
        /**
         * Submits a request to the freetreact's endpoint
         * @param params - Options for the request
         * @param params.body - Body for the request, if it exists
         * @param params.callback - Function to run if the the request succeeds
         */
        const options = {
          method: params.method, headers: {'Content-Type': 'application/json'}
        };
        if (params.body) {
          options.body = params.body;
        }
        console.log(options);
        try {
         const r = await fetch(`/api/freetreacts/${this.freetId}`, options);
          if (!r.ok) {
            console.log('uh oh');
            const res = await r.json();
            throw new Error(res.error);
          }
          this.$store.commit('refreshReacts');
          params.callback();
        } catch (e) {
          this.$set(this.alerts, e, 'error');
          setTimeout(() => this.$delete(this.alerts, e), 3000);
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .freet {
      border: 1px solid #111;
      padding: 20px;
      position: relative;
  }
  </style>