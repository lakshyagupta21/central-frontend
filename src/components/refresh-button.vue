<!--
Copyright 2017 ODK Central Developers
See the NOTICE file at the top-level directory of this distribution and at
https://github.com/getodk/central-frontend/blob/master/NOTICE.

This file is part of ODK Central. It is subject to the license terms in
the LICENSE file found in the top-level directory of this distribution and at
https://www.apache.org/licenses/LICENSE-2.0. No part of ODK Central,
including this file, may be copied, modified, propagated, or distributed
except according to the terms contained in the LICENSE file.
-->
<template>
  <button :disabled="disabled || refreshing" type="button"
    class="btn btn-secondary btn-refresh" @click="refresh">
    <span class="icon-refresh"></span>Refresh list
    <spinner :state="refreshing"/>
  </button>
</template>

<script>
import Spinner from './spinner.vue';
import { noop } from '../util/util';

export default {
  name: 'RefreshButton',
  components: { Spinner },
  props: {
    // Configs for this.$store.dispatch('get')
    configs: {
      type: Array,
      required: true
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      refreshing: false
    };
  },
  methods: {
    refresh() {
      this.refreshing = true;
      this.$store.dispatch('get', this.configs.map(config => ({
        clear: false,
        ...config
      })))
        .catch(noop)
        .finally(() => {
          this.refreshing = false;
        });
    }
  }
};
</script>
