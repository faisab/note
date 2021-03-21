<template>
  
    <div class="autocomplete">
      <label :for="id">{{label}}</label>
      <textarea v-if="textarea" :id="id" :rows="rows" :cols="cols" class="autocomplete-input" :placeholder="placeholder" @focusout="focusout" @focus="focus" @keydown.13="chooseItem" @keydown.tab="chooseItem" @keydown.40="moveDown" @keydown.38="moveUp" v-model="inputValue"
        type="text"></textarea>
      <input v-else :id="id" class="autocomplete-input" :placeholder="placeholder" @focusout="focusout" @focus="focus" @keydown.13="chooseItem" @keydown.tab="chooseItem" @keydown.40="moveDown" @keydown.38="moveUp" v-model="inputValue" @update = emitToParent() type="text">


      <ul :class="{
        'autocomplete-list': true,
        [id+'-list']: true
      }" v-if="searchMatch.length > 0">
        <li :class="{active: selectedIndex === index}" v-for="(result, index) in searchMatch" @click="selectItem(index), chooseItem()" v-html="highlightWord(result)">

        </li>
      </ul>
      
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
export default {
    name: "autocomplete",
	props: ["items", "placeholder", "label", "textarea", "rows", "cols", 'page'],
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
        

        if (_self.searchMatch.length > 0) {
          const element = document.querySelectorAll('.' + _self.id + '-list');

          
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
		inputValue(page) {
			this.focus();
			console.log(this.inputSplitted)
            this.emitToParent()
            this.page.content = this.inputValue
            console.log("new page content - input value" + page.content)
			this.selectedIndex = 0;
			this.wordIndex = this.inputSplitted.length - 1;
            
		}
	},
	methods: {
        emitToParent () {
              this.$emit('emit-to-parent', this.inputValue)
              console.log("autocomplete emit to parent:" + this.inputValue)
          },
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
    
}

</script>

<style scoped> 
.autocomplete {
	border-style: none;
    border-radius: 0.25rem;
    border: solid 1px rgb(220, 224, 219);
    width: 100%;
    box-sizing: border-box;
    margin-bottom: 1.25rem;
}

.content {
    border-style: none;
    border-radius: 0.25rem;
    border: solid 1px rgb(220, 224, 219);
    width: 100%;
    box-sizing: border-box;
    margin-bottom: 1.25rem;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    resize: vertical;
    font-size: 1.5rem;
    padding: 0.5rem;
    height: 20rem;
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
    border-style: none;
            border-radius: 0.25rem;
            border: solid 1px rgb(220, 224, 219);
            width: 100%;
            box-sizing: border-box;
            margin-bottom: 1.25rem;
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
        border-style: none;
    border-radius: 0.25rem;
    border: solid 1px rgb(220, 224, 219);
    width: 100%;
    box-sizing: border-box;
    margin-bottom: 1.25rem;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    resize: vertical;
    font-size: 1.5rem;
    padding: 0.5rem;
    height: 20rem;
		
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