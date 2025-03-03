<script lang="ts">
import axios from 'axios'

type Player = {
    // 課題割り当てフラグ
    isAssigned: boolean
    name: string
    startPoint: string
    startPointUrl: string
    goalPoint: string
    goalPointUrl: string
    defaultHint1: string
    defaultHint2: string
    defaultHint3: string
}
type Admin = {
    isAssigned: boolean
    name: string
}

type GameManageViewData = {
    fetchedPlayers: string[]
    fetchedAdmins: string[]
    name: string
    schedule: string
    meetingPoint: string
    players: Player[]
    admins: Admin[]
    query: Query | unknown
}

type Course = {
    name: string
    player: string
    startPoint: {
        name: string
        url: string
    }
    goalPoint: {
        name: string
        url: string
    }

}

type Query = {
    name: string
    date: string
    count: string
    players:
    {
        // id: number
        name: string
    }[],
    admins:
    {
        // id: number
        name: string
    }[],
    courses: Course[],
}

export default {
    data(): GameManageViewData {
        return {
            // 仮の値 (API から取得する情報を想定)
            fetchedPlayers: ["testplayer1", "testplayer2"],
            fetchedAdmins: ["testadmin1", "testadmin2"],

            // フォーム入力値
            name: '',
            schedule: '',
            meetingPoint: '',
            players: [],
            admins: [],
            // API送信クエリ
            query: {}
        }
    },
    methods: {
        handleRegisterGame() {
            console.log("register")
            const courses: Course[] = []
            this.players.forEach(player => {
                if (player.isAssigned == true) {
                    const course = {
                        name: player.name + "の課題",
                        player: player.name,
                        startPoint: {
                            name: player.startPoint,
                            url: player.startPointUrl
                        },
                        goalPoint: {
                            name: player.goalPoint,
                            url: player.goalPointUrl
                        },
                        hints: [
                            {
                                name: "デフォルトヒント1",
                                content: player.defaultHint1
                            },
                            {
                                name: "デフォルトヒント2",
                                content: player.defaultHint2
                            },
                            {
                                name: "デフォルトヒント3",
                                content: player.defaultHint3
                            }
                        ]
                    }
                    courses.push(course)
                }

            });
            this.query = {
                name: this.name,
                date: this.schedule,
                players: [],
                admins: this.admins,
                courses: courses
            }
            // ここでAPIへPOSTする
            console.log(this.query)
            const baseurl = import.meta.env.VITE_API_URL
            const url = baseurl + "/games"
            axios.post(url, this.query)
                .then((res) => {
                    console.log(res)
                })
                .then((err) => {
                    console.log(err)
                })
        }
    },
    mounted() {
        // API から取得したプレイヤーに対し、課題情報を一時的に紐づけておく
        this.fetchedPlayers.forEach(player => {
            const newPlayer = {
                isAssigned: false,
                name: player,
                startPoint: "",
                startPointUrl: "",
                goalPoint: "",
                goalPointUrl: "",
                defaultHint1: "",
                defaultHint2: "",
                defaultHint3: ""
            }
            this.players.push(newPlayer)
        });
        this.fetchedAdmins.forEach(admin => {
            const newAdmin = {
                isAssigned: false,
                name: admin
            }
            this.admins.push(newAdmin)
        })
        console.log(this.players)
    },
}
</script>

<template>

    <div class="container">
        <h3>ゲーム登録</h3>
        <p>ゲームマスターとして担当するゲームを新規登録します。</p>
        <form @submit.prevent="handleRegisterGame()">
            <h4>ゲーム基本情報</h4>
            <div class="mb-3">
                <label class="form-label">ゲーム名</label>
                <input v-model="name" type="text" class="form-control">
                <div class="form-text">例: 第〇回リアルGeoguessor </div>
            </div>
            <div class="mb-3">
                <label class="form-label">開催日</label>
                <input v-model="schedule" type="date" class="form-control">
            </div>
            <div class="mb-3">
                <label class="form-label">集合場所・時刻</label>
                <input v-model="meetingPoint" type="text" class="form-control">
                <div class="form-text">例: 〇〇駅 午前9時集合 </div>
            </div>
            <div class="mb-3">
                <label class="form-label">参加プレイヤー</label>
                <div class="form-check" v-for="(player, index) in players" v-bind:key="index">
                    <input v-model="player.isAssigned" class="form-check-input" type="checkbox">
                    <label class="form-check-label">
                        {{ player.name }}
                    </label>
                </div>
            </div>
            <div class="mb-3">
                <label class="form-label">自分以外のゲームマスター</label>
                <div class="form-check" v-for="(admin, index) in admins" v-bind:key="index">
                    <input v-model="admin.isAssigned" class="form-check-input" type="checkbox">
                    <label class="form-check-label">
                        {{ admin.name }}
                    </label>
                </div>
            </div>

            <div v-for="(player, index) in players" v-bind:key="index">
                <template v-if="player.isAssigned === true">
                    <h4>ゲーム課題登録</h4>
                    <p>各プレイヤーに割り当てるゲーム課題を入力します。</p>
                    <h5>{{ player.name }} の課題</h5>

                    <!-- <div class="mb-3">
                    <label class="form-label">課題名</label>
                    <input type="text" class="form-control">
                    <div class="form-text">課題を識別するための名前です。 </div>
                </div> -->
                    <div class="mb-3">
                        <label class="form-label">スタート地点名</label>
                        <input v-model="player.startPoint" type="text" class="form-control">
                    </div>
                    <div class="mb-3">
                        <label class="form-label">スタート地点URL</label>
                        <input type="text" v-model="player.startPointUrl" class="form-control">
                        <div class="form-text">マップアプリで参照できるスタート地点のリンクです。 </div>
                    </div>
                    <div class="mb-3">
                        <label class="form-label">ゴール地点名</label>
                        <input type="text" v-model="player.goalPoint" class="form-control">
                    </div>
                    <div class="mb-3">
                        <label class="form-label">ゴール地点URL</label>
                        <input type="text" v-model="player.goalPointUrl" class="form-control">
                        <div class="form-text">マップアプリで参照できるゴール地点のリンクです。 </div>
                    </div>
                    <div class="mb-3">
                        <label class="form-label">デフォルトヒント1</label>
                        <input type="text" v-model="player.defaultHint1" class="form-control">
                    </div>
                    <div class="mb-3">
                        <label class="form-label">デフォルトヒント2</label>
                        <input type="text" v-model="player.defaultHint2" class="form-control">
                    </div>
                    <div class="mb-3">
                        <label class="form-label">デフォルトヒント3</label>
                        <input type="text" v-model="player.defaultHint3" class="form-control">
                    </div>
                </template>


            </div>


            <div class="mb-3">
                <button type="submit" class="btn btn-primary">ゲーム登録</button>
            </div>

        </form>
    </div>
</template>
