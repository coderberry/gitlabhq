<script>
import { mapActions } from 'vuex';
import { __, sprintf } from '~/locale';
import Icon from '~/vue_shared/components/icon.vue';
import tooltip from '~/vue_shared/directives/tooltip';
import ListItem from './list_item.vue';

export default {
  components: {
    Icon,
    ListItem,
  },
  directives: {
    tooltip,
  },
  props: {
    title: {
      type: String,
      required: true,
    },
    fileList: {
      type: Array,
      required: true,
    },
    iconName: {
      type: String,
      required: true,
    },
    action: {
      type: String,
      required: true,
    },
    actionBtnText: {
      type: String,
      required: true,
    },
    itemActionComponent: {
      type: String,
      required: true,
    },
    stagedList: {
      type: Boolean,
      required: false,
      default: false,
    },
  },
  data() {
    return {
      showActionButton: false,
    };
  },
  computed: {
    titleText() {
      return sprintf(__('%{title} changes'), {
        title: this.title,
      });
    },
  },
  methods: {
    ...mapActions(['stageAllChanges', 'unstageAllChanges']),
    actionBtnClicked() {
      this[this.action]();
    },
    setShowActionButton(show) {
      this.showActionButton = show;
    },
  },
};
</script>

<template>
  <div
    class="ide-commit-list-container"
  >
    <header
      class="multi-file-commit-panel-header"
      @mouseenter="setShowActionButton(true)"
      @mouseleave="setShowActionButton(false)"
    >
      <div
        class="multi-file-commit-panel-header-title"
      >
        <icon
          v-once
          :name="iconName"
          :size="18"
        />
        {{ titleText }}
        <span
          v-show="!showActionButton"
          class="ide-commit-file-count"
        >
          {{ fileList.length }}
        </span>
        <button
          v-show="showActionButton"
          type="button"
          class="btn btn-blank btn-link ide-staged-action-btn"
          @click="actionBtnClicked"
        >
          {{ actionBtnText }}
        </button>
      </div>
    </header>
    <ul
      v-if="fileList.length"
      class="multi-file-commit-list list-unstyled append-bottom-0"
    >
      <li
        v-for="file in fileList"
        :key="file.key"
      >
        <list-item
          :file="file"
          :action-component="itemActionComponent"
          :key-prefix="title"
          :staged-list="stagedList"
        />
      </li>
    </ul>
    <p
      v-else
      class="multi-file-commit-list help-block"
    >
      {{ __('No changes') }}
    </p>
  </div>
</template>
