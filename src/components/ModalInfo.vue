<script setup>
    import { ref, watch } from 'vue';

    const modalOpened = ref(false);

    // funcoes fechar & abir modal.
    function fecharModal(){ modalOpened.value = false; }
    function abrirModal(){ modalOpened.value = true;  }
    // watch no modal
    watch(modalOpened, () =>{
        if(modalOpened.value){
            document.querySelector('body').style.overflow = 'hidden';      
        } else {
            document.querySelector('body').style.overflow = 'auto';      
        }
    })
    // expor funcao para o lemento pai.
    defineExpose({
        abrirModal
    })
</script>

<template>
    <div v-if="modalOpened" class="modal">
        <div class="modal-content">
            <!-- header -->
            <div class="header">
                <p>Informacoes</p>
            </div>
            <!-- body -->
            <div class="body">
                <p class="nome-dev">Este site foi desenvolvido por agi abudos</p>
                <span class="box">
                    <p class="">Redes socias</p>
                    <span class="social-container">
                        <!-- [facebook] -->
                        <a href="https://web.facebook.com/profile.php?id=61569689648821" target="_blank">
                            <i class='fb-link bx bxl-facebook-circle' ></i>
                        </a>
                    </span>
                </span>
                
                <span class="box">
                    <p class="">Outros</p>
                    <span class="api-usada">
                       <p>API de noticias usada:</p>
                       <a href="https://newsdata.io/" target="_blank">Newsdata</a> 
                    </span>
                </span>
            </div>
            <!-- button fechar -->
            <button @click="fecharModal" class="botao-fechar">
                <i class='bx bx-x' ></i>
            </button>
        </div>
    </div>
</template>

<style scoped>
.modal{
    z-index: 99;
    position: fixed;
    top: 0;
    height: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
}
.modal-content{
    position: relative;
    border-radius: 5px;
    width: 500px;
    padding: 10px;
    background-color: #fff;
    animation: anim_abrir_modal .5s;
}
/*abir modal animacao*/
@keyframes anim_abrir_modal {
    0%{
        transform: translateY(20px);
    }
    100%{
        transform: translateY(0px);
    }
}

@media(max-width: 500px){
    .modal-content{
        width: 360px;
    }
}
.header{
    text-align: center;
    padding: 10px;
    font-size: 20px;
}
.box{
    margin-top: 10px;
    display: flex;
    flex-direction: column;
    gap: 5px;
}
.box>p{
    color: gray;
}

.social-container a{
    text-decoration: none;
}
.social-container{
    display: flex;
    gap: 10px;
}
.fb-link{
    font-size: 50px;
    color: rgb(0, 123, 255);
}
.api-usada{
    display: flex;
    gap: 5px;
}
.api-usada a{
    text-decoration: none;
}

.botao-fechar{
    top: 10px;
    right:10px;
    position: absolute;
    border-radius: 50%;
    background-color: rgb(255, 86, 86);
    color: #fff;
    border: none;
    display: flex;
    font-size: 24px;
    padding: 5px;
}
.botao-fechar:hover{
    opacity: 0.8;
}
</style>