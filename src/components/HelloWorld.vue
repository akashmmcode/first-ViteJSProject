<script setup>
import { ref,onMounted } from 'vue';


const newTask = ref("");

const movieListArr = ref([]);

async function getdata() {
  const url = "https://hoblist.com/api/movieList";
  const params = {
    category: "movies",
    language: "kannada",
    genre: "all",
    sort: "voting"
  };

  try {
    const response = await fetch(url, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(params)
    });

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const data = await response.json(); 

    movieListArr.value = data.result;

    console.log(movieListArr.value); // Check the updated movieListArr

  } catch (error) {
    console.error("Failed to fetch data:", error);
  }
}

onMounted(() => {
  getdata();
});

</script>

<template>
  <div class="card" v-for="item in movieListArr" :key="task">
  <img :src="item.poster" alt="Movie Poster">
  <div class="card-details">
    <h2>Title: {{item.title}}</h2>
    <h4>Genere: {{item.genre}}</h4>
    <h4>Language: {{item.language}}</h4>
    <h4>Director: {{item.director[0]}}</h4>
  </div>
  </div>
</template>

<style scoped>
h1 {
  color: blue;
}

.card{
  display:flex;
  flex-direction:row;
  margin-bottom:10px;
  background-color: gray;
  width: 60vw;
  border-radius:20px;
  justify-content: space-around;
  color:black

}

.card-details{
  text-align:start;
  color:black

}

.card img{
  width:20rem;
  height:20rem;
}

h2,h4{
  color:"black"
}
</style>
