<template>
  <div style="height: 100vh;" id="C2_21">
    <div>
      <!-- 1.如果你需要高亮的代码是一个变量值，那么你可以这样使用它。 其中 sourcecode 为变量。 -->
      <pre v-highlightjs="sourcecode"><code></code></pre>
      <el-button type="primary" style="text-align: center;" @click="arrSortTest">运行</el-button>
    </div>
    <div>
      <h4>运行结果</h4>
      <h5>转换前</h5>
      {{numString}}
      <h5>转换后</h5>
<!--      {{arr.sort()}}-->
      {{numString.split('')}}
    </div>
    <div>
      <h4>代码编辑器</h4>
      <textarea ref="mycode" class="codesql" v-model="code" style="height:200px;width:600px;"></textarea>
    </div>
  </div>
</template>

<script>
import "codemirror/theme/ambiance.css";
import "codemirror/lib/codemirror.css";
import "codemirror/addon/hint/show-hint.css";

let CodeMirror = require("codemirror/lib/codemirror");
require("codemirror/addon/edit/matchbrackets");
require("codemirror/addon/selection/active-line");
// require("codemirror/mode/sql/sql");
require("codemirror/mode/javascript/javascript");
require("codemirror/addon/hint/show-hint");
// require("codemirror/addon/hint/sql-hint");
require("codemirror/addon/hint/javascript-hint");

	export default {
    name: "codeMirror",
	  data () {
	    return {
	      numString: '123456',
        arr: [2,10,6,1,4,22,3],
        code: '//按Ctrl键进行代码提示'
      }
    },
    computed: {
      sourcecode () {
        return this.numString.toString()
      }
    },
    mounted () {
      debugger
      let mime = 'application/javascript' // 由相应js文件中CodeMirror.defineMIME决定
      let theme = 'ambiance'//设置主题，不设置的会使用默认主题
      let editor = CodeMirror.fromTextArea(this.$refs.mycode, {
        mode: mime,//选择对应代码编辑器的语言，我这边选的是数据库，根据个人情况自行设置即可
        indentWithTabs: true,
        smartIndent: true,
        lineNumbers: true,
        matchBrackets: true,
        theme: theme,
        autofocus: true,
        extraKeys: {'Ctrl': 'autocomplete'},//自定义快捷键
        hintOptions: {//自定义提示选项
          tables: {
            users: ['name', 'score', 'birthDate'],
            countries: ['name', 'population', 'size']
          }
        }
      })
      //代码自动提示功能，记住使用cursorActivity事件不要使用change事件，这是一个坑，那样页面直接会卡死
      editor.on('cursorActivity', function () {
        editor.showHint()
      })
    },
	  methods: {
      arrSortTest () {
        let arr = [2,10,6,1,4,22,3]
        console.log(arr.sort())   // [1, 10, 2, 22, 3, 4, 6]
        let arr1 = arr.sort((a, b) =>a - b)
        console.log(arr1)   // [1, 2, 3, 4, 6, 10, 22]
        let arr2 = arr.sort((a, b) =>b-a)
        console.log(arr2)  // [22, 10, 6, 4, 3, 2, 1]
      }
    }
	}

</script>

<style scoped>
.codesql {
  font-size: 11pt;
  font-family: Consolas, Menlo, Monaco, Lucida Console, Liberation Mono, DejaVu Sans Mono, Bitstream Vera Sans Mono, Courier New, monospace, serif;
}
#C2_2 {
  font-family: Arial;
  font-size: 18px;
  line-height: 30px;
  font-weight: bold;
  color: white;
  padding: 40px;
  box-shadow:
      inset #0096880005px,
      inset #059c8e0001px,
      inset #0cab9c00010px,
      inset #1fbdae00011px,
      inset #8ce9ff00016px,
      inset #48e4d600017px,
      inset #e5f9f700021px,
      inset #bfecf700022px
}
</style>