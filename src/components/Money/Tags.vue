<template>
  <div class="tags">
    <div class="new">
      <router-link class="addLabel" to="/add">新增标签</router-link>
    </div>
    <ul class="current">
      <li v-for="tag in tagList" :key="tag.id"
          :class="{selected: selectedTags.indexOf(tag)>=0}"
          @click="toggle(tag)">
        <Icon :name="tag.iconName"/>
        <span>{{tag.name}}</span>
      </li>
    </ul>
  </div>

</template>

<script lang="ts">
  import Vue from 'vue';
  import {Component} from 'vue-property-decorator';
  import {mixins} from "vue-class-component";
  import TagHelper from '@/mixins/TagHelper';

  @Component
  export default class Tags extends mixins(TagHelper) {
    selectedTags: Tag[] = [];

    get tagList() {
      return this.$store.state.tagList
    }

    created() {
      this.$store.commit('fetchTags')
    }

    toggle(tag: Tag) {
      const index = this.selectedTags.indexOf(tag);
      if (index >= 0) {
        this.selectedTags.splice(index, 1);
      } else {
        this.selectedTags.push(tag);
      }
      this.$emit('update:value', this.selectedTags);
    }

  }
</script>

<style lang="scss" scoped>
  .tags {
    font-size: 14px;
    padding-left: 16px;
    padding-bottom: 10px;
    flex-grow: 1;
    background: #ffffff;
    display: flex;
    flex-direction: column-reverse;
    > .current {
      flex: 1;
      display: flex;
      align-items: center;
      flex-wrap: wrap;
      overflow-y: auto;
      width: auto;
      > li {
        .icon{
          min-width: 35px;
          min-height: 35px;
          border-radius: 50%;
          font-size: 15px;
          background: #d9d9d9;
          padding: 5px;
        }
        span{
          min-width: 50px;
        }
        $bg: #d9d9d9;
        padding: 10px;
        display: flex;
        align-content: center;
        align-items: center;
        text-align: center;
        flex-direction: column;
        font-size: 12px;
        &.selected .icon{
          background: darken($bg, 50%);
          color: #f5f5f5;
        }
      }
    }
    > .new {
      padding-top: 10px;
      .addLabel {
        background: transparent;
        border: none;
        color: #999;
        border-bottom: 1px solid;
        padding: 0 4px;
      }
    }
  }
</style>