<template>
  <li class="module">
    <div class="arrow-container" :class="{hidden: !isFolder}" @click="toggle">
      <i :class="{down: isOpen}" />
    </div>
    <div class="label" :class="{ selected: isSelected}">
      <customLink class="custom-link" :name="item.label" :query="item.iri" :customLinkOnClick="this.ontologyClicked"></customLink>
    </div>
    <ul v-show="isOpen" v-if="isFolder" class="list-unstyled">
      <module-tree
        :item="subItem"
        :location="location"
        v-for="subItem in item.subModule"
        :key="subItem.label"
      />
    </ul>
  </li>
</template>

<script>
import Vue from 'vue';
import customLink from './chunks/link';

Vue.component('customLink', customLink);

export default {
  name: 'module-tree',
  props: {
    item: Object,
    location: Object,
  },
  data() {
    return {
      isOpen: false,
      isSelected: false,
    };
  },
  methods: {
    toggle() {
      this.isOpen = !this.isOpen;
    },
    ontologyClicked(event) {
    }
  },
  mounted() {
  },
  computed: {
    isFolder() {
      return this.item.subModule && this.item.subModule.length;
    },
  },
  watch: {
    location: {
      handler(val, oldVal) {
        if (val && val.locationInModules) {
          this.isSelected = val.locationInModules.some(
            location => location == this.item.iri,
          );
          this.isOpen = /* this.isOpen || */ this.isSelected; //isOpen is commented out to enable collapsing tree after opening different branch 
        }
      },
      deep: true,
    },
  },
};
</script>

<style lang="scss" scoped>
.module {
  -moz-user-select: none;
  -khtml-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
  user-select: none;

  ul,
  li {
    margin: 0;
    padding: 0;
    line-height: 24px;
    font-size: 12px;
    font-weight: bold;

    ::before {
      margin-top: 10px;
      display: none;
    }
  }

  ul {
    margin-left: 15px;
  }

  .arrow-container {
    display: inline-block;
    width: 10px;
    &.hidden {
      visibility: hidden;
    }
    i {
      border: solid black;
      border-width: 0 2px 2px 0;
      margin-bottom: 2px;
      display: inline-block;
      padding: 3px;
      transform: rotate(-45deg);
      -webkit-transform: rotate(-45deg);

      &.down {
        transform: rotate(45deg);
        -webkit-transform: rotate(45deg);
        margin-bottom: 4px;
      }
    }
  }
  .label {
    display: inline;
    &.selected {
      text-decoration: underline;
    }

    .custom-link {
      margin-left: 10px;
    }
  }
}
</style>
