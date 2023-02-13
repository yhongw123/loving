<template>
    <div id="app">
        <StyleEditor ref="styleEditor" :code="currentStyle"></StyleEditor>
        <ResumeEditor ref="resumeEditor" :markdown="currentMarkdown" :enableHtml="enableHtml"></ResumeEditor>
    </div>
</template>
<script>
import StyleEditor from './components/StyleEditor'
import ResumeEditor from './components/ResumeEditor'
import './assets/reset.css'
let isPc = (function() {
    var userAgentInfo = navigator.userAgent;
    var Agents = ["Android", "iPhone",
        "SymbianOS", "Windows Phone",
        "iPad", "iPod"
    ];
    var flag = true;
    for (var v = 0; v < Agents.length; v++) {
        if (userAgentInfo.indexOf(Agents[v]) > 0) {
            flag = false;
            break;
        }
    }
    return flag;
}());
let getDateDiff = function(startDate, endDate) {
    var startTime = new Date(Date.parse(startDate.replace(/-/g, "/"))).getTime();
    var endTime = new Date(Date.parse(endDate.replace(/-/g, "/"))).getTime();
    var dates = Math.abs((startTime - endTime)) / (1000 * 60 * 60 * 24);
    return dates;
}
//document.title += getDateDiff((new Date()).getFullYear() + '-' + ((new Date()).getMonth() + 1) + '-' + (new Date()).getDate(), '2023-09-15') + 1 + '天';
export default {
    name: 'app',
    components: {
        StyleEditor,
        ResumeEditor
    },
    data() {
        return {
            interval: 27,
            currentStyle: '',
            enableHtml: false,
            fullStyle: [
                `/*
* hello, 节日快乐
* 之前你说你对自己该不该转行有点迷茫。
* 那就我先先介绍我的行业吧，希望能给点启示。
* 我是个前端工程师。俗称程序员。网页相关。
* 如这个页面。就是个什么也没有的网页。
* 我的工作就是给这种空白的页面加点儿东西。
* 嗯。说起来手机和电脑还得区分一下。
* 你现在用的是。。。${isPc ? '电脑' : '手机'}
*/

/* 首先给所有元素加上过渡效果 */
* {
  -webkit-transition: all .3s;
  transition: all .3s;
}
/* 白色背景太单调了。来点背景 */
html {
  color: rgb(222,222,222);
  background: rgb(0,43,54); 
}
/* 文字太近了 */
.styleEditor {
  padding: .5em;
  border: 1px solid;
  margin: .5em;
  overflow: auto;
  ${ isPc ? 'width: 48%;height: 96%;' : 'width: 96%;height: 50%;' }
  font-size: 14px;
  line-height:1.5;
}
/* 这些代码颜色都一样。加点儿高亮区别来 */
.token.selector{ color: rgb(133,153,0) }
.token.property{ color: rgb(187,137,0) }
.token.punctuation{ color: yellow }
.token.function{ color: rgb(42,161,152) }
.token.comment{ color: rgb(177,177,177) }
/* 加个 3D 效果 */
html{
  -webkit-perspective: 1000px;
          perspective: 1000px;
}
.styleEditor {
  position: fixed; 
  ${ isPc ? 'left: 0;' : 'left:0;right:0;margin:auto;'}
  top: 0; 
  -webkit-transition: none; 
  transition: none;   
  ${ isPc ? '-webkit-transform: rotateY(10deg) translateZ(-100px) ;transform: rotateY(10deg) translateZ(-100px) ;' : '-webkit-transform: rotateX(-10deg) translateZ(-100px) ;transform: rotateX(-10deg) translateZ(-100px) ;' }
  ${ isPc ? '' : '-webkit-transform-origin: 50% 0% 0;transform-origin: 50% 0% 0;' }
}

/* 再来一张信纸 */
.resumeEditor{
  position: fixed; 
  ${ isPc ? 'right: 0;' : 'left:0;right:0;margin:auto;'}
  ${ isPc ? 'top: 0;' : 'bottom:2%;'}
  padding: .5em;  
  ${ isPc ? 'margin: .5em;' : ''}
  ${ isPc ? 'width: 48%;height: 96%;' : 'width: 96%;height: 50%;' }
  border: 1px solid;
  color: #222;
  overflow: auto;
  font-size: 14px;
  line-height:1.5;
  ${ isPc ? '-webkit-transform: rotateY(-10deg) translateZ(-100px) ;transform: rotateY(-10deg) translateZ(-100px) ;' : '-webkit-transform: rotateX(10deg) translateZ(-100px) ;transform: rotateX(10deg) translateZ(-100px) ;' }
    ${ isPc ? '' : '-webkit-transform-origin: 50% 0% 0;transform-origin: 50% 0% 0;' }
  }
/* 我开始写了 */


`,
                `
/* 是不是看着很简陋粗糙？
 * 因为这是 Markdown 格式的
 * 一种程序员用来写文档日志的简易语言
 * 翻译成 网页 就行了
 */
`,
                `
/* 再加点样式 */
.resumeEditor{
  padding: 2em;
  line-height:1.8;
}
.resumeEditor h2{
  display: inline-block;
  border-bottom: 1px solid;
  margin: 1em 0 .5em;
  font-size:18px;
}
.resumeEditor ul,.resumeEditor ol{
  list-style: none;
}
.resumeEditor ul> li::before{
  content: '•';
  margin-right: .5em;
}
.resumeEditor ol {
  counter-reset: section;
}
.resumeEditor ol li::before {
  counter-increment: section;            
  content: counters(section, ".") " ";  
  margin-right: .5em;
}
.resumeEditor blockquote {
  margin: 1em;
  padding: .5em;
  background: rgba(221,221,221,.5);
}

/* OK。完成！ */

`
            ],
            currentMarkdown: '',
            fullMarkdown: `
----

这边写点啥呢，  

现在年轻人压力都挺大的，不管工作上还是生活上，所以还是要想办法快乐起来，

那就祝你每天都开心快乐吧，接下来请欣赏笑话十则，哈哈。

1.一天，牛给驴出了一个难题，问“蠢”字下面两只虫子哪只是公的，哪只是母的。驴绞尽脑汁，还是答不上来。牛骂道：真是头蠢驴，男左女右嘛！

2.毕业后七年，总算接了个大工程，造一根三十米烟囱，工期两个月，造价三十万，不过要垫资。总算在去年年底搞完了。今天人家去验收，被人骂得要死，还没有钱拿。
  
  图纸看反了，人家是要挖一口井！

3.一醉汉不慎从三楼掉下，引来路人围观，一警察过来：发生什么事？醉汉：不清楚，我也是刚到。

4.医生问病人是怎么骨折的。病人说，我觉得鞋里有沙子，就扶着电线杆抖鞋。有个经过那里，以为我触电了，便抄起木棍给了我两棍子！

5.乌龟受伤。让蜗牛去买药。过了2个小时。蜗牛还没回来。乌龟急了骂道：他再不回来老子就死了！这时门外传来了蜗牛的声音：你再说老子不去了！

6.某人养一猪，烦，弃之，然猪知归路，数弃无功。一日，其驾车转了很多弯弃猪，深夜致电家人，问：“猪归否？”答曰：“已归！”其怒吼：“让它接电话，老子迷路了！

7.大象不小心踩到蚂蚁窝，蚂蚁们倾巢而出，纷纷爬到大象身上。大象抖抖身子，蚂蚁们都掉了下来。此时还有一只在大象的脖子上，掉下的蚂蚁大声叫到“掐死它”。

8.一日上电脑课，有一排同学的电脑死机了。于是一位同学站起来说：“老师，电脑死机了，我们这排全死了。”这时，许多同学都说：“我们也死了。”

  这时老师问：“还有谁没死？”只有一位同学站起来：“我还没死！”老师奇怪的说：“全班都死了，你为什么不死？”

9.一猴子吃花生前都要先塞进屁股再拿出来吃。对此管理员解释道：曾有人喂它桃子，结果桃核拉不出来，猴子吓怕了，现在一定要量好再吃。

10.大二时，全宿舍的女生都喜欢周华建的歌，一盘磁带被大家借来借去的。一日，上铺的女生问：我的周华建呢？下铺的女生回答：在我床上呢！两秒钟寂静无声，然后全体翻倒在床。

`
        }
    },
    created() {
        this.makeResume()
    },

    methods: {
        makeResume: async function() {
            await this.progressivelyShowStyle(0)
            await this.progressivelyShowResume()
            await this.progressivelyShowStyle(1)
            await this.showHtml()
            await this.progressivelyShowStyle(2)
        },
        showHtml: function() {
            return new Promise((resolve, reject) => {
                this.enableHtml = true
                resolve()
            })
        },
        progressivelyShowStyle(n) {
            return new Promise((resolve, reject) => {
                let interval = this.interval
                let showStyle = (async function() {
                    let style = this.fullStyle[n]
                    if (!style) {
                        return
                    }
                    // 计算前 n 个 style 的字符总数
                    let length = this.fullStyle.filter((_, index) => index <= n).map((item) => item.length).reduce((p, c) => p + c, 0)
                    let prefixLength = length - style.length
                    if (this.currentStyle.length < length) {
                        let l = this.currentStyle.length - prefixLength
                        let char = style.substring(l, l + 1) || ' '
                        this.currentStyle += char
                        if (style.substring(l - 1, l) === '\n' && this.$refs.styleEditor) {
                            this.$nextTick(() => {
                                this.$refs.styleEditor.goBottom()
                            })
                        }
                        setTimeout(showStyle, interval)
                    } else {
                        resolve()
                    }
                }).bind(this)
                showStyle()
            })
        },
        progressivelyShowResume() {
            return new Promise((resolve, reject) => {
                let length = this.fullMarkdown.length
                let interval = this.interval
                let showResume = () => {
                    if (this.currentMarkdown.length < length) {
                        this.currentMarkdown = this.fullMarkdown.substring(0, this.currentMarkdown.length + 1)
                        let lastChar = this.currentMarkdown[this.currentMarkdown.length - 1]
                        let prevChar = this.currentMarkdown[this.currentMarkdown.length - 2]
                        if (prevChar === '\n' && this.$refs.resumeEditor) {
                            this.$nextTick(() => this.$refs.resumeEditor.goBottom())
                        }
                        setTimeout(showResume, interval)
                    } else {
                        resolve()
                    }
                }
                showResume()
            })
        }
    }
}
</script>
<style scoped>
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}

html {
    min-height: 100%;
}
.styleEditor {
    -webkit-backface-visibility: hidden;
}
</style>
