<script>
import '@wangeditor/editor/dist/css/style.css' // 引入 css

import { onBeforeUnmount, ref, shallowRef, onMounted } from 'vue'
import { Editor, Toolbar } from '@wangeditor/editor-for-vue'


export default {
  components: { Editor, Toolbar },
  setup() {
    // Editor instance, must use shallowRef
    const editorRef = shallowRef()

    // CONTENT HTML
    const valueHtml = ref('<p>hello</p>')

    //  ajax asynchronous retrieval of content
    onMounted(() => {
      setTimeout(() => {
        valueHtml.value = '<p>input here</p>'
      }, 1500)


      setTimeout(() => {
        var genericIntegrationProperties = {};
        genericIntegrationProperties.target = document.getElementById('wangEditor');
        genericIntegrationProperties.toolbar = document.getElementById('wangToolbar').firstChild;


        // GenericIntegration instance.
        var genericIntegrationInstance = new WirisPlugin.GenericIntegration(genericIntegrationProperties);
        genericIntegrationInstance.init();
        genericIntegrationInstance.listeners.fire('onTargetReady', {});


        WirisPlugin.currentInstance = genericIntegrationInstance;

      // Change buttons styles to match the wang toolbar buttons.
        document.getElementById('editorIcon').style.padding = '11px';
        document.getElementById('chemistryIcon').style.padding = '11px';
      }, 100)

    })

    const toolbarConfig = {}
    const editorConfig = { placeholder: 'inputv...' }

    onBeforeUnmount(() => {
      const editor = editorRef.value
      if (editor == null) return
      editor.destroy()
    })

    const handleCreated = (editor) => {
      editorRef.value = editor // 记录 editor 实例，重要！
    }


    const cpMath2WangHandler = ()=>{
      let my_formula = document.getElementById('mathtype_htmlEditor').innerHTML
      console.log(my_formula)
      editorRef.value.dangerouslyInsertHtml(my_formula)
      // console.log(editor_html)
      // editorRef.value.setHtml(editor_html)
    }

    return {
      editorRef,
      valueHtml,
      mode: 'default', // 或 'simple'
      toolbarConfig,
      editorConfig,
      handleCreated,
      cpMath2WangHandler
    };
  }
}

</script>

<template>
  <div style="border: 1px solid #ccc">

    <Toolbar
        id="wangToolbar"
        style="border-bottom: 1px solid #ccc"
        :editor="editorRef"
        :defaultConfig="toolbarConfig"
        :mode="mode"
    />
    <Editor
        id="wangEditor"
        style="height: 500px; overflow-y: hidden;"
        v-model="valueHtml"
        :defaultConfig="editorConfig"
        :mode="mode"
        @onCreated="handleCreated"
    />
    <input type="button" value="mathtype2wang" @click="cpMath2WangHandler"/>
  </div>
</template>

<style scoped>

</style>
