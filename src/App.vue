    <template>
      <div id="app">
          <Folder @change-page="changePage" @new-page="newPage" :pages="pages" :activePage="index" />
          <Page @save-page="savePage" @delete-page="deletePage" :page="pages[index]" />
          <Search @emit-to-parent="search" :childMessage="query"/>
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
        query:""
      }),
      mounted() {
        database.once('value', (pages) => {
          pages.forEach((page) => {
            this.pages.push({
              ref: page.ref,
              title: page.child('title').val(),
              tags: page.child('tags').val(),
              content: page.child('content').val()
            })
          })
        })
      },
      methods: {
        search (query, results) {
          console.log("This function does something")
          console.log("query" + query)
          var ref = database;
          var test;
          
          ref.orderByChild("title").on("child_added", function(snapshot) {
            console.log(snapshot.val().title + " was the title and " + snapshot.val().tags + " is the tags");
          });
          ref.orderByChild('title').once('child_added', function(snapshot) {
            test = (snapshot.val().tags);
            
            // do something with documents
          }).then(function() {
            console.log("test" + test);
            results = test;
            console.log("results" + results);
          })
         
          
        },
        newPage () {
          this.pages.push({
            title: '',
            content: ''
          })
          this.index = this.pages.length - 1
        },
        changePage (index) {
          this.index = index
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