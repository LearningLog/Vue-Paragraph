<template>
  <div ref="textOverflow" class="text-overflow" :style="boxStyle">
    <el-tooltip v-bind="tooltipAttrs" :disabled="!(type === 'tooltip' || type === 'tooltipExpandable') || expanded || !isCutOut" :content="text">
      <span ref="overEllipsis" :title="!(type === 'tooltip' || type === 'tooltipExpandable') && text">{{ realText }}</span>
    </el-tooltip>
    <span ref="slotRef" class="slot-box">
      <span v-if="showSlotNode && (type === 'expandable' || type === 'tooltipExpandable')" @click="toggle">
        <span v-if="!expanded">{{ unfoldText }}</span>
        <span v-else>{{ foldText }}</span>
      </span>
    </span>
  </div>
</template>

<script>
/**
 使用示例：https://blog.csdn.net/qq_41887214/article/details/116663975
 <Paragraph
    :text="text"
 />
 */
export default {
  props: {
    // 显示的文本
    text: {
      type: String,
      default: ''
    },
    // 最多展示的行数
    maxLines: {
      type: Number,
      default: 3
    },
    // 组件宽
    width: {
      type: Number,
      default: 0
    },
    // 展开
    unfoldText: {
      type: String,
      default: '展开'
    },
    // 收起
    foldText: {
      type: String,
      default: '收起'
    },
    // 是否使用Tooltip
    tooltip: {
      type: Boolean,
      default: false
    },
    // 组件类型，expandable、tooltip、tooltipExpandable
    type: {
      type: String,
      default: 'expandable'
    },

    // el-tooltip Attributes
    tooltipAttrs: {
      type: Object,
      default: () => {
        return {
          effect: 'dark',
          placement: 'top'
        }
      }
    }
  },
  data() {
    return {
      offset: this.text.length, // 原始文本length
      expanded: false, // 是否已展开
      slotBoxWidth: 0, // 展开收起按钮宽度
      textBoxWidth: this.width, // 展示的文本宽度
      showSlotNode: false // 是否展示slot节点
    }
  },
  computed: {
    // 设置展示文本宽度
    boxStyle() {
      if (this.width) {
        return {
          width: this.width + 'px'
        }
      } else {
        return { width: 'auto' }
      }
    },

    // 是否被截取
    isCutOut() {
      const isCutOut = this.offset !== this.text.length
      return isCutOut && !this.expanded
    },

    // 获取展示文本
    realText() {
      let realText = this.text
      if (this.isCutOut) {
        realText = this.text.slice(0, this.offset) + '...'
      }
      return realText
    }
  },
  mounted() {
    const { len } = this.getLines()
    if (len > this.maxLines) {
      this.showSlotNode = true
      this.$nextTick(() => {
        this.slotBoxWidth = this.$refs.slotRef.clientWidth
        this.textBoxWidth = this.$refs.textOverflow.clientWidth
        this.calculateOffset(0, this.text.length)
      })
    }
  },
  methods: {
    // 计算offset 核心代码
    calculateOffset(from, to) {
      this.$nextTick(() => {
        if (Math.abs(from - to) <= 1) return
        if (this.isOverflow()) {
          to = this.offset
        } else {
          from = this.offset
        }
        this.offset = Math.floor((from + to) / 2)
        this.calculateOffset(from, to)
      })
    },

    // 内容是否溢出
    isOverflow() {
      const { len, lastWidth } = this.getLines()
      if (len < this.maxLines) {
        return false
      }
      if (this.maxLines) {
        // 超出部分 行数 > 最大行数 或者  已经是最大行数但最后一行宽度 + 后面内容超出正常宽度
        const lastLineOver = !!(
          len === this.maxLines &&
          lastWidth + this.slotBoxWidth > this.textBoxWidth
        )
        if (len > this.maxLines || lastLineOver) {
          return true
        }
      }
      return false
    },

    // 获取元素占据页面的所有矩形区域的行数和最后一行宽度
    getLines() {
      // getClientRects()：是获取元素占据页面的所有矩形区域：
      const clientRects = this.$refs.overEllipsis.getClientRects()
      return {
        len: clientRects.length,
        lastWidth: clientRects[clientRects.length - 1].width
      }
    },

    // 切换展开收起
    toggle() {
      this.expanded = !this.expanded
    }
  }
}
</script>

<style lang="scss" scoped>
.slot-box {
  display: inline-block;
  cursor: pointer;
  color: #1890ff;
}
</style>
