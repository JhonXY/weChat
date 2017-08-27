<template>
  <div class="hello">
    <article class="block post wysiwyg" v-for="item in msg">
      <h2>{{item.title}}</h2>
      <p class="article-meta">发布于 {{item.createDate}}</p>
      <div class="ui ribbon label red">
        <a href="">{{item.tag}}</a>
      </div>
      <div class="abstract" v-html="item.content.html">
      </div>
    </article>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'hello',
  data () {
    return {
      msg: [],
      page: 1,
      pageSize: 10,
      count: 0
    }
  },
  mounted () {
    this.init()
  },
  methods: {
    init () {
      let $http = {
        get (url) {
          let promise = new Promise ((resolve, reject)=>{
            let param = {
              page: this.page,
              pageSize: this.pageSize
            }
            axios.get(url, {
              params: param
            }).then((result)=>{
              let res = result.data
              if (res.status == "0") {
                if (res.result.count == 0) {
                  this.page -= 1
                  return
                } else {
                  resolve(res.result.list)
                }
              } else {
                reject(res.status)
              }
            })
          })
          return promise
        }
      }

      $http.get('/api/articleList').then((res)=>{
            this.msg = res;
        }, (err)=>{
            document.write(err);
        })
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
