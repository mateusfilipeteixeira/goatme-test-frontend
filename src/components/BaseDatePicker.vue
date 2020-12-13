<template>
  <v-menu
    v-model="BaseDatePickerdateMenu"
    :close-on-content-click="false"
    :nudge-right="40"
    transition="scale-transition"
    offset-y
    min-width="290px"
    >
    <template v-slot:activator="{ on, attrs }">
      <v-text-field
        v-model="value"
        :label="label"
        prepend-icon="mdi-calendar"
        readonly
        v-bind="attrs"
        v-on="on"
      ></v-text-field>
    </template>
    <v-date-picker
      v-model="value"
      @input="BaseDatePickerdateMenu = false"
    ></v-date-picker>
  </v-menu>
</template>

<script>

export default {
  data: () => ({
    BaseDatePickerdateMenu: false,
    value: ''
  }),
  props: {
    val: {
      required: false
    },
    label: {
      required: false,
      default: 'Data'
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