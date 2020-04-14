<template>
  <DxPieChart
    id="pie"
    :data-source="areas"
    palette="Bright"
    title="Casos de covid-19 no Brasil"
    @point-click="pointClickHandler($event)"
    @legend-click="legendClickHandler($event)"
  >
    <DxSeries argument-field="state" value-field="count">
      <DxLabel :visible="true">
        <DxConnector :visible="true" :width="10" />
      </DxLabel>
    </DxSeries>
    <DxSize />
    <DxExport :enabled="true" />
  </DxPieChart>
</template>

<script>
import DxPieChart, {
  DxSize,
  DxSeries,
  DxLabel,
  DxConnector,
  DxExport
} from "devextreme-vue/pie-chart";

import api from "./plugins/axios";

export default {
  components: {
    DxPieChart,
    DxSize,
    DxSeries,
    DxLabel,
    DxConnector,
    DxExport
  },
  data() {
    return {
      areas: []
    };
  },
  methods: {
    pointClickHandler(e) {
      this.toggleVisibility(e.target);
    },
    legendClickHandler(e) {
      let arg = e.target,
        item = e.component.getAllSeries()[0].getPointsByArg(arg)[0];

      this.toggleVisibility(item);
    },
    toggleVisibility(item) {
      item.isVisible() ? item.hide() : item.show();
    },
    async carregar() {
      const { data } = await api.get("");
      this.areas = data.infectedByRegion;
    }
  },
  mounted() {
    this.carregar();
  }
};
</script>

<style>
#pie {
  height: 700px;
}

#pie * {
  margin: 0 auto;
}
</style>
