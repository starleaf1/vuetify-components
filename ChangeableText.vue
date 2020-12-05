<template>
  <div>
    <span v-show="isEditing">
      <v-text-field :label="label" :loading="isLoading" :disabled="isDisabled"
        v-model="outputValue"
        :hint="hint"
      >
        <template v-slot:append-outer>
          <div v-if="!loading">
            <v-tooltip top>
              <template v-slot:activator="{ on }">
                <v-btn
                  @click="changeValue"
                  color="primary"
                  icon
                  :disabled="isDisabled"
                  small
                  v-on="on"
                >
                  <v-icon small>mdi-check</v-icon>
                </v-btn>
              </template>
              <span>OK</span>
            </v-tooltip>
            <v-tooltip top>
              <template v-slot:activator="{ on }">
                <v-btn
                  @click="cancelEdit"
                  icon
                  :disabled="isDisabled"
                  small
                  v-on="on"
                >
                  <v-icon small>mdi-undo</v-icon>
                </v-btn>
              </template>
              <span>Reset</span>
            </v-tooltip>
          </div>
          <div v-else>
            <v-progress-circular indeterminate />
          </div>
        </template>
      </v-text-field>
    </span>
    <div v-show="!isEditing">
      <h5 class="text-caption font-weight-bold" v-text="label" />
      <div>
        <span :class="{ 'text--disabled': !outputValue, 'font-italic': !outputValue }" v-text="outputValue ? outputValue : '(kosong)'" />
        <v-btn dark @click="isEditing = true" color="primary" icon small><v-icon small>mdi-pencil</v-icon></v-btn>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ChangeableText',

  props: {
    value: String,
    label: String,
    loading: Boolean,
    disabled: Boolean,
    handler: Function,
    hint: {
      type: String,
      default: () => 'Input teks yang dikehendaki, kemudian tekan tanda centang. Tekan panah balik untuk membatalkan.'
    }
  },

  data () {
    return ({
      isEditing: false,
      outputValue: this.value,
      isLoading: this.loading,
      isDisabled: this.disabled
    })
  },

  methods: {
    async changeValue () {
      this.$data.isDisabled = true
      try {
        if (typeof this.handler === 'function') await this.handler(this.$data.outputValue)
      } catch (e) {
        /* eslint no-console: ["error", { allow: ["warn", "error"] }] */
        console.error(e)
      }
      this.$emit('input', this.outputValue)
      this.$data.isDisabled = false
      this.$data.isEditing = false
    },

    cancelEdit () {
      this.$data.outputValue = this.value
      this.$data.isEditing = false
    }
  }
}
</script>
