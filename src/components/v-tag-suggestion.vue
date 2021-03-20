<template>
  <div>
    <tag :tags="tags" :suggestions="suggestions" :suggestionLength="6" @removeItem="removeTag" @addItem="addTag"/>
  </div>
</template>

<script>
import Tag from './Tag.vue';

  export default{
    name:'v-tag-suggestion',
    data() {
    return {
      tags: [
        {
          name: "hello"
        }
      ],
      suggestions: [
        {
          name: "This"
        },
        {
          name: "is"
        },
        {
          name: "a"
        },
        {
          name: "new"
        },
        {
          name: "vue"
        },
        {
          name: "tag"
        },
        {
          name: "component"
        },
        {
          name: "which"
        },
        {
          name: "will"
        },
        {
          name: "give"
        },
        {
          name: "auto"
        },
        {
          name: "suggestion"
        }
      ],
      msg: "Welcome to Your Vue.js App"
    };
  },
  props:['tags','suggestions','suggestionLength'],
    components:{
      Tag
    },
    methods:{
      removeTag(i){
        this.tags.splice(i,1)
      },
       addTag(tag){
        this.tags.push(tag);
      }
    }
  }
</script>