<template>
  <div class="add_container">
    <form @submit.prevent="submitComment()">
        <input
            class="add_input"
            type="text"
            placeholder="Add a comment"
            onfocus="this.placeholder = ''"
            onblur="this.placeholder = 'Add a comment'"
            v-model="comment"
            maxlength="300"
        >
        <div class="add_control">
          <input
            class="add_post"
            type="submit"
            value="Post"
            v-bind:class="{ disabled: comment == '' }"
            >
          <input
            class="add_cancel"
            type="button"
            value="Cancel"
            @click="comment=''"
            v-bind:class="{ disabled: comment == '' }"
          >
        </div>
    </form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      comment: ''
    }
  },
  methods: {
    submitComment () {
      this.$emit('submited', this.comment)
      this.comment = ''
    }
  }
}
</script>

<style lang='scss'>
.add_container{
    background-color: #fff;
    height: 100%;
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 5px;
    form{
        position: absolute;
        width: 100%;
        padding: 20px;
        bottom: 0;
        .add_input {
            width: 100%;
            color: #868e95;
            font-size: 1rem;
            padding: 0 0 10px 15px;
            outline: 0;
            border-width: 0 0 1px;
            border-color: #cccccc;
            transition: all 0.2s ease-in-out;
        }
        .add_input:focus {
            color: #30a64a;
            border-color: #30a64a;
        }
        .add_control{
            margin-top: 20px;
            input{
                font-size: 1rem;
                color: #868e95;
                border-radius: 5px;
                border: 1px #868e95 solid;
                background-color: transparent;
                outline: none;
                padding: 10px 15px;
                cursor: pointer;
                transition: all 0.2s ease-in-out;
            }
            input[type=range]::-moz-focus-outer {
              border: 0;
            }
            input:first-child{
                margin-right: 3px;
            }
            .add_post:hover{
                color: #fff;
                background-color: #30a64a;
                border-color: #30a64a;
            }
            .add_cancel:hover{
                color: #fff;
                background-color: #868e95;
            }
            .disabled{
              color: #cccccc;
              border: 1px #cccccc solid;
              pointer-events: none;
            }
        }
    }
}
</style>
