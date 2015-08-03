# css3_tab
Css3 Tab选项卡

 通过css3 `:checked` `:target` 相结合来达到`Tab选项卡效果`!

Tab页面:
	需求:  就是一个Tab效果 要求刷新的时候 还是这个Tab
	分析难点: 就后面那个 刷新的时候还是那个Tab
	解答: (因为这些代码很常见是吧 就不写代码了)
		1: 使用 css3 + hashChange  --- https://github.com/rambo-panda/css3_tab.git
			优:
				页面不刷新，对页面的压力小，不用每次都请求数据库 重新渲染
				css3 hashChange 对页面代码依赖小 易于扩展

			劣:
				【浏览器兼容性】 比如chrome44 会出现的 script标签 不能与 :target 伪类使用
				页面不刷新 也许会造成数据缓存大
				一个新技术

		2: 使用最传统的原则 后面加查询参数 然后在Ejs里渲染
			优:
				枯燥的代码，学习成本最小的代码. 最可靠的代码 履行 一个url对应一个资源的 行业标准
				依赖最少 没有js css我照样能让你看到数据的完整性
				每次都去服务器拿最新的代码. 保证数据的正确性

			劣:
				每次都去服务器拿最新的代码,造成服务器压力过大。
				扩展性差
				可维护性较差

	教训:
		1: 绝对不要在产品上使用任何未经大量测试的 **新技术**
		2: 最老套的最枯燥往往是最可靠的   web开发准则: 没有css 和 js，我仍然可以看到[数据的完整性]  css js 是建立在html之上.



##注意点

*  在**Chrome/44.0.2403.125** `:target` 在刷新页面的时候，如果页面存在 `<script>`  则 `:target` **失效**。 这就是JavaScript中为什么加一个 `refresh`  貌似只在`chrome && version < 44`上看到这个问题  Chrome/46.0.2471.0  已经没有这个问题
*  需要注意在没有 `location.hash` 以及 `location.hash` 不在合法范围的时候该怎么处理？

#### Html 结构
``` html
<div class="tab_dom">

	<div class="tab_labels">
		<input type="radio" class="do_aaa" name="tab_helpful">
		<a class="tab_label" href="#aaa">AAA</a>

		<input type="radio" class="do_bbb" name="tab_helpful">
		<a class="tab_label" href="#bbb">BBB</a>

		<input type="radio" class="do_ccc" name="tab_helpful">
		<a class="tab_label" href="#ccc">CCC</a>
	</div>

	<div class="tab_content">
		<div id="aaa">this is AAA content</div>

		<div id="bbb">this is BBB content</div>

		<div id="ccc">this is CCC content</div>
	</div>

<div>
```

#### Scss结构
``` scss
.tab_dom{

	.tab_labels{

		input[type="radio"]{

			position : fixed;
			left : -1000px;

			&:checked{
				+a{
					color : red;
				}
			}
		}

	}

	.tab_content{
		display : none !important;

		&:target{
			display : block !important;
		}
	}

}
```

#### JavaScript 结构
``` javascript
$(function(){

	(function(){

		$('.tab_dom').exists(function(){
			var allow_hash = $(this).find('.tab_label a').map(function(){
					return $(this).attr('href').replace('#', '');
				}).toArray(),
				default_hash = 'aaa';

			var hash_change = function(event){

				var hash = win.location.hash.replace('#', '');

				if(allow_hash.indexOf(hash) === -1){
					location.hash=default_hash;
					return;
				}

				$('.tab_label .do_' + hash).trigger('click');

			};

			$(win).bind('hashchange', hash_change).trigger('hashchange');

		});

	})();

});
```
