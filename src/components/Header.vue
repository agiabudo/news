<script setup>
    import { ref } from 'vue';
    import ModalInfo from './ModalInfo.vue';
    
    const modalInfo = ref(null);
    
    function abrir_modalInfo(){
        modalInfo.value.abrirModal();
    }
</script>

<template>
    <header>
        <div class="container">
            <p>Noticias</p>
            <button @click="abrir_modalInfo"><i class='bx bx-info-circle' ></i></button>
        </div>
    </header>
    <ModalInfo ref="modalInfo" />
</template>

<style scoped>
    .container{
        display: flex;
        align-items: center;
        gap: 10px;
    }
    .container button{
        display: flex;
        border: none;
        background-color: transparent;
        font-size: 32px;
        animation: anim_button 1s infinite;
    }
    .container button:hover{
        opacity: 0.3;
    }
    /* animacao icon */
    @keyframes anim_button {
        0%, 50%{
           color: rgb(255, 157, 0);
        }
        100%{
            color: rgb(0, 157, 255);
        }
    }
    header{
        z-index: 1;
        top: 0;
        left: 0;
        position: fixed;
        width: 100%;
        height: 80px;
        display: flex;
        align-items: center;
        justify-content: center;
        border-bottom: 1px solid rgb(221, 221, 221);
        background-color: #fff;
    }
    p{
        font-weight: 600;
        font-size: 28px;
        color: rgb(0, 106, 255);
    }

</style>