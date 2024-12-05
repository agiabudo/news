<script setup>

    import ScrollReveal from 'scrollreveal';
    import { onMounted } from 'vue';
    
    // propredades do componente (pai: SectionNoticias)
    const props = defineProps({
        id:         Number,
        autor:      String,
        titulo:     String,
        descricao:  String,
        publicado:  String,
        src_imagem: String,
        link:       String
    })

    onMounted(() =>{
        ScrollReveal().reveal('.noticia',{
            duration:  1500,
            distance: '50px'
        })
    })

</script>

<template>
    <div class="noticia">
        <img :src="props.src_imagem" class="noticia-imagem">
        <div class="noticia-informacoes">
            <p class="noticia-autor">{{props.autor}}</p>
            <p class="noticia-titulo">{{props.titulo}}</p>
            <a :href="props.link" target="_blank" >Ver esta noticia</a>
            <p class="noticia-publicada">Publicada: {{props.publicado}}</p>
        </div>
    </div>
</template>

<style scoped>
    .noticia{
        width: 800px;
        visibility: hidden;
        box-shadow: rgba(0, 0, 0, 0.4) 0px 0px 10px;
    }
    .noticia-imagem{
        height: 400px;
        width: 100%;
        object-fit: cover;
    }
    .noticia-informacoes{
        padding: 10px;
    }
    .noticia-titulo{
        font-weight: 600;
        font-size: 18px;
    }
    .noticia-publicada{
        margin-top: 10px;
        color: gray;
    
    }
    a{
        text-decoration: none;
        font-size: 18px;
        color: rgb(107, 0, 221);
        font-weight: 600;
    }
    /* telas menores que 400px */
    @media(max-width: 800px){
        .noticia{
            width: 500px;
        }
        .noticia-imagem{
            height: 250px;
        }
    }
    /* telas menores que 400px */
    @media(max-width: 500px){
        .noticia{
            width: 350px;
        }
        .noticia-imagem{
            height: 175px;
        }
    }
</style>