<template>
  <span class="date-display" :class="privacyMode ? 'blur-content' : null">
    {{ displayTimestamp | formatDate(dateDisplayFormat) }}
  </span>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import { mapGetters, mapState } from 'vuex';

@Component({
  computed: {
    ...mapGetters('session', ['dateDisplayFormat']),
    ...mapState('session', ['privacyMode', 'scrambleData'])
  }
})
export default class DateDisplay extends Vue {
  @Prop({ required: true, type: Number })
  timestamp!: number;

  dateDisplayFormat!: string;
  privacyMode!: boolean;
  scrambleData!: boolean;

  get displayTimestamp(): number {
    if (!this.scrambleData) {
      return this.timestamp;
    }
    const start = new Date(2016, 0, 1).getTime();
    const now = Date.now();
    return new Date(start + Math.random() * (now - start)).getTime() / 1000;
  }
}
</script>

<style scoped lang="scss">
.blur-content {
  filter: blur(0.75em);
}
</style>
