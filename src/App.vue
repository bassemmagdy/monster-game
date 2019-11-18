<template>
    <div id="app">
        <Header />
        <section class="row">
            <div class="small-6 columns">
                <h1 class="text-center">YOU</h1>
                <div class="healthbar">
                    <div
                        class="healthbar text-center"
                        style="background-color: green; margin 0; color:white"
                        :style="{width: myHealth + '%'}"
                    >{{myHealth}}</div>
                </div>
            </div>
            <div class="small-6 columns">
                <h1 class="text-center">MONSTER</h1>
                <div class="healthbar">
                    <div
                        class="healthbar text-center"
                        style="background-color: green; margin 0; color:white"
                        :style="{width: opHealth + '%'}"
                    >{{opHealth}}</div>
                </div>
            </div>
        </section>
        <section class="row controls" v-if="!start">
            <div class="small-12 columns">
                <button id="start-game" @click="startEndGame()">START NEW GAME</button>
            </div>
        </section>
        <section class="row controls">
            <div class="small-12 columns">
                <button id="attack" @click="attack()" :disabled="start == false">ATTACK</button>
                <button
                    id="special-attack"
                    @click="specialAttack()"
                    :disabled="start == false"
                >SPECIAL ATTACK</button>
                <button id="heal" @click="heal()" :disabled="start == false">HEAL</button>
                <button id="give-up" @click="startEndGame()" :disabled="start == false">GIVE UP</button>
            </div>
        </section>
        <section class="row log">
            <div class="small-12 columns">
                <ul>
                    <li v-for="log in logs" :key="log">{{log}}</li>
                </ul>
            </div>
        </section>
    </div>
</template>

<script>
import Header from './components/Header.vue';
import './assets/foundation.min.css';
export default {
    name: 'app',
    components: {
        Header
    },
    data: function() {
        return {
            myHealth: 100,
            opHealth: 100,
            start: false,
            logs: ['Just Started']
        };
    },
    methods: {
        startEndGame() {
            this.start = !this.start;
            this.myHealth = 100;
            this.opHealth = 100;
            this.logs = ['Just Started'];
        },
        attack() {
            let mhealth = Math.floor(Math.random() * 10 + 1);
            let ohealth = Math.floor(Math.random() * 10 + 1);
            this.myHealth -= mhealth;
            this.opHealth -= ohealth;
            this.logs.push('opponent hit you ' + mhealth);
            this.logs.push('you hit opponent with ' + ohealth);
        },
        heal() {
            let mhealth = 10;
            let ohealth = Math.floor(Math.random() * 10 + 1);
            this.myHealth += mhealth;
            this.myHealth -= ohealth;
            this.logs.push('You healead by ' + 10);
            this.logs.push('Opponent hit you with ' + ohealth);
        },
        specialAttack() {
            let mhealth = Math.floor(Math.random() * 10 + 1);
            let ohealth = 10;
            this.myHealth -= mhealth;
            this.opHealth -= ohealth;
            this.logs.push('You hit opponent by ' + 10);
            this.logs.push('Opponent hit you with ' + ohealth);
        }
    },
    watch: {
        opHealth() {
            if (this.opHealth <= 0) {
                this.opHealth = 0;
                let winner = 'You';
                alert(winner + ' are the winner ');
            }
        },
        myHealth() {
            if (this.myHealth <= 0) {
                this.myHealth <= 0;
                let winner = '';
                winner = 'Monster';
                alert(winner + ' is the winner ');
            }
        }
    }
};
</script>

<style>
.text-center {
    text-align: center;
}

.healthbar {
    width: 80%;
    height: 40px;
    background-color: #eee;
    margin: auto;
    transition: width 500ms;
}

.controls,
.log {
    margin-top: 30px;
    text-align: center;
    padding: 10px;
    border: 1px solid #ccc;
    box-shadow: 0px 3px 6px #ccc;
}

.turn {
    margin-top: 20px;
    margin-bottom: 20px;
    font-weight: bold;
    font-size: 22px;
}

.log ul {
    list-style: none;
    font-weight: bold;
    text-transform: uppercase;
}

.log ul li {
    margin: 5px;
}

.log ul .player-turn {
    color: blue;
    background-color: #e4e8ff;
}

.log ul .monster-turn {
    color: red;
    background-color: #ffc0c1;
}

button {
    font-size: 20px;
    background-color: #eee;
    padding: 12px;
    box-shadow: 0 1px 1px black;
    margin: 10px;
}

#start-game {
    background-color: #aaffb0;
}

#start-game:hover {
    background-color: #76ff7e;
}

#attack {
    background-color: #ff7367;
}

#attack:hover {
    background-color: #ff3f43;
}

#special-attack {
    background-color: #ffaf4f;
}

#special-attack:hover {
    background-color: #ff9a2b;
}

#heal {
    background-color: #aaffb0;
}

#heal:hover {
    background-color: #76ff7e;
}

#give-up {
    background-color: #ffffff;
}

#give-up:hover {
    background-color: #c7c7c7;
}
</style>
