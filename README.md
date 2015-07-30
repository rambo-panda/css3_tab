# css3_tab
Css3 Tab选项卡

 通过css3 `:checked` `:target` 相结合来达到`Tab选项卡效果`!


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
