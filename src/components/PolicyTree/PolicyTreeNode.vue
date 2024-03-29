<template>
  <div
    class="container"
    :class="{ selected: isSelected, part: isPartOfPath }"
    :style="{ paddingLeft: path.length * 10 + 'px' }"
  >

    <!-- arrow -->
    <div
      class="arrow-container"
      :class="{ selected: isSelected, part: isPartOfPath, hide: hideArrow }"
      @click="$emit('arrowClicked', path)"
    >
      <i v-if="arrowDown" class="fas fa-chevron-down" />
      <i v-else class="fas fa-chevron-right" />
    </div>

    <!-- labels -->
    <div
      class="label-container"
      :class="{ selected: isSelected, part: isPartOfPath, hide: hideArrow }"
      @click="$emit('followPath', path)"
    >
      <a
        href="#"
        :class="{ selected: isSelected, part: isPartOfPath }"
        class="label"
      >
        {{ label }}
        <span v-if="addition" class="addition"><br>{{ addition }}</span>
      </a>
    </div>

  </div>
</template>

<script>
export default {
  name: 'PolicyTreeNode',
  props: {
    label: {
      type: String,
      required: true,
    },
    addition: {
      type: String,
      required: false,
    },
    path: {
      type: Array,
      required: true,
    },
    selectedPath: {
      type: Array,
      required: true,
    },
    arrowDown: {
      type: Boolean,
      default: true,
      required: false,
    },
    hideArrow: {
      type: Boolean,
      default: false,
      required: false,
    },
  },
  computed: {
    isPartOfPath() {
      return this.selectedPath.join().includes(this.path.join());
    },
    isSelected() {
      return this.selectedPath.join() === this.path.join();
    },
  },
};
</script>

<style scoped>
.container {
  display: block;
  margin: 1px;
  cursor: pointer;

  border-top: 1px transparent solid;
  border-bottom: 1px transparent solid;

  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.arrow-container {
  padding-top: 8px;
  padding-left: 3px;
  float: left;
  width: 10%;
  height: 54px;
  color: #323232;
}

.label-container {
  padding: 8px 0px;
  border-top: 1px transparent solid;
  border-bottom: 1px transparent solid;
  width: 90%;
}

.label {
  color: black;
  font-weight: normal;
  text-decoration: none;
}

.addition {
  font-size: 15px;
}

/* on hover styling */

.container:hover:not(.selected) .label {
  font-weight: bold;
  color: #323232;
}

.container:hover:not(.selected) .addition,
.container:hover:not(.selected) .arrow-container {
  font-weight: normal;
  color: #323232;
}

/* highlight the selected node */

.label-container.selected {
  border-top: 1px #1f3b70 solid;
  border-bottom: 1px #1f3b70 solid;
}

.label.selected {
  color: #1f3b70;
  font-weight: bold;
}

.selected .addition {
  font-weight: normal;
  font-style: italic;
}

/* highlight nodes that are part of the path */

.label.part,
.arrow-container.part {
  color: #1f3b70;
}

/* hide arrow for leafs of the tree */

.arrow-container.hide {
  color: transparent;
}

.label-container.selected.hide {
  margin-left: 8%;
}

.container:hover:not(.selected) .arrow-container.hide {
  color: transparent;
}
</style>
