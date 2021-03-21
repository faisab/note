<template>
  <script type="text/x-template" id="autocomplete-tpl">
    <div class="autocomplete">
      <label :for="id">{{label}}</label>
      <textarea v-if="textarea" :id="id" :rows="rows" :cols="cols" class="autocomplete-input" :placeholder="placeholder" @focusout="focusout" @focus="focus" @keydown.13="chooseItem" @keydown.tab="chooseItem" @keydown.40="moveDown" @keydown.38="moveUp" v-model="inputValue"
        type="text"></textarea>
      <input v-else :id="id" class="autocomplete-input" :placeholder="placeholder" @focusout="focusout" @focus="focus" @keydown.13="chooseItem" @keydown.tab="chooseItem" @keydown.40="moveDown" @keydown.38="moveUp" v-model="inputValue" type="text">


      <ul :class="{
        'autocomplete-list': true,
        [id+'-list']: true
      }" v-if="searchMatch.length > 0">
        <li :class="{active: selectedIndex === index}" v-for="(result, index) in searchMatch" @click="selectItem(index), chooseItem()" v-html="highlightWord(result)">

        </li>
      </ul>
    </div>
</script>


<div id="app">

 <div class="container">
  <div class="container-row">
   <div class="container-item">
    <h2>VueJs - Autocomplete</h2>
    <p>A small proof of concept of an autocomplete<br />component in VueJS. Use your arrow keys to navigate,<br /> tab to choose first result, <br />enter or mouse to choose selected item.</p>
   </div>


   <div class="container-item">
    <autocomplete label="Standard items:" placeholder="span, div, h1..." />
   </div>

   <div class="container-item">
    <br />
    <br />
    <autocomplete label="Custom items:" :items="['all', 'auto', 'complete', 'items']" placeholder="all, auto..." />
   </div>
		<div class="container-item">
    <br />
    <br />
    <autocomplete label="Textarea" rows="5" placeholder="Your text..." textarea="true" />
   </div>
  </div>
 </div>
</div>

</template>

<script> 
// TODO: collision detection, autoscroll
// new: tab and highlighting, textareas, follow caret
const standardItems = [
	"input",
	"h1",
	"h2",
	"h3",
	"h4",
	"h5",
	"h6",
	"span",
	"div",
	"textarea",
	"margin",
	"padding",
	"display",
	"background",
	"background-color",
	"background-size",
	"background-repeat",
	"position",
	"top",
	"left",
	"right",
	"bottom"
];
const Autocomplete = Vue.component("autocomplete", {
	template: "#autocomplete-tpl",
	props: ["items", "placeholder", "label", "textarea", "rows", "cols"],
	data() {
		return {
			id: 'input-' + parseInt(Math.random() * 1000),
			inputValue: "",
			searchMatch: [],
			selectedIndex: 0,
			clickedChooseItem: false,
			wordIndex: 0
		};
	},
	mounted() {
    const _self = this;
    document.querySelector('#' + this.id)
      .addEventListener('input', function() {
        const caret = getCaretCoordinates(this, this.selectionEnd);

        if (_self.searchMatch.length > 0) {
          const element = document.querySelectorAll('.' + _self.id + '-list');

          if (element[0]) {
            element[0].style.top = caret.top + 40 + 'px';
            element[0].style.left = caret.left + 'px';
          }
        }
      });
  },
	computed: {
		listToSearch() {
			if (typeof this.items !== "undefined" && this.items.length > 0) {
				return this.items;
			} else {
				return standardItems;
			}
		},
		currentWord() {
			return this.inputValue.replace(/(\r\n|\n|\r)/gm, ' ').split(' ')[this.wordIndex];
		},
		inputSplitted() {
			return this.inputValue.replace(/(\r\n|\n|\r)/gm, ' ').split(" ");
		}
	},
	watch: {
		inputValue() {
			this.focus();
			console.log(this.inputSplitted)
			this.selectedIndex = 0;
			this.wordIndex = this.inputSplitted.length - 1;
		}
	},
	methods: {
		highlightWord(word) {
			const regex = new RegExp("(" + this.currentWord + ")", "g");
			return word.replace(regex, '<mark>$1</mark>');
		},
		setWord(word) {
			let currentWords = this.inputValue.replace(/(\r\n|\n|\r)/gm, '__br__ ').split(' ');
			currentWords[this.wordIndex] = currentWords[this.wordIndex].replace(this.currentWord, word + ' ');
			this.wordIndex += 1;
			this.inputValue = currentWords.join(' ').replace(/__br__\s/g, '\n');
		},
		moveDown() {
			if (this.selectedIndex < this.searchMatch.length - 1) {
				this.selectedIndex++;
			}
		},
		moveUp() {
			if (this.selectedIndex !== -1) {
				this.selectedIndex--;
			}
		},
		selectItem(index) {
			this.selectedIndex = index;
			this.chooseItem();
		},
		chooseItem(e) {
			this.clickedChooseItem = true;

			if (this.selectedIndex !== -1 && this.searchMatch.length > 0) {
				if (e) {
					e.preventDefault();
				}
				this.setWord(this.searchMatch[this.selectedIndex]);
				this.selectedIndex = -1;
			}
		},
		focusout(e) {
			setTimeout(() => {
				if (!this.clickedChooseItem) {
					this.searchMatch = [];
					this.selectedIndex = -1;
				}
				this.clickedChooseItem = false;
			}, 100);
		},
		focus() {
			this.searchMatch = [];
			if (this.currentWord !== "") {
				this.searchMatch = this.listToSearch.filter(
					el => el.indexOf(this.currentWord) >= 0
				);
			}
			if (
				this.searchMatch.length === 1 &&
				this.currentWord === this.searchMatch[0]
			) {
				this.searchMatch = [];
			}
		}
	}
});

