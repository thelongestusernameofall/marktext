<template>
    <div
      class="editor-tabs"
    >
      <ul class="tabs-container">
        <li
          :title="file.pathname"
          :class="{'active': currentFile.id === file.id, 'unsaved': !file.isSaved }"
          v-for="(file, index) of tabs"
          :key="index"
          @click.stop="selectFile(file)"
        >
          <span>{{ file.filename }}</span>
          <svg class="icon" aria-hidden="true"
            @click.stop="removeFileInTab(file)"
          >
            <use xlink:href="#icon-close-small"></use>
          </svg>
        </li>
        <li class="new-file">
          <svg class="icon" aria-hidden="true"
            @click.stop="newFile()"
          >
            <use xlink:href="#icon-plus"></use>
          </svg>
        </li>
      </ul>
    </div>
</template>

<script>
  import { mapState } from 'vuex'
  import { tabsMixins } from '../../mixins'

  export default {
    mixins: [tabsMixins],
    computed: {
      ...mapState({
        currentFile: state => state.editor.currentFile,
        tabs: state => state.editor.tabs
      })
    },
    methods: {
      newFile () {
        this.$store.dispatch('NEW_BLANK_FILE')
      }
    }
  }
</script>

<style scoped>
  .editor-tabs {
    width: 100%;
    height: 35px;
    user-select: none;
  }
  .tabs-container {
    list-style: none;
    margin: 0;
    padding: 0;
    display: flex;
    flex-direction: row;
    overflow: auto;
    &::-webkit-scrollbar:horizontal {
      display: none;
    }
    & > li {
      position: relative;
      padding: 0 8px;
      color: var(--editorColor50);
      font-size: 12px;
      line-height: 35px;
      height: 35px;
      background: var(--floatBgColor);
      display: flex;
      align-items: center;
      & > svg {
        opacity: 0;
      }
      &:hover > svg {
        opacity: 1;
      }
      & > span {
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        margin-right: 3px;
      }
    }
    & > li.active {
      background: var(--itemBgColor);
      &:not(:last-child):after {
        content: '';
        position: absolute;
        left: 0;
        bottom: 0;
        right: 0;
        height: 2px;
        background: var(--themeColor);
      }
      & > svg {
        opacity: 1;
      }
    }

    & > li.new-file {
      width: 35px;
      height: 35px;
      border-right: none;
      background: transparent;
      display: flex;
      align-items: center;
      justify-content: space-around;
      cursor: pointer;
    }
  }
</style>
