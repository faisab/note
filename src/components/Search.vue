    <template>
        <div class="search">
            <input type="text" v-model="childMessage" placeholder="Search" >
            <button @click="emitToParent()" class="button">Search</button>
            <ul>      
                <div v-if="results" class="page">
                    <li v-for="result of results" class="page" @click="changePage(result)" >
                        <div> {{ result[0] }}</div>
                    </li> 
                </div>
            </ul>
        </div>
    </template>

    <script>
      export default {
        name: 'Search',
        data() {
            return {
                childMessage: ''
            }
        },
        props: ['results'],
        methods: {
          emitToParent () {
              this.$emit('emit-to-parent', this.childMessage)
          }, 
          changePage (result) {
            this.$emit('change-page', result[1])
            console.log(result)
          }
        }
      }
    </script>

    <style scoped>

        .search {
            max-width: 20rem;
            width: 30rem;
            background: #a7d7c5;
            display: inline-block;
        }
        

        .classes {
            font-size: 6rem;
            font-family: 'Avenir', Helvetica, Arial, sans-serif;
            padding: 0.5rem 1rem;
        }

        .results {
            padding: 2rem;
            box-shadow: 0rem 0 0rem 0rem #c1f5ff;
            background-color: #f4f9f4
            
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
            background-color: #3a6e45;
            color: white;
        }

        .button {
            display: inline-block;
        }

        .active:hover {
            background-color: #5aa56f;
            color: white;
        }

        .new-page {
            background-color: #065c11;
            color: white;
            position: absolute;
            bottom: 0;
            width: 100%;
            box-sizing: border-box;
        }

        .new-page:hover {
            background-color: #5a9440;
        }

        .new-folder {
            background-color: #065c11;
            color: white;
            position: absolute;
            bottom: 0;
            width: 100%;
            box-sizing: border-box;
        }

        .new-folder:hover {
            background-color: #5a9440;
        }
    </style>