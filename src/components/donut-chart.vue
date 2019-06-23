<template>
  <div :id="id"></div>
</template>

<script>
import "morris.js/morris";
import "morris.js/morris.css";
import Converter from "../util/converter";
import ChartProps from "./chart-mixins";

export default {
  name: "donut-chart",

  mixins: [ChartProps.donut],

  data() {
    return {
      chart: null
    };
  },

  watch: {
    data(val) {
      this.$nextTick(() => {
        if (this.data && this.data.length > 0 && !this.chart) {
          this.createChart();
        }

        if (this.chart) {
          if (this["colors"]) {
            this.chart.options["colors"] = Converter.toObject(this["colors"]);
          }

          this.chart.setData(Converter.toObject(this.data));
        }
      });
    }
  },

  mounted() {
    if (this.data && this.data.length > 0) {
      this.createChart();
    }
  },

  methods: {
    createChart() {
      let options = {
        element: this.id,
        data: Converter.toObject(this.data),
        resize: Converter.toBoolean(this.resize),
        donutType: this.donutType,
        dataLabels: Converter.toBoolean(this.dataLabels),
        dataLabelsPosition: this.dataLabelsPosition,
        showPercentage: Converter.toBoolean(this.showPercentage)
      };

      this.addOptionAsObject("colors", options);
      this.addOption("formatter", options);
      this.addOption("donutType", options);
      this.addOption("dataLabels", options);
      this.addOption("dataLabelsPosition", options);
      this.addOption("showPercentage", options);

      this.chart = Morris.Donut(options);
    }
  }
};
</script>
