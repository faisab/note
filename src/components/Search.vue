    <template>
        <div class="search">
            <ul>
                <input v-model="query.q" placeholder="Search" >
                <li class="folder">
                    <div>{{query}}</div>
                </li>
                
            </ul>
        </div>
    </template>

    <script>
      export default {
        name: 'Search',
        props: ['query'],
        methods: {
          search (query) {
              this.$emit('search', query)
          }
        }
      }
    </script>

    <style scoped>
        .search {
            max-width: 20rem;
            width: 30rem;
            background: #edfff0;
        }
        

        .classes {
            font-size: 6rem;
            font-family: 'Avenir', Helvetica, Arial, sans-serif;
            padding: 0.5rem 1rem;
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