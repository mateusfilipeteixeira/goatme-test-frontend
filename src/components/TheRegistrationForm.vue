<template>
  <div>
    <v-row justify="center">
      <v-dialog
        v-model="registrationMode"
        persistent
        max-width="600px"
        hide-overlay
      >
        <v-card>
          <v-card-title>
            <span class="headline">Novo registro</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col
                  cols="12"
                >
                  <v-text-field
                    label="Evento"
                    v-model="sport.name"
                  ></v-text-field>
                </v-col>
                <v-col
                  cols="12"
                >
                  <v-textarea
                    label="Descrição"
                    v-model="sport.description"
                  ></v-textarea>
                </v-col>
                <v-col
                  cols="12"
                >
                  <BaseDatePicker
                    @change="changeDate"
                    :val="sport.schedule_date"
                  />
                </v-col>
                <v-col
                    cols="6"
                  >
                    <BaseTimePicker
                      @change="changeStart"
                      :val="sport.start"
                    />
                  </v-col>
                  <v-col
                    cols="6"
                  >
                    <BaseTimePicker
                      @change="changeEnd"
                      :val="sport.end"
                    />
                  </v-col>
                  <v-col
                    cols="12"
                  >
                    <v-radio-group
                      v-model="sport.recurrent"
                      row
                    >
                      <v-radio
                        label="Não se repete"
                        value="0"
                      ></v-radio>
                      <v-radio
                        label="Repetir"
                        value="1"
                      ></v-radio>
                    </v-radio-group>
                  </v-col>
                  <v-col
                    cols="12"
                    v-if="sport.recurrent == '1'"
                  >
                    <v-slider
                      :label="'Intervalo de Semanas'"
                      max="53"
                      min="2"
                      thumb-label="always"
                      v-model="sport.weeks"
                    ></v-slider>
                  </v-col>
              </v-row>
              <v-overlay :value="sendingData">
                <v-progress-circular
                  indeterminate
                  size="64"
                ></v-progress-circular>
              </v-overlay>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
              color="primary"
              text
              @click="saveSport"
            >
              Salvar
            </v-btn>
            <v-btn
              color="red"
              text
              @click="reset"
            >
              Cancelar
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-row>
  </div>
</template>

<script>
import BaseDatePicker from './BaseDatePicker.vue'
import BaseTimePicker from './BaseTimePicker.vue'

export default {
  components: {
    BaseDatePicker,
    BaseTimePicker
  },
  data: () => ({
    startTimeMenu: false,
    endTimeMenu: false,
    sport: {
      recurrent: '0'
    },
    sendingData: false
  }),
  props: {
    registrationMode: {
      type: Boolean,
      required: true
    }
  },
  methods: {
    async saveSport () {
      try {
        this.sendingData = true
        await this.axios.post('http://localhost:8000/api/schedules', this.sport)
        this.reset()
      } catch (error) {
        console.error(error)
      }
      this.sendingData = false
      this.$root.$emit('loadEvents')
    },
    reset () {
      this.closeModal()
      this.sport = {
        recurrent: '0'
      }
    },
    closeModal () {
      this.$emit('closeModal');
    },
    changeDate (value) {
      this.sport.schedule_date = value
    },
    changeStart (value) {
      this.sport.start = value
    },
    changeEnd (value) {
      this.sport.end = value
    }
  },
  watch: {
    'sport.recurrent' (newValue) {
      if(newValue == '0'){
        return this.sport.weeks = 1
      }
      return this.sport.weeks = 2
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
