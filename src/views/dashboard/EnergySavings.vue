<template lang="html">
  <v-container fluid grid-list-xs,sm,md,lg,xl>
    <v-layout row wrap>
      <v-flex xs12 sm12 md12 lg12 xl12>
        <span class="subheading secondary--text">Data for the period:</span>
        <v-daterange
          color="secondary"
          class="pr-3"
          v-model="range"
          start-label="Start"
          end-label="End"
          separator-label="to"
          display-format="MM-DD-YYYY"
          :presets="dateRangeOptions.presets"
          :max="dateRangeOptions.maxDate"
        ></v-daterange>
      </v-flex>
      <v-flex xs12 sm6 md6 lg6 xl6>
        <savings-by-facility></savings-by-facility>
      </v-flex>
      <v-flex xs12 sm6 md6 lg6 xl6>
        <savings-by-floor></savings-by-floor>
      </v-flex>
      <v-flex xs12 sm6 md6 lg6 xl6>
        <savings-by-device-type></savings-by-device-type>
      </v-flex>
      <v-flex xs12 sm6 md6 lg6 xl6>
        <savings-by-cluster></savings-by-cluster>
      </v-flex>
    </v-layout>
  </v-container>
</template>
<script>
import MutationTypes from '@/state/mutation-types';
import { format, subDays } from 'date-fns';
import { VDaterange } from 'vuetify-daterange-picker';
import 'vuetify-daterange-picker/dist/vuetify-daterange-picker.css';
import SavingsByFacility
  from '@/components/widgets/analytics/savings/SavingsByFacility.vue';
import SavingsByFloor
  from '@/components/widgets/analytics/savings/SavingsByFloor.vue';
import SavingsByDeviceType
  from '@/components/widgets/analytics/savings/SavingsByDeviceType.vue';
import SavingsByCluster
  from '@/components/widgets/analytics/savings/SavingsByCluster.vue';

export default {
  name: 'energy-savings-view',
  components: {
    VDaterange,
    SavingsByFacility,
    SavingsByFloor,
    SavingsByDeviceType,
    SavingsByCluster,
  },
  mounted() {
    // TODO Dispatch an event to either get data from Store or Pull data from Server
  },
  data() {
    return {
      dateRangeOptions: {
        maxDate: format(subDays(new Date(), 1), 'YYYY-MM-DD'),
        format: 'MM/DD/YYYY',
        presets: [
          {
            label: 'Yesterday',
            range: [
              format(subDays(new Date(), 1), 'YYYY-MM-DD'),
              format(subDays(new Date(), 1), 'YYYY-MM-DD'),
            ],
          },
          {
            label: 'Last Week',
            range: [
              format(subDays(new Date(), 7), 'YYYY-MM-DD'),
              format(subDays(new Date(), 1), 'YYYY-MM-DD'),
            ],
          },
          {
            label: 'Last 30 Days',
            range: [
              format(subDays(new Date(), 30), 'YYYY-MM-DD'),
              format(subDays(new Date(), 1), 'YYYY-MM-DD'),
            ],
          },
        ],
      },
    };
  },
  computed: {
    range: {
      get() {
        return this.$store.getters.analyticsDateRange;
      },
      set(range) {
        this.$store.commit(MutationTypes.SET_ANALYTICS_DATE_RANGE, range);
      },
    },
  },
};
</script>

<style lang="css" scoped></style>
