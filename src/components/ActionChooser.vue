<template>
  <BaseModal>
    <template v-slot:header>
      <h1>{{ $t('actionChooserHeader') }}</h1>
    </template>
    <template v-slot:body>
      <ul class="actions-list">
        <li
          v-for="(action, index) in actions"
          :key="index"
          :class="{selected: currentSelected === index}"
          @click="actionClicked(index)"
        >
          {{ $t(action) }}
        </li>
      </ul>
    </template>
    <template v-slot:footer>
      <div class="modal-footer">
        <BaseButton textlike @click="abort">
          {{ $t('cancel') }}
        </BaseButton>
        <BaseButton :disabled="currentAction == ''" @click="chosen($event)">
          {{ $t('accept') }}
        </BaseButton>
      </div>
    </template>
  </BaseModal>
</template>

<script>
import BaseButton from './BaseComponents/BaseButton.vue';
import BaseModal from './BaseComponents/BaseModal.vue';
import { actionList } from '../libs/odrl/actions';

export default {
  name: 'ActionChooser',
  components: {
    BaseButton,
    BaseModal,
  },
  data() {
    return {
      currentSelected: null,
      currentAction: '',
    };
  },
  computed: {
    actions() {
      return actionList;
    },
  },
  methods: {
    actionClicked(actionId) {
      this.currentAction = this.actions[actionId];
      this.currentSelected = actionId;
    },
    abort() {
      this.$emit('abort');
    },
    chosen(action) {
      this.$emit('chosen', this.currentAction);
    },
  },
};
</script>

<style scoped>
.actions-list {
  margin: 0px;
  padding: 0px;
}
.actions-list li {
  padding: 0.5em;
  border-bottom: 1px lightgray solid;
  cursor: default;
}
.actions-list li:hover {
  background-color: rgb(238, 238, 238);
  cursor: pointer;
}
.actions-list .selected {
  background-color: rgb(218, 218, 218);
}
.actions-list .selected:hover {
  background-color: rgb(218, 218, 218);
}

.modal-footer {
  text-align: right;
}

</style>
