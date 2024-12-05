<script setup>
    import { onMounted, ref } from 'vue';
    import Noticia from './Noticia.vue';
    
    // variaveis constantes.
    const noticias          = ref([]);
    const carregando        = ref(false);
    const carregando_erro   = ref(false);
    const filtroSelecionado = ref(0);
    // funcao: Recarregar noticias
    function reload() { filtroNoticia(filtroSelecionado.value); }
    // funcao: Buscar noticias (API)
    async function filtroNoticia(filtroid){
        filtroSelecionado.value = filtroid;

        const countrys = ['br', 'us', 'mz'];
        const country  = countrys[filtroid];
        const apiKey   = 'pub_61295e260407906027d97f9a1ca5f06d5e4cb';
        const endpoint = `https://newsdata.io/api/1/news?apikey=${apiKey}&country=${country}`;
       
        try{
            // variaveis
            noticias.value = [];
            carregando.value = true;
            carregando_erro.value = false;
            // request
            const response = await fetch(endpoint);
            const data = await response.json();
            // tratamento dos dados.
            data.results.forEach((art, index) => {
                noticias.value.push({
                    id:         index,
                    autor:      (art.creator==null) ? 'Desconhecido' : art.creator[0],
                    titulo:     art.title,
                    descricao:  art.v,
                    publicado:  art.pubDate,
                    src_imagem: art.image_url,
                    link:       art.link,
                })                
            });
        } catch(error) {
            console.error("error na requisicao:"+error);
            carregando_erro.value = true;
        } finally {
            carregando.value = false;
        }
    }

    onMounted(() =>{
        filtroNoticia(filtroSelecionado.value);
    })

</script>

<template>

    <section>
        <p>Noticias</p>
        <!-- Filtro -->
        <div class="container-filtro">
            <p>Filtros</p>
            <div class="option">
                <!-- Brasil -->
                <button 
                :class="{selected:(filtroSelecionado==0) ? true : false}"
                @click="filtroNoticia(0)">Brasil</button>
                <!-- USA -->
                <button 
                :class="{selected:(filtroSelecionado==1) ? true : false}"
                @click="filtroNoticia(1)">USA</button>
                <!-- Moçambique -->
                <button 
                :class="{selected:(filtroSelecionado==2) ? true : false}"
                @click="filtroNoticia(2)">Moçambique</button>
                <!-- Reaload -->
                <button 
                class="reaload"
                @click="reload()"><i class='bx bx-refresh'></i></button>
            </div>
        </div>
        <!-- Carregamento -->
        <div class="loading" v-if="carregando" >
        </div>
        <!-- Erro de carregamento -->
        <div v-if="carregando_erro" class="error">
            Erro ao buscar as noticias
        </div>
        <!-- Noticias -->
        <div class="container">
            <!-- noticia -->
            <Noticia v-for="noticia in noticias" 
            :key        = "noticia.id"
            :id         = "noticia.id"
            :autor      = "noticia.autor"
            :titulo     = "noticia.titulo"
            :descricao  = "noticia.descricao"
            :publicado  = "noticia.publicado"
            :src_imagem = "noticia.src_imagem"
            :link       = "noticia.link"
            />
        </div>
    </section>

</template>

<style scoped>

    .error{
        font-size: 18px;
        color: gray;
    }
    .container-filtro{
        display: flex;
        flex-direction: column;
        width: 100%;
        max-width: 800px;
        box-shadow: rgba(0, 0, 0, 0.4) 0px 0px 3px;
        padding: 10px;
        border-radius: 5px;
    }
    .container-filtro .option{
        display: flex;
        gap: 10px;
    }
    .container-filtro button{
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 10px 15px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }
    .reaload{
        padding: 0px;
        margin-left: auto;
        background-color: transparent;
        font-size: 24px;
    }
    .container-filtro button.selected{
        background-color: rgb(9, 99, 255);
        color: #fff;
    }
    section>p{
        font-size: 22px;         
    }
    section, .container{
        display: flex;
        align-items: center;
        flex-direction: column;
        gap: 10px;
        padding:20px;
    }
    .container{
        padding: 0px;
    }
    .loading{
        margin-top: 20px;
        height: 50px;
        width: 50px;
        border-radius: 50%;
        background-color: transparent;
        border: 6px solid rgb(0, 111, 245);
        border-right: 6px solid transparent;
        animation: anim_loading 1s ease-in-out infinite;
    }
    @keyframes anim_loading{
        0%{
            transform: rotate(0deg);
        }
        100%{
            transform: rotate(360deg);
        }
    }
</style>
