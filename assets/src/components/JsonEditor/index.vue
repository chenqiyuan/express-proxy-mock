<template>
  <div class="jsoneditor-box" ref="jsoneditor">
  </div>
</template>

<script>
  import JSONEditor from 'jsoneditor/dist/jsoneditor-minimalist.js'
  import 'jsoneditor/dist/jsoneditor.min.css'
  import _ from 'lodash'
  export default {
    name: 'json-editor',
    data () {
      return {
        editor: null
      }
    },
    props: {
      json: {
        required: true
      },
      options: {
        type: Object,
        default: () => {
          return {
            mode: 'code',
            indentation: 2,
            ace: ace
          }
        }
      },
      onChange: {
        type: Function
      },
      onError: {
        type: Function
      }
    },
    methods: {
      _onChange (e) {
        if (this.onChange && this.editor) {
          this.onChange(this.editor.get())
        }
      }
    },
    mounted () {
      const container = this.$refs.jsoneditor
      const options = _.extend({
        onChange: this._onChange,
        onError: function () {
          debugger
        }
      }, this.options)
      this.editor = new JSONEditor(container, options)
      this.editor.set(this.json)
    },
    beforeDestroy () {
      if (this.editor) {
        this.editor.destroy()
        this.editor = null
      }
    }
  }
</script>

<style>
  .jsoneditor-box *{
    transition: initial;
  }
</style>
