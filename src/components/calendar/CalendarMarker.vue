<template>
  <v-col class="d-flex justify-center pa-3">
    <v-icon
      class="calendar-marker"
      :color="iconColor"
      @click="mark"
      role="button"
      >{{ icon }}
    </v-icon>
  </v-col>
</template>

<script>
import ICON from "@/common/constants/icon";
import { mapGetters, mapActions } from "vuex";

export default {
  name: "CalendarMarker",
  props: {
    markerIcon: String,
    label: String,
    color: Number
  },
  computed: {
    ...mapGetters(["getDates"]),
    icon() {
      return ICON[this.markerIcon.toUpperCase()];
    },
    iconColor() {
      let todayMark = this.getDates.filter(
        d =>
          d.dates.getDate() === new Date().getDate() &&
          d.popover.label === this.label
      );
      return todayMark.length > 0 ? todayMark[0].dot.color : "";
    }
  },
  methods: {
    ...mapActions(["fetchDates", "addDate", "removeDate"]),
    addMark() {
      this.addDate({ color: this.color, label: this.label });
    },
    removeMark() {
      this.removeDate({ label: this.label });
    },
    mark() {
      if (this.iconColor) this.removeMark();
      else this.addMark();
    }
  }
};
</script>

<style scoped>
.calendar-marker {
  width: 44px;
  height: 44px;
  background-position: center;
}
</style>
