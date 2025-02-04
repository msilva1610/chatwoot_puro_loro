<script>
import { useAdmin } from 'dashboard/composables/useAdmin';
export default {
  name: 'WootTabsItem',
  props: {
    index: {
      type: Number,
      default: 0,
    },
    name: {
      type: String,
      required: true,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    count: {
      type: Number,
      default: 0,
    },
    showBadge: {
      type: Boolean,
      default: true,
    },
  },
  setup() {
    const { isAdmin } = useAdmin();
    return {
      isAdmin,
    };
  },
  computed: {
    active() {
      return this.index === this.$parent.index;
    },
    shouldShowTab() {
      // return this.name !== 'All' || this.isAdmin;
      return this.isAdmin;
    },
    getItemCount() {
      return this.count;
    },
  },

  methods: {
    onTabClick(event) {
      event.preventDefault();
      if (!this.disabled) {
        this.$parent.$emit('change', this.index);
      }
    },
  },
};
</script>

<template>
  <li
    v-if="shouldShowTab"
    class="tabs-title"
    :class="{
      'is-active': active,
    }"
  >
    <a @click="onTabClick">
      {{ name }}
      <div v-if="showBadge" class="badge min-w-[20px]">
        <span>
          {{ getItemCount }}
        </span>
      </div>
    </a>
  </li>
</template>
