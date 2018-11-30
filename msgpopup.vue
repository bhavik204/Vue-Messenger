<template>
    <div class="msg-todo-overlay">
        <div class="msg-todo-whole-container msg-todo-popup-container-width">
            <slot></slot>
        </div>
        <div @click.native="closePop" class="close-bubble-ds btn-pos1">
            <div class="img-pos">
                <image class="img-ico1" src="https://www.materialui.co/materialIcons/content/clear_white_36x36.png"/>
            </div>
        </div>
        <!-- Mobile -->
        <!-- <div @click="closePop" class="close-bubble-ds btn-pos1 mobile">
            <div class="img-pos">
                <image class="img-ico1" src="https://www.materialui.co/materialIcons/content/clear_white_36x36.png"/>
            </div>
        </div> -->
    </div>
</template>

<script>
// import vxInputBox from '../../components/vx-input-box/vx-input-box'

export default {
    data(){
        return{
            floatBtnOn: true,
            msgPopOff: false
        }
    },
    methods:{
        closePop(){
            this.$emit('toggleEvent',this.msgPopOff);
        }
    }
    
}
    
</script>

<style>
    
    @media screen and (min-width: 320px) {
        .mobile{
            display: block
        }
        .web{
            display: none
        }
        .msg-todo-overlay {
            position: fixed;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0, 0, 0, 0.5);
            /* opacity: 0.1; */
            z-index: 10;
        }
        .msg-todo-whole-container {
            position: absolute;
            top: 50%;
            left: 50%;
            min-height: 300px;
            width: 400px;
            border-radius: 6px;
            background-color: white;
            transform: translate(-50%, -50%);
            -ms-transform: translate(-50%, -50%);
            box-shadow: 0 5px 45px rgba(0, 0, 0, .5);
        }
        .msg-todo-popup-container-width {
            width: 90%;
        }
        .close-bubble-ds {
            background: #142729;
            width: 35px;
            height: 35px;
            border-radius: 8px;
            padding: 15px;
            position: fixed;
        }

        .img-ico1 {
            width: 20px;
            height: 20px;
            cursor: pointer;
        }
        .btn-pos1 {
            bottom: 75px;
            right: 25px;
            justify-content: center;
            cursor: pointer
            /*background-color: #ddd;*/
        }
    }
    @media screen and (min-width: 768px) {
        .mobile{
            display: block
        }
        .web{
            display: none
        }
        .msg-todo-popup-container-width {
            width: 50%;
        }
        .msg-todo-overlay {
            position: fixed;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0, 0, 0, 0);
            /* opacity: 0.1; */
            z-index: 1;
        }
        .msg-todo-whole-container {
            position: absolute;
            top: 60%;
            left: 88%;
            min-height: 300px;
            width: 400px;
            border-radius: 6px;
            background-color: white;
            transform: translate(-50%, -50%);
            -ms-transform: translate(-50%, -50%);
            box-shadow: 0 5px 45px rgba(0, 0, 0, .5);
        }
        .close-bubble-ds {
            background: #142729;
            width: 35px;
            height: 35px;
            border-radius: 8px;
            padding: 15px;
            position: fixed;
        }

        .img-ico1 {
            width: 20px;
            height: 20px;
            cursor: pointer;
        }
        .btn-pos1 {
            bottom: 40px;
            right: 25px;
            justify-content: center;
            cursor: pointer
            /*background-color: #ddd;*/
        }
    }
    @media screen and (min-width: 992px) {
        .msg-todo-popup-container-width {
            width: 20%;
        }

    }

</style>

