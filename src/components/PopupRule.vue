<template>
    <div class="wrapper-popup" v-bind:class="getClassPopup">
        <div class="rule">     
            <h3>Luật chơi</h3>
            <p>Chơi theo quy tắc xúc xắc nếu ai nhiều điểm trong thời gian quy định thì
                sẽ chiến thắng
            </p>
            <button v-on:click="confirm" class="confirm">Đã hiểu</button>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'popup-rule',
        props:{
            isOpenPopup: {type: Boolean, default: false}
        },
        data() {
            return {
                
            }
        },
        methods:{
            confirm(){
                console.log('Confirm vue')
                this.$emit('handleConfirm')
            }
        },
        computed: {
            getClassPopup(){
                return {
                    'open-popup': this.isOpenPopup
                }
            }
        }
    }
</script>

<style>
    .wrapper-popup{
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0,0,0,0.4);
        opacity: 0;
        visibility: hidden;
        transition: all .3s ease;
    }

    .wrapper-popup.open-popup{
        opacity:1;
        visibility: visible;
    }

    .rule{
        position: absolute;
        background-color: #fff;
        width: 300px;
        top: 50%;
        left: 50%;
        padding: 20px;
        transform: translate(-50%,-50%) scale(1.2);
        font-family: Arial, Helvetica, sans-serif;
    }

    .open-popup .rule{
        transform: translate(-50%,-50%) scale(1.0);
    }

    .rule h3{
        margin-bottom: 10px;
    }

    .rule .confirm{
        background-color: #fff;
        border: 2px solid #333;
        padding: 8px 15px;
        cursor: pointer;
        margin-top: 20px;
    }

</style>