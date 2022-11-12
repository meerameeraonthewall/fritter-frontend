<!-- Form for getting freets (all, from user) (inline style) -->

<script>
import InlineForm from '@/components/common/InlineForm.vue';

export default {
  name: 'AddCitationForm',
  mixins: [InlineForm],
  props: {
      // freetId of the freet the citation is attached to
      freetId: {
        type: String,
        required: true
      }
    },
  methods: {
    async submit() {
      const route = `/api/cite/${this.freetId}`;
      const options = {method: 'POST',
      headers: {'Content-Type': 'application/json'},
      body: JSON.stringify({url: this.$data.value}),
      };
      try {
        const r = await fetch(route, options);
        const res = await r.json();
        if (!r.ok) {
          throw new Error(res.error);
        }
        this.$emit('cited');
        this.$store.commit('refreshCitations');
        
      } catch (e) {
        this.$set(this.alerts, e, 'error');
        setTimeout(() => this.$delete(this.alerts, e), 3000);
      }
    }
  }
};
</script>