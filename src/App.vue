    <template>
      <div id="app">
          <Folder @change-page="changePage" @new-page="newPage" :pages="pages" :activePage="index" />
          <Page @save-page="savePage" @delete-page="deletePage" :page="pages[index]" />
          <Search @search="search" :query="query" />
      </div>
    </template>



    <script>
    import Search from './components/Search'
    import Page from './components/Page'
    import Folder from './components/Folder'
    import Firebase from 'firebase'


    var database = Firebase.initializeApp({
      apiKey: 'AIzaSyB9Q7e_H9MPIk2eZdB_TnJArILVMS_Jpp0',
      authDomain: 'notebook-49e66.firebaseapp.com',
      databaseURL: 'https://notebook-49e66-default-rtdb.firebaseio.com',
      projectId: 'notebook-49e66',
      storageBucket: 'notebook-49e66.appspot.com',
      messagingSenderId: '934283410857'
    }).database().ref();

    export default {
      name: 'app',
      components: {
        Search,
        Folder,
        Page
      },
      data: () => ({
        pages: [],
        result: [],
        index: 0,
        query:""
      }),
      mounted() {
        database.once('value', (folders) => { folders.forEach((pages) => {
          pages.forEach((page) => {
            this.pages.push({
              ref: page.ref,
              title: page.child('title').val(),
              tags: page.child('tags').val(),
              content: page.child('content').val()
            })
          })
          })
        })
      },
      methods: {
        search (query) {
          console.log("This function does something")
          this.query = query
          results = database.search(query)
          console.log(results)
          console.log("This function does something")
        },
        newPage () {
          this.pages.push({
            title: '',
            tags: '',
            content: ''
          })
          this.index = this.pages.length - 1
        },
        changePage (index) {
          this.index = index
        },
        newFolder () {
          this.folders.push({
            title: '',
            pages: ''
          })
          this.indexF = this.folders.length - 1
        },
        changeFolder (index) {
          this.indexF = indexF
        },
        savePage () {
          var page = this.pages[this.index]
          if (page.ref) {
            this.updateExistingPage(page)
          } else {
            this.insertNewPage(page)
          }
        },
        updateExistingPage (page) {
          page.ref.set({
            title: page.title,
            tags: page.tags,
            content: page.content
          })
        },
        insertNewPage (page) {
          page.ref = database.push(page)
        },
        deletePage () {
          var ref = this.pages[this.index].ref
          ref && ref.remove()
          this.pages.splice(this.index, 1)
          this.index = Math.max(this.index - 1, 0)
        }
      }
    }
    </script>

    <style>
    html, body, #app {
        height: 100%;
    }

    body {
        margin: 0;
    }

    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        display: flex;
        flex-direction: row;
    }
    </style>