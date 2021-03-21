<template>
        <div class="folder">
            
            <label for="title">Notes</label>
            <label for="title">--------------</label>
            <ul>
                <li v-for="(page, index) of pages " class="page" v-bind:class="{ 'active': index === activePage }" @click="changePage(index)" v-bind:key="index">
                    <div>{{page.title}}</div>
                </li> 
                <li class="new-page" @click="newPage()">New Page +</li>
            </ul>
            
        </div>
    </template>

    <script>
      export default {
        name: 'Folder',
        props: ['pages', 'activePage'],
        methods: {
          changePage (index) {
            this.$emit('change-page', index)
          },
          newPage () {
            this.$emit('new-page')
          }
        }
      }
    </script>

    <style scoped>
        .folder {
            max-width: 20rem;
            width: 30rem;
            background: #a7d7c5;
        }

        ul {
            list-style-type: none;
            padding: 0;
            margin: 0;
            height: 100%;
            position: relative;
        }

        li {
            padding: 1rem;
            font-size: 1.25rem;
            min-height: 1.5rem;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        li:hover {
            cursor: pointer;
            background-color: #c5edf5;
        }

        .active {
            background-color: #488b8f;
            color: white;
        }

        .active:hover {
            background-color: #5aa56f;
            color: white;
        }
        
        .title {
            background-color: #488b8f;
            color: white;
            position: absolute;
            bottom: 1%;
            width: 100%;
            box-sizing: border-box;
            font-size: 2rem;
        }

        .new-page {
            background-color: #488b8f;
            color: white;
            position: relative;
            bottom: 0%;
            width: 100%;
            box-sizing: border-box;
        }

        .new-page:hover {
            background-color: #5a9440;
        }

        
        label {
            margin-bottom: 2rem;
            display: inline-block;
            font-size: 2rem;
            margin-right: 2rem;
            margin-left: 1rem;
            margin-top: 2rem;
            color: #000;
            font-family: 'Fira Code', monospace;
        }
    </style>