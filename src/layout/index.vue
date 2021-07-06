<template>
  <div :class="classObj" class="app-wrapper">
    <div
      v-if="device === 'mobile' && sidebar.opened"
      class="drawer-bg"
      @click="handleClickOutside"
    />
    <div v-if="!isFull">
      <sidebar class="sidebar-container" />
    </div>

    <div
      :class="{ hasTagsView: needTagsView }"
      class="main-container"
      :style="{ marginLeft: isFull ? 0 : '210px' }"
    >
      <div :class="{ 'fixed-header': fixedHeader }">
        <navbar
          :is-show="isShow"
          @changeComponentData="handleComponent"
          @changeComponentDataCopy="handleComponentTwo"
        />
        <tags-view v-if="needTagsView" />
      </div>
      <app-main />
      <right-panel v-if="showSettings">
        <settings />
      </right-panel>

      <!-- Modal -->
      <div
        v-show="isShow"
        class="modal"
      >
        <div class="modal-box">
          <div class="modal-header">
            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-hidden="true"
            >
              &times;
            </button>
            <h3>对话框标题</h3>
          </div>
          <div class="modal-body">
            <iframe
              src="http://192.168.1.21:9527/#/dashboard"
              width="460"
              height="auto"
              frameborder="no"
              border="0"
            />
          </div>
          <div class="modal-footer">
            <a href="#" class="btn" @click="isShow = false">关闭</a>
            <a href="#" class="btn btn-primary">Save changes</a>
          </div>
        </div>
      </div>

      <!-- 弹框 -->
      <el-dialog title="提示" :visible.sync="dialogVisible">
        <div><app-main /></div>
        <span slot="footer" class="dialog-footer">
          <el-button @click="dialogVisible = false">取 消</el-button>
          <el-button
            type="primary"
            @click="dialogVisible = false"
          >确 定</el-button>
        </span>
      </el-dialog>
    </div>
  </div>
</template>

<script>
import RightPanel from '@/components/RightPanel'
import { AppMain, Navbar, Settings, Sidebar, TagsView } from './components'
import ResizeMixin from './mixin/ResizeHandler'
import { mapState } from 'vuex'

export default {
  name: 'Layout',
  components: {
    AppMain,
    Navbar,
    RightPanel,
    Settings,
    Sidebar,
    TagsView
  },
  mixins: [ResizeMixin],
  data() {
    return {
      isShow: false,
      dialogVisible: false,
      isFull: false
    }
  },
  computed: {
    ...mapState({
      sidebar: (state) => state.app.sidebar,
      device: (state) => state.app.device,
      showSettings: (state) => state.settings.showSettings,
      needTagsView: (state) => state.settings.tagsView,
      fixedHeader: (state) => state.settings.fixedHeader
    }),
    classObj() {
      return {
        hideSidebar: !this.sidebar.opened,
        openSidebar: this.sidebar.opened,
        withoutAnimation: this.sidebar.withoutAnimation,
        mobile: this.device === 'mobile'
      }
    }
  },
  mounted() {
    this.$EventBus.$on('updateFull', (val) => {
      this.isFull = val
      console.log('我要开始打印接收数据了====》', val)
    })
  },
  methods: {
    handleClickOutside() {
      this.$store.dispatch('app/closeSideBar', { withoutAnimation: false })
    },
    handleComponent(params) {
      this.isShow = params
    },
    handleComponentTwo(params) {
      this.dialogVisible = params
    }
  }
}
</script>

<style lang="scss" scoped>
@import "~@/styles/mixin.scss";
@import "~@/styles/variables.scss";

.app-wrapper {
  @include clearfix;
  position: relative;
  height: 100%;
  width: 100%;

  &.mobile.openSidebar {
    position: fixed;
    top: 0;
  }
}

.drawer-bg {
  background: #000;
  opacity: 0.3;
  width: 100%;
  top: 0;
  height: 100%;
  position: absolute;
  z-index: 999;
}

.fixed-header {
  position: fixed;
  top: 0;
  right: 0;
  z-index: 9;
  width: calc(100% - #{$sideBarWidth});
  transition: width 0.28s;
}

.hideSidebar .fixed-header {
  width: calc(100% - 54px);
}

.mobile .fixed-header {
  width: 100%;
}

.modal {
  display: block;
  background: gray;
  position: absolute; inset: 0px; z-index: 2000;
  .modal-box {
    background: #fff;
    width: 500px;
    height: 500px;
    margin: 30px auto;
  }
}
</style>
