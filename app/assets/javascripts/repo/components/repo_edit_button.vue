<script>
import Store from '../stores/repo_store';
import RepoMixin from '../mixins/repo_mixin';

export default {
  data: () => Store,
  mixins: [RepoMixin],
  computed: {
    buttonLabel() {
      return this.editMode ? this.__('Cancel edit') : this.__('Edit');
    },

    buttonIcon() {
      return this.editMode ? [] : ['fa', 'fa-pencil'];
    },
  },
  methods: {
    editClicked() {
      if (this.changedFiles.length) {
        this.dialog.open = true;
        return;
      }
      this.editMode = !this.editMode;
      Store.toggleBlobView();
    },
  },

  watch: {
    editMode() {
      if (this.editMode) {
        $('.project-refs-form').addClass('disabled');
        $('.fa-long-arrow-right').show();
        $('.project-refs-target-form').show();
      } else {
        $('.project-refs-form').removeClass('disabled');
        $('.fa-long-arrow-right').hide();
        $('.project-refs-target-form').hide();
      }
    },
  },
};
</script>

<template>
<button class="btn btn-default" @click.prevent="editClicked" v-cloak v-if="isCommitable && !activeFile.render_error" :disabled="binary">
  <i :class="buttonIcon"></i>
  <span>{{buttonLabel}}</span>
</button>
</template>
