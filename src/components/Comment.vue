<template>
  <div @mouseover="dispDelete=true" @mouseout="dispDelete=false" class="comment_container">
      <div class="comment_image_wrap">
        <img class="comment_image" :src="userIcon" alt="Profile Image">
      </div>
      <div class="comment_text_wrap">
      <div class="comment_top">
          <h1>{{userName}}</h1>
          <p>{{time}}</p>
          <span @click="deleteComment()">
          <Icon class="comment_delete" name="x" v-show="dispDelete"></Icon>
          </span>
      </div>
      <p class="comment_content">{{userComment}}</p>
      </div>
  </div>
</template>

<script>
import Icon from 'vue-icon/lib/vue-feather.esm'
export default {
  props: ['userIcon', 'userName', 'time', 'userComment', 'id'],
  components: {Icon},
  data () {
    return {
      dispDelete: false
    }
  },
  methods: {
    deleteComment () {
      this.$emit('delete', this.id, this.userName)
    }
  }
}
</script>

<style lang="scss">
.comment_container{
    position: relative;
    display: flex;
    padding: 20px;
    color: #232323;
    .comment_image_wrap{
      .comment_image{
          width: 55px;
          border-radius: 50%;
      }
    }
    .comment_text_wrap{
        padding: 0 15px;
        .comment_top{
            padding-bottom: 5px;
            display: flex;
            h1{
                font-size: 1rem;
                vertical-align: top;
            }
            p{
                font-size: 0.9rem;
                color: #7a7a7a;
                margin: 0 15px;
                line-height: 1.65;
                white-space: nowrap;
            }
            .comment_delete{
              width: 20px;
              color: #7a7a7a;
              position: absolute;
              margin-top: 2px;
              right: 20px;
              cursor: pointer;
            }
            .comment_delete:hover{
              color: orangered
            }
        }
    }
    @media screen and (max-width: 300px) {
      .comment_top{
        p{
          margin: 0 10px!important;
        }
      }
    }
}
</style>
