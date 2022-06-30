<template>
  <div>
    <h1 class="heading-text">Top 20 most viewed categories on Twitch</h1>       
        <div class="top-categories">
          <div class="top-category" v-for="topCategory in listOfTopCategories" :key="topCategory.gameID">
            <a :href="'https://www.twitch.tv/directory/game/' + topCategory.gameName" target="_blank">
              <div>
                <img class="Top-category-thumbnail" :src="`${topCategory.gameArtURL}`" alt="">
              </div>
              <div class="Top-category-description">
                {{topCategory.gameName}}
              </div>
            </a>
          </div>
      </div>
  </div>
</template>

<script>
  import { RouterLink, RouterView } from 'vue-router'
  
  export default {
    name: 'GetTopCategories',
    data: function () {
      return {
          listOfTopCategories: []
      } 
    },
    methods: {
      // function to connect and get data from Twitch Api connected to top games
      fetchTopCategories: function () {
        let fetchLink = 'https://api.twitch.tv/helix/games/top?first=20';

        fetch(fetchLink, {
            method: 'get',
            headers: new Headers({
                'Authorization': `Bearer y70xdhaxvx2p9cc0gmjqesscvxj4eh`,
                'Client-ID': `gp762nuuoqcoxypju8c569th9wz7q5`
            })
        })
        .then(
            function (response) {
                return response.json();
            }
        )
        .then(
            data => {

                let dataListOfTopCategories = [];

                for (var key in data.data) {
                  dataListOfTopCategories.push({
                    gameID: data.data[key].id,
                    gameName: data.data[key].name,
                    gameArtURL: data.data[key].box_art_url.replace('{width}', '213').replace('{height}', '285'),
                  });
                }
              this.listOfTopCategories = [...this.listOfTopCategories, ...dataListOfTopCategories]
            }
        );
      }
    },
    mounted () {
        this.fetchTopCategories();
    }
  }
</script>

<style>
/* Styling for category cards */
  .heading-text {
    font-size: 1.5rem;  
    padding: 30px;
    color: white;
    text-align: center;
  }

  @media screen and (min-width: 920px) {
    .heading-text {
      text-align: start; 
    }
  }

  .top-categories {
    display: flex;
    flex-direction: row;
    justify-content: center;
    flex-wrap: wrap;
  }

  .top-category {
    padding: 10px 10px 10px 10px;
  }

  .Top-category-description {
    text-align: center;
    color: white;
    font-size: 0.8rem;
    background-color: black;
    margin-top: -10px;
    padding: 5px;
  }
</style>
