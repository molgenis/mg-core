<template>
  <div
    v-b-tooltip.d500
    :title="$t('navigator:action-delete')"
    class="btn-tooltip-wrapper"
  >
    <b-btn
      v-b-modal.resourceDeleteModal
      :disabled="!canDelete"
      variant="danger"
    >
      <font-awesome-icon
        :class="{'fa-disabled': !canDelete}"
        icon="trash"
        size="lg"
      />
    </b-btn>
  </div>
</template>

<script>
import { DOWNLOAD_SELECTED_RESOURCES } from '@/store/navigator/actions'
import { mapGetters, mapState } from 'vuex'

export default {
  name: 'NavigatorActionsTransfer',
  computed: {
    ...mapGetters('navigator', [ 'nrSelectedResources' ]),
    ...mapState('navigator', [ 'folder' ]),
    getSelectedResourceType () {
      return this.selectedResources[0].type
    },
    canDelete () {
      return this.nrSelectedResources > 0 && !(this.folder && this.folder.readonly)
    }
  },
  methods: {
    downloadSelectedResources: function () {
      this.$store.dispatch(`navigator/${DOWNLOAD_SELECTED_RESOURCES}`)
    }
  }
}
</script>
