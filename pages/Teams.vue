<template>
    <div class="w-full mt-8 gap-8">
        <div class="flex justify-center">
            <h1 class="font-bold text-3xl text-white">Titulares:</h1>
        </div>
        <div class="flex justify-center flex-wrap gap-3 w-full mt-8">
            <div class="flex items-center flex-wrap gap-5">
                <div v-for="player in holders" class="flex rounded-3xl items-center gap-3 p-6 w-96 cursor-pointer bg-red-500 transition duration-300 ease-in-out hover:scale-105 hover:drop-shadow-2xl">
                    <div class="avatar">
                        <div class="w-24 rounded-full">
                            <img :src="player!.img" />
                        </div>
                    </div>
                    <div class="font-bold text-3xl text-white">
                        {{ player.name }}
                        
                    </div>
                </div>
            </div>
        </div>

        <div class="flex justify-center mt-8">
            <h1 class="font-bold text-3xl text-white">Reservas:</h1>
        </div>
        <div class="flex justify-center flex-wrap gap-3 w-full mt-8">
            <div class="flex items-center flex-wrap gap-5">
                <div v-for="player in reserves" class="flex rounded-3xl items-center gap-3 p-6 w-96 cursor-pointer bg-red-500 transition duration-300 ease-in-out hover:scale-105 hover:drop-shadow-2xl">
                    <div class="avatar">
                        <div class="w-24 rounded-full">
                            <img :src="player!.img" />
                        </div>
                    </div>
                    <div class="font-bold text-3xl text-white">
                        {{ player.name }}
                        
                    </div>
                </div>
            </div>
        </div>

        <div class="mt-14">
            <div class="divider divider-error w-fit lg:divider-vertical">
                <div class="avatar">
                    <div class="w-24 rounded-full">
                        <img src="https://cdn.awsli.com.br/800x800/2607/2607018/produto/220651557/nba-logo-jogador-0c24f157.png" />
                    </div>
                </div>
            </div> 
        </div>

        <div class="w-full mt-14">
            <div class="flex items-center flex-wrap gap-5">
                <div v-for="player in players" class="flex rounded-3xl items-center gap-3 p-6 w-96 cursor-pointer bg-red-300 transition duration-300 ease-in-out hover:scale-105 hover:drop-shadow-2xl hover:bg-red-500">
                    <div class="avatar">
                        <div class="w-24 rounded-full">
                            <img :src="player.img" />
                        </div>
                    </div>
                    <div class="font-bold text-3xl text-white">
                        {{ player.name }}
                        <div v-show="!player.used" class="flex gap-3 mt-8">
                            <div @click="addTeam(true, player)" class="bg-red-300 font-bold text-xl rounded text-white px-3 transition duration-300 ease-in-out hover:scale-105 hover:drop-shadow-2xl hover:text-red-300 hover:bg-white">
                                Titulares
                            </div>
                            <div @click="addTeam(false, player)" class="bg-red-300 font-bold text-xl rounded text-white px-3 transition duration-300 ease-in-out hover:scale-105 hover:drop-shadow-2xl hover:text-red-300 hover:bg-white">
                                Reservas
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
    import data from '../db/db.json'

    const players = data.jogadores
    const holders = data.holders
    const reserves = data.reserves

    function addTeam(isHolder: boolean,item: any) {
        if(isHolder) {
            if(holders.length < 5) {
                let existHolders = holders.filter((x:any) => x.id == item.id)
                let existReserves = reserves.filter((x:any) => x.id == item.id)

                if(existHolders.length === 0 && existReserves.length === 0) {
                    const dataJson = JSON.stringify(item)

                    const req = fetch("http://localhost:8000/holders", {
                        method: "POST",
                        headers: {"Content-Type": "application/json"},
                        body: dataJson
                    })

                    getHolders()
                }  else {
                    alert('Jogador já está sendo utilizado!')
                }
            } else {
                alert('Você já possui o maximo de jogadores titulares!')
            }
        } else {
            if(reserves.length < 7) {
                let existHolders = holders.filter((x:any) => x.id == item.id)
                let existReserves = reserves.filter((x:any) => x.id == item.id)

                if(existHolders.length === 0 && existReserves.length === 0) {
                    const dataJson = JSON.stringify(item)

                    const req = fetch("http://localhost:8000/reserves", {
                        method: "POST",
                        headers: {"Content-Type": "application/json"},
                        body: dataJson
                    })

                    getReserves()
                }
            }  else {
                alert('Você já possui o maximo de jogadores reservas!')
            }
        }
    }

    function getHolders() {
        const req = fetch("http://localhost:8000/holders", {
            method: "GET",
            headers: {"Content-Type": "application/json"}
        })
    }

    function getReserves() {
        const req = fetch("http://localhost:8000/reserves", {
            method: "GET",
            headers: {"Content-Type": "application/json"}
        })
    }

    function deletePlayer(isHolder: Boolean) {
        const stringQuery = isHolder ? "http://localhost:8000/holders" : "http://localhost:8000/reserves"

        const req = fetch(stringQuery, {
            method: "DELETE",
            headers: {"Content-Type": "application/json"}
        })

    }
</script>