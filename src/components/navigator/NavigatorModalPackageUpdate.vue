<template>
  <b-modal
    id="packageUpdateModal"
    ref="packageUpdateModal"
    :title="$t('navigator:update-package-title')"
    :ok-title="$t('navigator:update-package-ok-text')"
    :cancel-title="$t('navigator:update-package-cancel-text')"
    @ok="handleOk"
    @shown="resetForm"
  >
    <b-form
      :validated="validated"
      @submit.stop.prevent="handleSubmit"
    >
      <b-form-group
        id="packageLabelInputGroup"
        :label="$t('navigator:package-input-label') + ' *'"
        :invalid-feedback="$t('navigator:package-input-label-invalid')"
        label-for="packageNameInput"
      >
        <b-form-input
          id="packageLabelInput"
          ref="updatePackageLabelInput"
          v-model="form.label"
          type="text"
          required
        />
      </b-form-group>
      <b-form-group
        id="packageDescriptionInputGroup"
        :label="$t('navigator:package-input-description')"
        label-for="packageDescriptionInput"
      >
        <b-form-input
          id="packageDescriptionInput"
          v-model="form.description"
          type="text"
        />
      </b-form-group>
    </b-form>
  </b-modal>
</template>

<script>
import {
  UPDATE_RESOURCE
} from '@/store/navigator/actions'
import { mapState } from 'vuex'

export default {
  name: 'NavigatorModalPackageUpdate',
  data () {
    return {
      form: {
        label: '',
        description: ''
      },
      validated: false
    }
  },
  computed: {
    ...mapState('navigator', [ 'resources', 'selectedResources' ]),
    selectedResource () {
      var selectedResourceId = this.selectedResources[0].id
      return this.resources.find(resource => resource.id === selectedResourceId)
    }
  },
  methods: {
    resetForm () {
      this.form = { label: this.selectedResource.label, description: this.selectedResource.description }
      this.validated = false
      this.$refs.updatePackageLabelInput.$el.focus()
    },
    handleOk (evt) {
      evt.preventDefault()
      if (!this.form.label) {
        this.validated = true
      } else {
        this.handleSubmit()
      }
    },
    handleSubmit () {
      var updatedResource = Object.assign({}, this.selectedResource,
        { label: this.form.label, description: this.form.description })
      this.$store.dispatch(`navigator/${UPDATE_RESOURCE}`, updatedResource)
      this.$refs.packageUpdateModal.hide()
    }
  }
}
</script>
