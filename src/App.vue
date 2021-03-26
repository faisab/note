    <template>
      <div id="app">
          <Folder @change-page="changePage" @new-page="newPage" :pages="pages" :activePage="index" />
          <Page @save-page="savePage" @forceRerender="forceRerender" @delete-page="deletePage" :page="pages[index]" :componentKey = "componentKey"/>
          <Search @emit-to-parent="search" :childMessage="query" :results = "results" @change-page="changePage"/>
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
        results: "",
        index: 0,
        query:"",
        componentKey: 0
      }),
      mounted() {
        database.once('value', (pages) => {
          pages.forEach((page) => {
            this.pages.push({
              ref: page.ref,
              title: page.child('title').val(),
              tags: page.child('tags').val(),
              class: page.child('class').val(),
              datetime: page.child('datetime').val(),
              indexSearch: page.child('index').val(),
              content: page.child('content').val()
            })
          })
        })
      },
      methods: {
        search (query, results) {
          console.log("query" + query)
          var ref = database;
          var test = [];
          switch (query) {
            case "datetime":
              ref.orderByChild('datetime').on('child_added', function(snapshot) {
              test.push([snapshot.val().title, snapshot.val().indexSearch]);
              })
              break;
            case "title":
              ref.orderByChild('title').on('child_added', function(snapshot) {
              test.push(snapshot.val().title);
              })
              break;
            case "class":
              ref.orderByChild('tags').on('child_added', function(snapshot) {
              test.push(snapshot.val().title);
              })
              break;   
            case "tags":
              ref.orderByChild('tags').on('child_added', function(snapshot) {
              test.push(snapshot.val().title);
              })
              break;      
          }
          /*
          ref.orderByChild("title").on("child_added", function(snapshot) {
            console.log(snapshot.val().title + " was the title and " + snapshot.val().tags + " is the tags");
          });
          */
          this.results = test
        },
        newPage () {
          this.pages.push({
            title: '',
            content: '',
            indexSearch: this.pages.length - 1
          })
          this.index = this.pages.length - 1
          this.pages[this.index].indexSearch = this.pages.length - 1
        },
        changePage (index) {
          this.index = index
          this.forceRerender()
        },
        savePage () {
          var page = this.pages[this.index]
          if (page.ref) {
            this.updateExistingPage(page)
          } else {
            this.insertNewPage(page)
          }
          this.forceRerender()
        },
        forceRerender () {
          console.log("autocomplete has been reset")
          this.componentKey += 1;
        },
        updateExistingPage (page) {
          page.ref.set({
            title: page.title,
            tags: page.tags,
            class: page.class,
            datetime: new Date().toLocaleString(),
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
        font-family: 'Fira Code', monospace;
        display: flex;
        flex-direction: row;
    }
    </style>