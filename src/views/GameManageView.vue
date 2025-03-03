<script lang="ts">

type Player = {
    name: string
    startPoint: string
    startPointUrl: string
    goalPoint: string
    goalPointUrl: string
    defaultHint1: string
    defaultHint2: string
    defaultHint3: string
}
type GameManageViewData = {
    allPlayers: string[]
    allAdmins: string[]
    name: string
    schedule: string
    meetingPoint: string
    selectedPlayers: Player[]
    selectedAdmins: string[]
    query: Query | unknown
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
    courses:
    {
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
    }[],
}

export default {
    data(): GameManageViewData {
        return {
            // 仮の値 (API から取得する情報を想定)
            allPlayers: ["testplayer1", "testplayer2"],
            allAdmins: ["testadmin1", "testadmin2"],

            // フォーム入力値
            name: '',
            schedule: '',
            meetingPoint: '',
            selectedPlayers: [],
            selectedAdmins: [],
            // API送信クエリ
            query: {}
        }
    },
    methods: {
        handleRegisterGame() {
            console.log("register")
            console.log()
            const courses = []
            this.selectedPlayers.forEach(player => {
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
            });
            // this.query = {
            //     name: this.name,
            //     date: this.date,
            //     players: [],
            //     admins: this.selectedAdmins,
            //     courses: courses
            // }
            // ここでAPIへPOSTする
            console.log(this.query)
        }
    },
    mounted() {
        // API から取得したプレイヤーに対し、課題情報を一時的に紐づけておく
        this.allPlayers.forEach(player => {
            const newPlayer = {
                name: player,
                startPoint: "",
                startPointUrl: "",
                goalPoint: "",
                goalPointUrl: "",
                defaultHint1: "",
                defaultHint2: "",
                defaultHint3: ""
            }
            this.selectedPlayers.push(newPlayer)
        });
        console.log(this.selectedPlayers)
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
                <div class="form-check" v-for="(player, index) in allPlayers" v-bind:key="index">
                    <input v-model="selectedPlayers[index]" class="form-check-input" type="checkbox" :value="player">
                    <label class="form-check-label">
                        {{ player }}
                    </label>
                </div>
            </div>
            <div class="mb-3">
                <label class="form-label">自分以外のゲームマスター</label>
                <div class="form-check" v-for="(admin, index) in allAdmins" v-bind:key="index">
                    <input v-model="selectedAdmins" class="form-check-input" type="checkbox" :value="admin">
                    <label class="form-check-label">
                        {{ admin }}
                    </label>
                </div>
            </div>
            <div v-if="selectedPlayers.length > 1">
                <h4>ゲーム課題登録</h4>
                <p>各プレイヤーに割り当てるゲーム課題を入力します。</p>

                <div v-for="(player, index) in selectedPlayers" v-bind:key="index">
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
                </div>

            </div>

            <div class="mb-3">
                <button type="submit" class="btn btn-primary">ゲーム登録</button>
            </div>

        </form>
    </div>
</template>
