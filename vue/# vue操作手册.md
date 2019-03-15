# Vue操作手册





### vue-ci初始化项目



##### 安装vue-cli脚手架：

```
cnpm install vue-cli -g       #全局安装vue-cli
```



##### 初始化一个项目：

```
vue init webpack vue_demo      #用webpack模板创建一个项目
```







### 引入element-ui



1. 打开cmd，在项目的根目录下输入：

   ``` 
   npm install element-ui -S  # -S：安装到上线环境 -D：安装到开发环境
   ```










### 引入 mavon-editor



1. 安装mavon-editor

   ```shell
   $ cnpm install mavon-editor --save
   ```

2. 全局引入

   `index.js` 中全局引用

   ```javascript
       // 全局注册
       // import with ES6
       import Vue from 'vue'
       import mavonEditor from 'mavon-editor'
       // markdown-it对象：md.s_markdown, md => mavonEditor实例
       //                 or
       //                 mavonEditor.markdownIt 
       import 'mavon-editor/dist/css/index.css'
       // use
       Vue.use(mavonEditor)
       new Vue({
           'el': '#main',
           data() {
               return { value: '' }
           }
       })
   ```

4. 页面引入

   ``` vue
   <mavon-editor v-model="context" :toolbars="toolbars" @keydown="" />
   data(){
     return {
         context: ' ',//输入的数据
         toolbars: {
                 bold: true, // 粗体
                 italic: true, // 斜体
                 header: true, // 标题
                 underline: true, // 下划线
                 mark: true, // 标记
                 superscript: true, // 上角标
                 quote: true, // 引用
                 ol: true, // 有序列表
                 link: true, // 链接
                 imagelink: true, // 图片链接
                 help: true, // 帮助
                 code: true, // code
                 subfield: true, // 是否需要分栏
                 fullscreen: true, // 全屏编辑
                 readmodel: true, // 沉浸式阅读
                 /* 1.3.5 */
                 undo: true, // 上一步
                 trash: true, // 清空
                 save: true, // 保存（触发events中的save事件）
                 /* 1.4.2 */
                 navigation: true // 导航目录
               }
     }
   }
   ```

   

5. 图片上传

``` javascript
<template>
    <mavon-editor ref=md @imgAdd="$imgAdd" @imgDel="$imgDel"></mavon-editor>
</template>
exports default {
    methods: {
        // 绑定@imgAdd event
        $imgAdd(pos, $file){
            // 第一步.将图片上传到服务器.
           var formdata = new FormData();
           formdata.append('image', $file);
           axios({
               url: 'server url',
               method: 'post',
               data: formdata,
               headers: { 'Content-Type': 'multipart/form-data' },
           }).then((url) => {
               // 第二步.将返回的url替换到文本原位置![...](./0) -> ![...](url)
               /**
               * $vm 指为mavonEditor实例，可以通过如下两种方式获取
               * 1. 通过引入对象获取: `import {mavonEditor} from ...` 等方式引入后，`$vm`为`mavonEditor`
               * 2. 通过$refs获取: html声明ref : `<mavon-editor ref=md ></mavon-editor>，`$vm`为 `this.$refs.md`
               */
               $vm.$img2Url(pos, url);
           })
        }
    }
}
```

6. 数据库获取显示

   ```javascript
   <mavon-editor
         class="md"
        :value="articleDetail.context"//获取数据
        :subfield = "prop.subfield"
        :defaultOpen = "prop.defaultOpen"
        :toolbarsFlag = "prop.toolbarsFlag"
        :editable="prop.editable"
        :scrollStyle="prop.scrollStyle"
     ></mavon-editor>
   
       computed: {
           prop () {
             let data = {
               subfield: false,// 单双栏模式
               defaultOpen: 'preview',//edit： 默认展示编辑区域 ， preview： 默认展示预览区域 
               editable: false,
               toolbarsFlag: false,
               scrollStyle: true
             }
             return data
           }
         },
   ```

   