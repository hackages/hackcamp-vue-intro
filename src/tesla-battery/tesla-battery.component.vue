<template>
  <form class="tesla-battery">
    <!-- TODO 1: Interpolate the title here  -->
    <h1>title</h1>

    <!-- TODO 2: create a Vue component from the div below this component and use it here  -->
    <div class="tesla-car">
      <div class="tesla-wheels">
        <div :class="`tesla-wheel tesla-wheel--front tesla-wheel--${wheels}--${speed}`"></div>
        <div :class="`tesla-wheel tesla-wheel--rear tesla-wheel--${wheels}--${speed}`"></div>
      </div>
    </div>

    <tesla-stats :stats="stats" />
    <div class="tesla-controls cf">
      <!-- TODO 3: The component below needs to have a speed value bound to it, find how you can bind values  -->
      <tesla-counter title="Speed"
                     unit="kmh"
                     :step="5"
                     :min="45"
                     :max="70"
                      />
      <div class="tesla-climate cf">
        <tesla-counter title="Outside Temperature"
                       unit="°"
                       :step="10"
                       :min="-10"
                       :max="40"
                       v-model="temperature" />
        <!-- TODO 4: attach the function to change the climate here  -->
        <tesla-climate :limit="temperature > 10"
                       :value="climate"
                       :onClick="() => {}" />
      </div>
      <tesla-wheels v-model="wheels" />
    </div>
    <div class="tesla-battery__notice">
      <p>
        The actual amount of range that you experience will vary based on your particular use conditions. See how particular use conditions may affect your range in our simulation model.
      </p>
      <p>
        Vehicle range may vary depending on the vehicle configuration, battery age and condition, driving style and operating, environmental and climate conditions.
      </p>
    </div>
  </form>
</template>

<script>
import TeslaClimate from './components/tesla-climate.component';
import TeslaCounter from './components/tesla-counter.component';
import TeslaStats from './components/tesla-stats.component';
import TeslaWheels from './components/tesla-wheels.component';

import teslaService from './tesla-battery.service';

export default {
  name: 'tesla-battery',
  components: {
    TeslaClimate,
    TeslaCounter,
    TeslaStats,
    TeslaWheels,
  },
  created() {
    this.metrics = teslaService.getModelData();
  },
  data() {
    return {
      title: 'Ranger Per Charge',
      models: ['60', '60D', '75', '75D', '90D', 'P100D'],
      speed: 55,
      temperature: 20,
      climate: true,
      wheels: 19,
      metrics: [],
    };
  },
  computed: {
    stats() {
      return this.models.map(model => {
        const miles = this.metrics[model][this.wheels][
          this.climate ? 'on' : 'off'
        ].speed[this.speed][this.temperature];
        return {
          model,
          miles,
        };
      });
    },
  },
  methods: {
    changeClimate() {
      //TODO 5: toggle the climate property from the data object
    },
  },
};
</script>
