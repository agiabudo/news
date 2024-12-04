<script setup>
    import { onMounted, ref } from 'vue';
    import Noticia from './Noticia.vue';
    
    const noticias = ref([]);

    const carregando = ref(false);
    const carregando_erro = ref(false);

    const filtroSelecionado = ref(0);

   async function filtroNoticia(filtroid){
        filtroSelecionado.value = filtroid;
        const category = (filtroid==0) ? 'technology' : 'business';
        const endpoint = `https://newsapi.org/v2/top-headlines?country=us&category=${category}&apiKey=1e5f5688e7104daa9f0e5000c51866bc`;
       
        try{
            noticias.value = [];
            carregando.value = true;
            carregando_erro.value = false;
            
            const response = await fetch(endpoint);
            const data = await response.json();
            console.log(data)
            data.articles.forEach((art, index) => {
                noticias.value.push({
                    id:         index,
                    autor:      art.author,
                    titulo:     art.title,
                    descricao:  art.description,
                    publicado:  art.publishedAt,
                    src_imagem: art.urlToImage
                })                
            });
        } catch(error) {
            console.error("error na requisicao:"+error);
            carregando_erro.value = true;
        } finally {
            carregando.value = false;
            console.warn('noticias carregadas com sucesso');
        }
    }

    onMounted(() =>{
        filtroNoticia(filtroSelecionado.value);
    })

</script>

<template>

    <section>
        <p>Noticias</p>
        <div class="container-filtro">
            <p>Filtros</p>
            <div class="option">
                <button 
                :class="{selected:(filtroSelecionado==0) ? true : false}"
                @click="filtroNoticia(0)">Teclologia</button>
                
                <button 
                :class="{selected:(filtroSelecionado==1) ? true : false}"
                @click="filtroNoticia(1)">Negocio</button>
            </div>
        </div>
        <!-- carregamento -->
        <div class="loading" v-if="carregando" >
        </div>
        <!-- erro de carregamento -->
        <div v-if="carregando_erro" class="error">
            Erro ao buscar as noticias
        </div>
        <!-- noticias -->
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
        padding: 10px 15px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }
    .container-filtro button.selected{
        background-color: aquamarine;
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
