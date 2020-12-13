<template>
  <v-menu
    ref="timePickerMenu"
    v-model="timePickerMenu"
    :close-on-content-click="false"
    :nudge-right="40"
    :return-value.sync="value"
    transition="scale-transition"
    offset-y
    max-width="290px"
    min-width="290px"
  >
    <template v-slot:activator="{ on, attrs }">
      <v-text-field
        v-model="value"
        :label="label"
        prepend-icon="mdi-clock-time-four-outline"
        readonly
        v-bind="attrs"
        v-on="on"
      ></v-text-field>
    </template>
    <v-time-picker
      v-if="timePickerMenu"
      v-model="value"
      full-width
      @click:minute="$refs.timePickerMenu.save(value)"
    ></v-time-picker>
  </v-menu>
</template>

<script>

export default {
  data: () => ({
    timePickerMenu: false,
    value: ''
  }),
  props: {
    val: {
      required: false
    },
    label: {
      required: false,
      default: 'Hora'
    }
  },
  watch: {
    value (newVal) {
      this.$emit('change', newVal)
    }
  },
  beforeMount () {
    this.value = this.val
  }
}
</script>