<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <span> from {{ appName }} running on {{ platform }} </span>
    <p>File content is : {{ content }}
    <button v-on:click="read()">do read this shit</button>
  </div>
</template>

<script>
  import { remote } from 'electron'
  import os from 'os'
  import fs from 'fs'

  export default {
    data () {
      return {
        platform: os.platform(),
        appName: remote.app.getName(),
        content: 'default text',
        // note: changing this line won't causes changes
        // with hot-reload because the reloaded component
        // preserves its current state and we are modifying
        // its initial state.
        msg: 'Hello World!'
      }
    },

    methods: {
      read: function () {
        var self = this
        fs.readFile('/Users/ikwattro/dev/_file.txt', 'utf8', function (err, data) {
          if (err) {
            return console.log(err)
          }
          console.log(data)
          self.content = data
        })
        var driver = global.neo4j.v1.driver('bolt://localhost')
        var session = driver.session()
        session.run('CREATE (n:Electron) RETURN n')
          .subscribe({
            onNext: function (record) {
              console.log(record._fields)
            },
            onCompleted: function () {
              session.close()
            },
            onError: function (error) {
              console.log(error)
            }
          })
      }
    }
  }
</script>
