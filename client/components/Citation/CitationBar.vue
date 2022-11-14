<!-- Reusable component representing the citations on a freet -->

<template> 
  <article
    class="citationbar"
  >
    <div class="bar">
      <div
        v-if="$store.state.citations.filter(citation => citation.freetId === freetId).length === 0"
        class="existCitations"
      >
        No citations for this freet.
      </div>
      <div
        v-else
        ref="citationHeader"
        class="existCitations"
        @click="toggleCitations"
      >
        Expand Citations
      </div>
      <div
        v-if="citeView"
      >
        <div
          v-for="citation in $store.state.citations.filter(citation => citation.freetId === freetId)"
          :key="citation._id"
          :title="citation.url"
          class="oneCite"
        >
          <span 
            v-if="owner"
            class="cite remove"
            cursor="pointer"
            @click="removeCitation(citation._id)"
          >
            remove
          </span>
          <span 
            class="cite"
            @click="openTab(citation.url)"
          >
            📜 {{ citation.url }}
          </span>
        </div>
      </div>
    </div>
  </article>
</template>
    
    <script>
    export default {
      name: 'CitationBar',
      props: {
        // freetId of the freet the reactbar is attached to
        freetId: {
          type: String,
          required: true
        },
        owner: {
            type: Boolean,
            required: true
        }
      },
      data() {
        return {
          alerts: {}, 
          citations: this.$store.state.citations,
          citeView: false
        };
      },
      computed: {
        console: () => console,
        window: () => window
      },
  
      methods: {
        openTab(url) {
            window.open(url, '_blank');
        },
        toggleCitations() {
            this.citeView = !this.citeView;
            const header = this.$refs.citationHeader;
            if (this.citeView) {
                header.innerHTML = 'Collapse Citations';
            }
            else {
                header.innerHTML = 'Expand Citations';
            }

        },
        

        async removeCitation(citationId) {
          const params = {
            method: 'DELETE',
            body: JSON.stringify({citationId: citationId}),
            headers: {'Content-Type': 'application/json'},
            callback: () => {
              this.$store.commit('alert', {
                message: 'Successfully removed citation.', status: 'success',
              });
            }
          };
          try {
           const r = await fetch(`/api/cite/${citationId}`, params);
            if (!r.ok) {
              const res = await r.json();
              throw new Error(res.error);
            }
            this.$store.commit('refreshCitations');
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
    .cite:hover {
        cursor: pointer;
    }
    .remove {
        font-size: 12px;
        text-decoration: underline;
        color: purple;
    }
    .existCitations {
        color: green;
    }

    </style>