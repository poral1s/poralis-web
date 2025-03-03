<script lang="ts">
import axios from "axios"

type GameViewData = {
  games: {
    name: string
    date: string
    players: {
      name: string
    }[]
    admins: {
      name: string
    }[]
    courses: {
      id: number
      name: string
      player: string
      startpoint: {
        name: string
        url: string
      }
      goalpoint: {
        name: string
        url: string
      }
      hints: {
        id: number
        content: string
      }[]
    }[]
  }[]
}


export default {
  data(): GameViewData {
    return { games: [] }
  },
  created() {
    const baseurl = import.meta.env.VITE_API_URL
    const url = baseurl + "/games"
    axios.get(url)
      .then((res) => {
        console.log(res)
        this.games = res.data
      })
      .catch((err) => {
        console.log("Error: ", err)
      })
  }

}
</script>

<template>

  <div class="container">
    <div class="mb-3">
      <h3 class="mb-3">ゲーム一覧</h3>
      <p>プレイヤーまたはゲームマスターとして参加したゲームの一覧を表示します。</p>
    </div>
    <p></p>
    <div class="card mb-4" v-for="(game, index) in games" v-bind:key="index">

      <h5 class="card-header">{{ game.name }}</h5>
      <table class="table table-bordered">
        <tbody>
          <tr>
            <th scope="row">開催日</th>
            <td>{{ game.date }}</td>
          </tr>
          <tr>
            <th scope="row">プレイヤー</th>
            <td><span v-for="(player, index) in game.players" v-bind:key="index">{{ player.name }}, </span></td>
          </tr>
          <tr>
            <th scope="row">ゲームマスター</th>
            <td><span v-for="(admin, index) in game.admins" v-bind:key="index">{{ admin.name }}, </span></td>
          </tr>
          <tr v-for="(course, index) in game.courses" v-bind:key="index">
            <th scope="row">ゲーム課題 {{ course.id }}</th>
            <td><b>課題名: </b>{{ course.name }} <br>
              <b>割当プレイヤー: </b>{{ course.player }} <br>
              <b>スタート地点: </b>{{ course.startpoint.name }} <br>
              <b>スタート地点URL: </b>{{ course.startpoint.url }} <br>
              <b>ゴール地点: </b>{{ course.goalpoint.name }} <br>
              <b>ゴール地点URL: </b>{{ course.goalpoint.url }} <br>
              <span v-for="(hint, index) in course.hints" v-bind:key="index">
                <b>ヒント {{ hint.id }}: </b> {{ hint.content }} <br>
              </span>
            </td>
          </tr>
        </tbody>
      </table>

    </div>
  </div>
</template>
