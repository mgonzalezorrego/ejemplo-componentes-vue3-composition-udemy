<script setup>
  import { onMounted, ref } from "vue";

  import ButtonCounter from './components/ButtonCounter.vue' 
  import BlogPost from './components/BlogPost.vue'
  import PaginatePost from './components/PaginatePost.vue'
  import LoadingSpinner from './components/LoadingSpinner.vue'

  const posts = ref([]);
  const postXpage = 10
  const inicio = ref(0)
  const fin = ref(postXpage)
  const loading = ref(true);

  const favorito = ref("");
  const cambiarFavorito = (title) => {
    favorito.value = title;
  };

  const siguiente = () => {
    inicio.value = inicio.value + postXpage
    fin.value = fin.value + postXpage
  }

  const anterior = () => {
    inicio.value = inicio.value - postXpage
    fin.value = fin.value - postXpage
  }

//onMounted(() => {
//  fetchData();
//})

//  
//       .then((res) => res.json())
//        .then((data) => {
//          posts.value = data;
//        })
//        .finally(() => {
//          setTimeout(() => {
//            loading.value = false;
//          }, 2000);
//          
//        })

const fetchData = async() => {
  try {
    const res = await fetch('https://jsonplaceholder.typicode.com/posts');
    posts.value = await res.json();
  } catch (error) {
      console.log(error);    
  }finally {
      setTimeout(() => {
      loading.value = false;
  }, 1000);
    
  }
}
fetchData();
</script>

<template>
  <LoadingSpinner  v-if="loading"/>
  <div class="container" v-else> 
    <h1>APP</h1>
    <h2>Mi Post Favorito: {{ favorito }}</h2>
    <PaginatePost @next="siguiente" @prev="anterior" :inicio="inicio" :fin="fin" :ultimo="posts.length" class="mb-2"/>
    <!--<ButtonCounter />-->
    <BlogPost
      v-for="post in posts.slice(inicio,fin)" 
      :key="post.id"
      :title="post.title" 
      :id="post.id" 
      :body="post.body" 
      colorText="primary"
      @cambiarFavoritoNombre="cambiarFavorito"
    ></BlogPost>
  </div>
</template>