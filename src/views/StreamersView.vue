<template>
  <div>
    <h2 class="streamer-text">Use the search box to see data of your favourite streamer!</h2>
    <div class="columns is-centered" id="search-box">
      <input type="text" class="column is-half" id="search-bar" placeholder="Search for streamer.." v-model="userInput">
      <button class="search-button" @click="fetchStreamer(); fetchEmbedded();">Search</button>
    </div>
    <div class="display-streamer">
      <div class="video-container">
        <iframe class="video-box" :src="`https://player.twitch.tv/?channel=${username}&parent=localhost`" allowfullscreen></iframe>
      </div>
      <div v-for="showStreamerData in listOfStreamerData" :key="showStreamerData.streamerID">
        <div class="streamer-container">
          <table>
            <tr class="title-box">
              <th>Streamer</th>
              <th>Status</th>
              <th>Title</th>
              <th>Game</th>
              <th>Language</th>
            </tr>
            <tr class="data-box">
              <td>
                <img class="streamer-logo" :src="`${showStreamerData.thumbnailURL}`" alt=""> <br>
                {{showStreamerData.streamerName}}
              </td>
              <td class="online-box" v-if="showStreamerData.streamerIsLive === true">Online</td>
              <td class="offline-box" v-else>Offline</td>
              <td>{{showStreamerData.streamerTitle}}</td>
              <td>{{showStreamerData.streamerIsPlaying}}</td>
              <td>{{showStreamerData.streamerLanguage}}</td>
            </tr>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { RouterLink, RouterView } from 'vue-router'

  export default {
    name: 'GetStreamer',
    data: function () {
      return {
          listOfStreamerData: [],
          userInput: '',
          username: ''
      } 
    },
    methods: {
      
      fetchStreamer: function () {
        
        let fetchLink = `https://api.twitch.tv/helix/search/channels?first=1&query=${this.userInput}`;

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

                let dataListOfStreamerData = [];

                for (var key in data.data) {
                  dataListOfStreamerData.push({
                    streamerID: data.data[key].id,
                    streamerName: data.data[key].display_name,
                    streamerIsLive: data.data[key].is_live,
                    streamerTitle: data.data[key].title,
                    streamerIsPlaying: data.data[key].game_name,
                    streamerLanguage: data.data[key].broadcaster_language,
                    thumbnailURL: data.data[key].thumbnail_url
                  });
                }

              this.listOfStreamerData = [];
              this.listOfStreamerData = [...this.listOfStreamerData, ...dataListOfStreamerData]
            }
        );
      },
      
      fetchEmbedded: function () {
        this.username = this.userInput 
      }
    },
    mounted () {
      this.fetchStreamer();
      this.fetchEmbedded();
    } 
  }
</script>

<style>
/* Styling for searchbox and button */
  .streamer-text {
    text-align: center;
    margin-bottom: 30px;
    font-size: 1.5rem;
  }

  #search-box {
    margin-bottom: 30px;
  }

  #search-box #search-bar {

    font-size: 20px;
    appearance: none;
    border: none;
    outline: none;
  }

  .search-button {
    color: white;
    background-color: hsla(160, 100%, 37%, 1);
    border: none;
    text-decoration: none;
    padding: 0px 15px 0px 15px;
    font-size: 18px;
    font-weight: 400;
  }

  .search-button:hover {
    background-color: rgba(84, 84, 84, 0.48);
    transition: 0.5s;
    cursor: pointer;
  }

/* Styling for displaying streamer data */
  .display-streamer {
    display: flex;
    flex-direction: column;
    justify-content: center;
    flex-wrap: wrap;
  }
  .streamer-container {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-items: center;
    background-color: rgb(24, 24, 24);
    margin-top: 50px;
  }

  .streamer-logo {
    width: 100px;
    border-radius: 50%;
  }

  .video-container {
    display: flex;
    flex-direction: row;
    justify-content: center;
  }

  .video-box {
    height: 608px;
    width: 1216px;
    margin: auto;
    border: 3px solid white;
  }

  table, tr {
    width: 100%;
    height: 100%;
    text-align: center;
  }

  tr th {
    color: white;
    font-size: 18px;
    padding: 10px;
    border: 3px solid white;
  }

  tr td {
    color: white;
    font-size: 16px;
    padding: 10px;
    text-align: center;
    vertical-align: middle;
    border-right: 3px solid white;
    border-left: 3px solid white;
    border-bottom: 3px solid white;
  }

  .online-box {
    background-color: green;
  }

  .offline-box {
    background-color: red;
  }
</style>
