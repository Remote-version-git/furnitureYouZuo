<template>
  <div class="custom-list-view">
    <x-list-view
      :listEngine="listEngine"
    >
      <template v-slot:listSearch>
        <div></div>
      </template>
      <template v-slot:listTable>
        <x-list-table ref="listTable">
          <template v-slot:customHeadSlot="scopeSlot">
            <div class="custom-list-view-header" @keyup.enter="handlerSearch">
              <x-ellipsis class="custom-list-view-label" :label="scopeSlot.column.label" mode="origin" />
              <div class="custom-list-view-search">
                <el-form :inline="formInline" @submit.native.prevent>
                  <component
                    :is="getSearchComponent(scopeSlot.column.property)"
                    v-if="getSearchComponent(scopeSlot.column.property)"
                    :value="searchValue(scopeSlot.column.property)"
                    :compInfo="getComponentConfig(scopeSlot.column.property)"
                    :tileFormComponent="getComponentConfig(scopeSlot.column.property)"
                    :showOpt="false"
                    @change="searchValueChange($event, scopeSlot.column.property)"
                  ></component>
                </el-form>
              </div>
            </div>
          </template>
        </x-list-table>
      </template>
    </x-list-view>
  </div>
</template>
<script>
import CustomInputSearchItem from './search-compont/custom-input-search-item'
export default {
  name: 'CustomListViewDemo',
  components: {
    CustomInputSearchItem
  },
  props: {
    listEngine: {
      type: Object
    }
  },
  data() {
    return {
      formInline: false
    }
  },
  computed: {
    searchComponentConfig() {
      return (this.listEngine && this.listEngine.listDataControl.searchComponentConfig) || []
    },
    getSearchComponent() {
      return function(uuid) {
        const componetConfig = this.allTiledComps.find(item => item.uuid === uuid) || {}
        const searchComponentConfig = this.searchComponentConfig.find(item => item.componentType === componetConfig.componentType)
        if (searchComponentConfig && searchComponentConfig.searchComponent) {
          // ????????? ???????????? ?????????????????????????????????????????????????????????????????????????????????apaas??????????????????????????????????????????
          if (componetConfig.componentType === 'FORM_TEXT_INPUT') {
            return 'custom-input-search-item'
          }
          return searchComponentConfig.searchComponent + '-search-item'
        } else {
          return undefined
        }
      }
    },
    allTiledComps() {
      return (this.listEngine && this.listEngine.listDataControl.allTiledComps) || []
    },
    getComponentConfig() {
      return function(uuid) {
        return this.allTiledComps.find((comp) => comp.uuid === uuid)
      }
    },
    searchList() {
      return (this.listEngine && this.listEngine.listDataControl.searchList) || []
    },
    searchValue() {
      return function(uuid) {
        const searchConfig = this.searchList.find(item => item.componentUuid === uuid)
        return (searchConfig && searchConfig.params) || []
      }
    }
  },
  methods: {
    searchValueChange(value, uuid) {
      const searchConfig = this.searchList.find(item => item.componentUuid === uuid)
      if (searchConfig && searchConfig.params) {
        searchConfig.params = value
      }
    },
    // ??????????????????
    handlerSearch(e) {
      e.stopPropagation()
      e.preventDefault()
      this.listEngine.listDataControl.tableConfig.pagination.currentPage = 1
      this.listEngine.listDataControl.destroyVirtualAdvanceSearch()
      this.listEngine.actionControl.executeActionWithSync('DO_LIST_SEARCH')
    }
  }
}
</script>
<style lang="scss">
.custom-list-view {
  height: 100%;
  .custom-list-view-label {
    height: 40px;
  }
  .custom-list-view-search {
    border-top: 1px solid #e8eaec;
    margin: 0 -10px;
    height: 40px;
    // ??????????????????????????????
    .el-input, .el-date-editor {
      border: none;
      border-radius: 0;
      outline: none;
      background-color: #fafafa;

      * {
        border: none;
        border-radius: 0;
        outline: none;
        background-color: #fafafa;
      }
    }
    .el-form {
      .el-form-item {
        margin: 0 2px;
        // ?????? ??????????????? ??????
        .el-form-item__label {
          display: none;
        }
        .el-form-item__content {
          line-height: 40px !important;
          padding: 0;
          margin: 0;
        }
      }
      // ??????????????????????????????
      .x-lov-switch-search-item {
        // ?????? ??????????????? ??????
        .title-label {
          display: none;
        }
        .x-select {
          height: 40px;
          position: relative;
          background-color: #fafafa;
          .form-select-input {
            top: 50%;
            transform: translateY(-50%);
          }
          .select-input-tag {
            border: none;
            border-radius: 0;
            outline: none;
            background-color: #fafafa;
            height: 32px;
            z-index: 0;
          }
          .x-select-suffix {
            z-index: 1;
          }
        }
      }
      // ??????????????????????????????
      .x-number-search-item, .x-money-search-item {
        .range-item-divide {
          margin: 0;
        }
      }

      .el-select .el-input .el-select__caret {
        font-size: 14px !important;
        top: 0px;
      }
    }
  }
}
</style>
