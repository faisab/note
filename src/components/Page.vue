    <template>
        <div class="page">
            <div v-if="page">
                <dynamictitle/>
                <label for="title">Title:</label>
                <input type="text" v-model="page.title" class="title" name="title" placeholder="Enter a title" />
                <label for="tags">Class:</label>
                <input type="text" v-model="page.class" class="tags" name="tags" placeholder="Enters Class" />
                <label for="tags">Tags:</label>
                <input type="text" v-model="page.tags" class="tags" name="tags" placeholder="Enters Tags" />
                <label for="content">Update Note</label>
                <autocomplete :page= "page"/>
                <label for="content">Stored Note</label>
                <label class = "datetime">Last Updated: {{page.datetime}} 🕒 </label>
                <textarea class="content" name="content" v-model="page.content" placeholder="Enter some content"></textarea>
                <button @click="deletePage()">Delete Page</button>
                <button @click="savePage()">Save Page</button>
            </div>
            <div v-else>
                <h1 class="reveal-text">I'm here.</h1>
            </div>
        </div>
    </template>

    <script>
      
      import autocomplete from './autocomplete'
      import dynamictitle from './title'

      export default {
        name: 'Page',
        props: ['page'],
        data() {
            return {
                inputValue: ""
            }
        },
        components: {
            autocomplete, dynamictitle
        },
        methods: {
          deletePage () {
            this.$emit('delete-page')
          },
          savePage () {
            this.$emit('save-page')
          }, 
          newVal (inputValue) {
            page.content = this.inputValue
            console.log(page.content)
          }

        }
      }
    </script>

    <style scoped>
        

        .page {
            width: 100%;
            padding: 2rem;
            box-shadow: 3rem 0 5rem 3rem #c1f5ff;
            background-color: #f4f9f4
        }

        .content, .title, .tags {
            border-style: none;
            border-radius: 0.25rem;
            border: solid 1px rgb(220, 224, 219);
            width: 100%;
            box-sizing: border-box;
            margin-bottom: 1.25rem;

        }

        .content:focus, .title:focus {
            outline: 0;
        }

        .content {
            font-family: 'Avenir', Helvetica, Arial, sans-serif;
            resize: vertical;
            font-size: 1.5rem;
            padding: 0.5rem;
            height: 20rem;
        }

        .header {
            font-size: 4rem;
            text-align: center;
            padding: 2rem 2rem;
            color: #f4f9f4;
            background: #5c8d89;
        }

        .title {
            margin-bottom: 0.5rem;
            font-size: 2rem;
            padding: 0.5rem 1rem;
        }

        .tags {
            font-size: 2rem;
            padding: 0.5rem 1rem;
        }

        .datetime {
            font-size: 1rem;
            padding: 0.5rem 1rem;
            color: #3a6351;
        }

        .date {
            font-size: 2rem;
            padding: 0.5rem 1rem;
        }

        label {
            margin-bottom: 0.5rem;
            display: inline-block;
            color: #5c8d89;
            font-family: "Spectral", Times, serif;
            font-size: 1.5rem;
        }

        button {
            border-style: none;
            padding: 0.5rem 0.75rem;
            background-color: #488b8f;
            margin-right: 1rem;
            border-radius: 0.25rem;
            color: white;
            font-size: 1rem;
            cursor: pointer;
        }

        button:hover {
            background-color: #7bac7b;
        }
        
    </style>