new Vue({
	el: "#app"
});


// Thanks: https://github.com/component/textarea-caret-position
(function(){function e(b,e,f){if(!h)throw Error("textarea-caret-position#getCaretCoordinates should only be called in a browser");if(f=f&&f.debug||!1){var a=document.querySelector("#input-textarea-caret-position-mirror-div");a&&a.parentNode.removeChild(a)}a=document.createElement("div");a.id="input-textarea-caret-position-mirror-div";document.body.appendChild(a);var c=a.style,d=window.getComputedStyle?window.getComputedStyle(b):b.currentStyle,k="INPUT"===b.nodeName;c.whiteSpace="pre-wrap";k||(c.wordWrap=
"break-word");c.position="absolute";f||(c.visibility="hidden");l.forEach(function(a){k&&"lineHeight"===a?c.lineHeight=d.height:c[a]=d[a]});m?b.scrollHeight>parseInt(d.height)&&(c.overflowY="scroll"):c.overflow="hidden";a.textContent=b.value.substring(0,e);k&&(a.textContent=a.textContent.replace(/\s/g,"\u00a0"));var g=document.createElement("span");g.textContent=b.value.substring(e)||".";a.appendChild(g);b={top:g.offsetTop+parseInt(d.borderTopWidth),left:g.offsetLeft+parseInt(d.borderLeftWidth),height:parseInt(d.lineHeight)};
f?g.style.backgroundColor="#aaa":document.body.removeChild(a);return b}var l="direction boxSizing width height overflowX overflowY borderTopWidth borderRightWidth borderBottomWidth borderLeftWidth borderStyle paddingTop paddingRight paddingBottom paddingLeft fontStyle fontVariant fontWeight fontStretch fontSize fontSizeAdjust lineHeight fontFamily textAlign textTransform textIndent textDecoration letterSpacing wordSpacing tabSize MozTabSize".split(" "),h="undefined"!==typeof window,m=h&&null!=window.mozInnerScreenX;
"undefined"!=typeof module&&"undefined"!=typeof module.exports?module.exports=e:h&&(window.getCaretCoordinates=e)})();
</script>

<style scoped> 
body {
	height: 100%;
	min-height: 800px;
}
html {
	position: relative;

	width: 100%;
	min-height: 800px;
	height: 100%;
	&:after, &:before {
		position: absolute;

		width: 100%;
		height: 10px;

		content: '';
		z-index: 5;
	}
	&:after {
		top: 0;

		background-color: #4fc08d;
	}
	&:before {
		bottom: 0;

		background-color: #35495e;
	}
}
#app {
	height: 100%;
	margin: 0;
	h2 {
		margin: 0;

		font-weight: 100;
	}
	p {
		margin: 5px 0 30px;

		color: #999;

		font-weight: 100;
		line-height:24px;
		font-size: 15px;
	}
}

.container {
	display: flex;

	height: 100%;

	align-items: center;
	justify-content: center;
	&-row {
		width: auto;
		padding: 25px;

		border-radius: 5px;
		background-color: #f7f7f7;
		box-shadow: 0px 40px 100px -20px rgba(0, 0, 0, 0.1);
	}
	&-item {
		width: 100%;
	}
}


// AUTOCOPMPLETE SCSS
.autocomplete {
	position: relative;
	label {
		display: block;

		margin-bottom: 5px;

		font-size: 14px;
		font-weight: 100;
	}
	&-input {
		padding: 7px 10px;
		width: 93%;
		border: 1px solid #ddd;
		border-radius: 4px;
		outline: none;
		&:focus {
			border-color: #000;
		}
	}
	&-list {
		position: absolute;
		z-index: 2;

		overflow: auto;

		min-width: 250px;
		max-height: 150px;
		margin: 0;
		margin-top: 5px;
		padding: 0;
		
		border: 1px solid #eee;

		list-style: none;

		border-radius: 4px;
		background-color: #fff;
		box-shadow: 0 5px 25px rgba(0, 0, 0, 0.05);
		li {
			margin: 0;
			padding: 8px 15px;

			border-bottom: 1px solid #f5f5f5;
			&:last-child {
				border-bottom: 0;
			}
			&:hover, &.active {
				background-color: #f5f5f5;
			}
		}
	}
}


</style>