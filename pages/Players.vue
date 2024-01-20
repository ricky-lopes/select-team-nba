<template>
    <div class="flex w-full mt-8 gap-8">
        <div class="w-full">
            <div class="flex justify-center w-full">
                <h1 class="font-bold text-3xl text-white">Cadastre seu Jogador:</h1>
            </div>
            <UForm :validate="validate" :state="state" class="space-y-4 mt-8" @submit="onSubmit">

                <div class="w-full flex flex-col justify-center gap-8">
                    <input v-model="state.name" type="text" placeholder="Nome do Jogador" class="input input-bordered input-primary w-full" />

                    <input v-model="state.img" type="text" placeholder="URL imagem" class="input input-bordered input-primary w-full" />
                </div>

                <div class="flex justify-center w-full">
                    <UButton color="red" class="transition duration-300 ease-in-out hover:scale-105 hover:drop-shadow-2xl" type="submit">
                        Cadastrar
                    </UButton>
                </div>
                
            </UForm>

            <div class="flex mt-8 gap-3">
                <div v-if="state.img && state.name" class="rounded-3xl flex items-center gap-3 p-6 w-96 cursor-pointer font-bold text-3xl text-white bg-red-500 transition duration-300 ease-in-out hover:scale-105 hover:drop-shadow-2xl">
                    <div class="avatar">
                        <div class="w-24 rounded-full">
                            <img :src="state.img" />
                        </div>
                    </div>
                    {{ state.name }}
                </div>
            </div>
                
        </div>

        <div class="divider divider-error w-fit lg:divider-horizontal">
            <div class="avatar">
                <div class="w-24 rounded-full">
                    <img src="https://cdn.awsli.com.br/800x800/2607/2607018/produto/220651557/nba-logo-jogador-0c24f157.png" />
                </div>
            </div>
        </div> 

        <div class="w-full">
            <div class="flex items-center flex-wrap gap-5">
                <div v-for="player in players" class="flex rounded-3xl items-center gap-3 p-6 w-96 cursor-pointer font-bold text-3xl text-white bg-red-300 transition duration-300 ease-in-out hover:scale-105 hover:drop-shadow-2xl hover:bg-red-500">
                    <div class="avatar">
                        <div class="w-24 rounded-full">
                            <img :src="player.img" />
                        </div>
                    </div>
                    {{ player.name }}
                </div>
            </div>
        </div>
    </div>
    
    
</template>

<script setup lang="ts">
    import type { FormError, FormSubmitEvent } from '#ui/types'
    import { reactive } from 'vue';
    import data from '../db/db.json'

    const players = data.jogadores

    const state = reactive({
    name: undefined,
    position: undefined,
    img: undefined,
    used: false
    })

    const validate = (state: any): FormError[] => {
    const errors = []
    if (!state.name) errors.push({ path: 'name', message: 'Required' })
    if (!state.img) errors.push({ path: 'img', message: 'Required' })
    return errors
    }

    async function onSubmit (event: FormSubmitEvent<any>) {
        const dataJson = JSON.stringify(state)

        const req = await fetch("http://localhost:8000/jogadores", {
            method: "POST",
            headers: {"Content-Type": "application/json"},
            body: dataJson
        })

        state.name = undefined
        state.img = undefined
    }

</script>