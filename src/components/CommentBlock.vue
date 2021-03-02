<template>
    <div class="chat_container">
        <div class="chat_wrap">
            <div class="chat_top" v-bind:class="{ shrink: earlierCount <= 0 }">
                <p class="chat_earlier" v-if="earlierCount > 0"> <span @click="addComments">{{earlierCount}}</span> earlier comments</p>
            </div>
            <div class="chat_comments" ref="Comments" v-bind:class="{ push: earlierCount <= 0 }">
                <h2 class="chat_empty" v-if="!comments.length">Be the first one to comment!</h2>
                <Comment
                  v-for="comment in comments.slice(displayCount)" v-bind:key="comment.id"
                  v-bind:userIcon="comment.img"
                  v-bind:userName="comment.name"
                  v-bind:time="comment.time"
                  v-bind:userComment="comment.comment"
                  v-bind:id="comment.id"
                  @delete="openDialog"
                  />
            </div>
            <div class="chat_comment">
                <AddComment @submited="storeComment"/>
            </div>
            <ConfirmDialog v-if="showDialog" v-bind:name="itemName" @confirm="deleteComment" @cancel="closeDialog"/>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import Comment from './Comment.vue'
import AddComment from './AddComment.vue'
import ConfirmDialog from './ConfirmDialog.vue'
export default {
  components: {Comment, AddComment, ConfirmDialog},
  data () {
    return {
      displayCount: 0,
      earlierCount: 0,
      newComment: false,
      showDialog: false,
      comments: [],
      itemID: undefined,
      itemName: '',
      id: 0,
      name: 'Guest',
      img: 'https://icon-library.com/images/no-user-image-icon/no-user-image-icon-23.jpg'
    }
  },
  watch: {
    comments: function () {
      this.earlierCount = this.comments.length + this.displayCount
    }
  },
  mounted () {
    this.getComments()
    this.getUser()
  },
  updated () {
    if (this.newComment) {
      this.$refs.Comments.scrollTop = this.$refs.Comments.scrollHeight
      this.newComment = false
    }
  },
  methods: {
    getUser () {
      axios.get('https://randomuser.me/api/')
        .then((resp) => {
          this.name = `${resp.data.results[0].name.first} ${resp.data.results[0].name.last}`
          this.img = resp.data.results[0].picture.thumbnail
        })
    },
    getComments () {
      if (localStorage.comments) {
        this.comments = JSON.parse(localStorage.getItem('comments'))
        this.id = this.comments[this.comments.length - 1].id + 1
        if (this.comments.length > 4) {
          this.displayCount = -4
        } else {
          this.displayCount = -(this.comments.length)
        }
      }
    },
    storeComment (text) {
      if (text !== '') {
        this.comments.push(
          {
            id: this.id,
            name: this.name,
            img: this.img,
            time: new Date().toLocaleString('en-US', { hour: 'numeric', minute: 'numeric', hour12: true }),
            comment: text
          }
        )
        if (this.displayCount > -4) {
          this.displayCount--
        }
        localStorage.setItem('comments', JSON.stringify(this.comments))
        this.id++
        this.newComment = true
      }
    },
    addComments () {
      this.displayCount = this.displayCount - 20
      this.earlierCount = this.earlierCount - 20
      this.$refs.Comments.scrollTop = 0
    },
    openDialog (id, name) {
      this.itemID = id
      this.itemName = name
      this.showDialog = true
    },
    closeDialog () {
      this.itemID = undefined
      this.showDialog = false
    },
    deleteComment () {
      for (let i = 0; i < this.comments.length; i++) {
        if (this.comments[i].id === this.itemID) {
          this.comments.splice(i, 1)
          this.itemID = undefined
          this.showDialog = false
          break
        }
      }
      localStorage.setItem('comments', JSON.stringify(this.comments))
    }
  }
}
</script>

<style lang="scss">
  .chat_container{
      font-family: 'Open Sans', sans-serif;
      color: #232323;
      display: flex;
      width: 100vw;
      height: 100vh;
      min-width: 280px;
      min-height: 600px;
      flex-direction: column;
      align-items: center;
      justify-content: center;

      .chat_wrap{
          position: relative;
          display: flex;
          flex-direction: column;
          width: 100%;
          height: 100%;
          max-width: 500px;
          max-height: 800px;
          background-color: #f4f4f4;
          border-radius: 5px;
          -webkit-box-shadow: 0px 2px 9px 1px rgba(0,0,0,0.25);
          -moz-box-shadow: 0px 2px 9px 1px rgba(0,0,0,0.25);
          box-shadow: 0px 2px 9px 1px rgba(0,0,0,0.25);

          .chat_top{
              height: 60px;
              p{
                padding: 20px 20px;
              }
              span{
                padding: 0 2px;
                cursor: pointer;
              }
              span:hover{
                  color: #30a64a;
                  font-weight: 700;
              }
          }

          .chat_comments{
              height: calc(100% - 210px);
              display: flex;
              flex-direction: column;
              justify-content: start;
              overflow-y: auto;
              scroll-behavior: smooth;
              .chat_empty{
                  text-align: center;
                  margin: auto;
                  color: #868e95;
                  font-weight: 400;
              }
          }
          .chat_comment{
              position: absolute;
              height: 150px;
              width: 100%;
              bottom: 0;
          }
          .push {
              margin-top: 3px;
              height: calc(100% - 153px);
              transition: all 0.5s ease-in-out;
          }
          .shrink {
              height: 0;
              padding: 0;
              transition: all 0.5s ease-in-out;
          }
      }
      @media screen and (max-width: 600px) {
        .push {
            margin-top: 0!important;
            height: calc(100% - 150px)!important;
        }
        .chat_wrap{
            max-height: 100%!important;
            max-width: 100%!important;
        }
      }
  }
</style>
