<template>
  <div class="calendar-container d-inline-flex pa-0">
    <VueCal
      :time-from="5 * 60"
      :time-to="21 * 60"
      :events="schedules"
      :selected-date="date" 
      active-view="week"
      @view-change="changeFilter"
    >
    </VueCal>
  </div>
</template>

<script>
import VueCal from 'vue-cal'
import 'vue-cal/dist/vuecal.css'

export default {
  components: {
    VueCal
  },
  data: () => ({
    date: new Date(),
    schedules: []
  }),
  mounted () {
    this.fetchData()
    this.$root.$on('loadEvents', this.fetchData)
  },
  methods: {
    async fetchData () {
      try {
        let month = this.date.getMonth()
        let { data } = await this.axios.get(`http://localhost:8000/api/schedules/${(month ? month + 1 : 'current')}`)
        this.schedules = data.schedules
      } catch (error) {
        this.schedules = []
      }
      this.$forceUpdate()
    },
    changeFilter ({ startDate }) {
      this.date = startDate
      this.fetchData()
    }
  },
  computed: {
  }
}
</script>

<style>
.calendar-container {
  width: 100%;
}
.vuecal__event {
  background-color: #dfd987;
}
.vuecal__body {
  background-color: #071d0f;
  color: white;
}
</style>
