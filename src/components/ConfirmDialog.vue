<template>
  <div class="confirm_container">
    <div class="confirm_dialog">
        <h2>Delete {{name}} comment?</h2>
        <div class="confirm_controls">
        <button class="confirm_delete" @click="$emit('confirm')">Delete</button>
        <button class="confirm_cancel" @click="$emit('cancel')">Cancel</button>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['name']
}
</script>

<style lang='scss'>
.confirm_container{
    display: block;
    position: absolute;
    width: 100%;
    height: 100%;
    min-width: 280px;
    min-height: 600px;
    border-radius: 5px;
    background-color: rgba(0, 0, 0, 0.3);
}
.confirm_dialog{
display: flex;
flex-direction: column;
justify-content: center;
position: absolute;
width: 80%;
height: 200px;
background-color: #fff;
top: 40%;
left: 50%;
border-radius: 5px;
transform: translate(-50%, -50%);
h2{
    color: #868e95;
    font-weight: 400;
    text-align: center;
    margin-bottom: 20px;
    padding: 0 10px;
}
.confirm_controls{
    text-align: center;
}
button{
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
button:first-child{
    margin-right: 10px;
}
.confirm_delete:hover{
    color: #fff;
    background-color: orangered;
    border-color: orangered;
}
.confirm_cancel:hover{
    color: #fff;
    background-color: #868e95;
}
}
</style>
