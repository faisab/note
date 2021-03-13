<template>
</template>


<script>

import Firebase from "firebase"

var database = Firebase.initializeApp({
      apiKey: 'AIzaSyB9Q7e_H9MPIk2eZdB_TnJArILVMS_Jpp0',
      authDomain: 'notebook-49e66.firebaseapp.com',
      databaseURL: 'https://notebook-49e66-default-rtdb.firebaseio.com',
      projectId: 'notebook-49e66',
      storageBucket: 'notebook-49e66.appspot.com',
      messagingSenderId: '934283410857'
    }).database().ref();

export default {
        name: 'Database',
        props: ['database'],
      }

</script>

