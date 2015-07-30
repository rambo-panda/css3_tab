# css3_tab
Css3 Tab选项卡

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
		display : none;

		&:target{
			display : block;
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

			var hash_change = function(event, refresh){

				var hash = win.location.hash.replace('#', '');

				if(allow_hash.indexOf(hash) === -1 || !hash){
					hash = default_hash;
				}

				$('.tab_label .do_' + hash).trigger('click');

				refresh && $('#'+hash).show();

			};

			$(win).bind('hashchange', hash_change);

			hash_change(null, true);
		});

	})();

});
```
