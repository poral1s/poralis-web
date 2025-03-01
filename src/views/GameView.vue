<script lang="ts">
import axios from "axios"
export default {
    data() {
        return {
            games: []
        }
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
        <div class="card mb-4" v-for="game in games">

            <h5 class="card-header">{{ game.name }}</h5>
            <table class="table table-bordered">
                <tbody>
                    <tr>
                        <th scope="row">開催日</th>
                        <td>{{ game.date }}</td>
                    </tr>
                    <tr>
                        <th scope="row">プレイヤー</th>
                        <td><span v-for="player in game.players">{{ player.name }}, </span></td>
                    </tr>
                    <tr>
                        <th scope="row">ゲームマスター</th>
                        <td><span v-for="admin in game.admins">{{ admin.name }}, </span></td>
                    </tr>
                    <tr v-for="course in game.courses">
                        <th scope="row">ゲーム課題 {{ course.id }}</th>
                        <td><b>課題名: </b>{{ course.name }} <br>
                            <b>割当プレイヤー: </b>{{ course.player }} <br>
                            <!-- バックエンド側で startpoint の型を整備していないのでとりあえずこう書く -->
                            <b>スタート地点: </b>{{ course.startpoint[0].Value }} <br>
                            <b>スタート地点URL: </b>{{ course.startpoint[1].Value }} <br>
                            <b>ゴール地点: </b>{{ course.goalpoint[0].Value }} <br>
                            <b>ゴール地点URL: </b>{{ course.goalpoint[1].Value }} <br>
                            <span v-for="hint in course.hints">
                                <b>ヒント {{ hint.id }}: </b> {{ hint.content }} <br>
                            </span>
                        </td>
                    </tr>
                </tbody>
            </table>

        </div>
    </div>
</template>
