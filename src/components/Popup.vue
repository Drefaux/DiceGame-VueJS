<template>
  <div class="wrapper-popup" v-bind:class="getClassPopup">
    <div class="rule">
        <h1>Luật chơi</h1>
        <p>Chọn số điểm để chiến thắng và thay phiên nhau roll xúc xắc, bạn có thể hold số điểm hiện tại
            khi gần đến số điểm cần đạt được. Nếu bạn quay trúng số 1 thì sẽ mất lượt chơi. Người chơi nào
            đạt được số điểm trước sẽ chiến thắng.
        </p>
         <button v-on:click="confirm" class="confirm">Đã hiểu</button>
    </div>
  </div>
</template>

<script>
export default {
    name: 'popup',
    props: {
        isOpenPopup: { type:Boolean, default: false }
    },
    data() {
        return {

        }
    },
    methods: {
        confirm() {
            this.$emit('handleConfirm')    
        }
    },
    computed: {
        getClassPopup: function() {
            return {
                'open-popup' : this.isOpenPopup
            }
        },
    }
}
</script>

<style>
    .wrapper-popup {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0, 0, 0, 0.4);
        opacity: 0;
        visibility: hidden;
        transition: all .3s ease;
    }
    .wrapper-popup.open-popup{
        opacity: 1;
        visibility: visible;
        transition: all .3s ease;
    }
    .rule {
        width: 350px;
        padding: 20px;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%) scale(1.2);
        position: absolute;
        background-color: #fff;
        font-family: Arial, Helvetica, sans-serif;
        transition: all .3s ease;
    }
    .open-popup .rule{
        transform: translate(-50%, -50%) scale(1);
    }
    .rule h1 {
        margin-bottom: 10px;
    }
    .rule .confirm {
        cursor: pointer;
        margin-top: 20px;
        padding: 8px 15px;
        border: 2px solid #333;
        background-color: #fff;
        transition: all .3s ease;
    }
    .rule .confirm:hover {
        background-color: #333;
        color: #fff;
    }
</style>