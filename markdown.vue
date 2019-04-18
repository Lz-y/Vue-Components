<template>
  <div class="markdown-editor">
    <div class="editor-toolbar" v-if="showTool">
      <ul>
        <li><upload @upload-success='uploadSuccess'><span>插入图片</span></upload></li>
        <li @click="insertUrl"><span>插入链接</span></li>
        <li @click="insertCode"><span>代码块</span></li>
        <li @click="setCursorPosition($refs.text, '***')"><span>分割线</span></li>
        <li @click="setCursorPosition($refs.text, '****', 2)"><span>粗体</span></li>
        <li @click="setCursorPosition($refs.text, '**', 1)"><span>斜体</span></li>
        <li @click="setCursorPosition($refs.text, '> ', 2)"><span>引用</span></li>
        <li @click="preview"><span>预览</span></li>
        <li @click="empty"><span>清空</span></li>
      </ul>
    </div>
    <div class="editor-panel">
      <div class="edit" v-show="edit">
        <textarea :value="value" ref="text" placeholder="开始编辑..." @input="enter"></textarea>
      </div>
      <div class="preview render" v-show="!edit" v-html="renderHtml"></div>
    </div>
  </div>
</template>
<script>
import marked from 'marked'
import high from 'highlight.js'
import Upload from './upload'
export default {
  name: 'markdown',
  props: {
    value: String,
    showTool: {
      type: Boolean,
      default: true
    },
    showEdit: {
      type: Boolean,
      default: true
    }
  },
  data () {
    return {
      content: this.value,
      edit: this.showEdit,
      text: null
    }
  },
  components: {
    Upload
  },
  computed: {
    renderHtml () {
      marked.setOptions({
        renderer: new marked.Renderer(),
        // 允许 Git Hub标准的markdown.
        gfm: true,
        // 允许支持表格语法。该选项要求 gfm 为true。
        tables: true,
        // 允许回车换行。该选项要求 gfm 为true。
        breaks: true,
        // 尽可能地兼容 markdown.pl的晦涩部分。不纠正原始模型任何的不良行为和错误。
        pedantic: false,
        // 对输出进行过滤（清理），将忽略任何已经输入的html代码（标签）
        sanitize: true,
        // 使用比原生markdown更时髦的列表。 旧的列表将可能被作为pedantic的处理内容过滤掉.
        smartLists: true,
        // 使用更为时髦的标点，比如在引用语法中加入破折号。
        smartypants: false,
        highlight: function (code) {
          return high.highlightAuto(code).value
        }
      })
      return marked(this.value)
    }
  },
  methods: {
    enter () {
      this.$emit('input', this.content)
    },
    uploadSuccess (file) {
      const img = '![图片描述]()'
      this.setCursorPosition(this.$refs.text, img, img.length - 1)
    },
    insertUrl () {
      const url = '[链接描述]()'
      this.setCursorPosition(this.$refs.text, url, url.length - 1)
    },
    insertCode () {
      const val = '``````'
      this.setCursorPosition(this.$refs.text, val, val.length - 3)
    },
    setCursorPosition (dom, val, posLen) { // 设置光标位置
      let cursorPosition = 0
      if (dom.selectionStart) {
        cursorPosition = dom.selectionStart
      }
      this.insertAtCursor(dom, val)
      dom.focus()
      dom.setSelectionRange(dom.value.length, cursorPosition + (posLen || val.length))
      this.content = dom.value
    },
    insertAtCursor (dom, val) { // 光标所在位置插入字符
      if (document.selection) {
        dom.focus()
        let sel = document.selection.createRange()
        sel.text = val
        sel.select()
      } else if (dom.selectionStart || dom.selectionStart === '0') {
        let startPos = dom.selectionStart
        let endPos = dom.selectionEnd
        let restoreTop = dom.scrollTop
        dom.value = dom.value.substring(0, startPos) + val + dom.value.substring(endPos, dom.value.length)
        if (restoreTop > 0) {
          dom.scrollTop = restoreTop
        }
        dom.focus()
        dom.selectionStart = startPos + val.length
        dom.selectionEnd = startPos + val.length
      } else {
        dom.value += val
        dom.focus()
      }
    },
    preview () {
      this.edit = !this.edit
    },
    empty () {
      this.content = ''
      this.$emit('input', '')
    }
  },
  mounted () {
    this.text = this.$refs.text
  }
}
</script>
<style>
@import '../../assets/css/markdown.css';
.markdown-editor{
  background: #fff;
  border: 1px solid #ddd;
  border-radius: 6px;
}
.editor-toolbar{
  background: #f5f7f9;
  height: 40px;
  line-height: 40px;
  padding: 0 20px;
}
.editor-toolbar li{
  width: 68px;
  display: inline-block;
  cursor: pointer;
  text-align: center;
}
.editor-toolbar li:hover{
  color: #409eff;
}
.editor-panel{
  display: flex;
  border-top: 1px solid #ddd;
  min-height: 500px;
}
.edit{
  flex: 1;
  cursor: text;
  box-sizing: border-box;
  padding: 8px 25px 10px 15px;
}
textarea{
  width: 100%;
  height: 100%;
  border: none;
  resize: none;
  overflow-y: hidden;
  word-wrap: break-word;
  font-size: 15px;
  line-height: 1.5;
}
.preview{
  flex: 1;
  padding: 8px 25px 10px 15px;
}
</style>
