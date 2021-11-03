<template>
    <div id="app" class="container text-center">
        <img class="text-center" src="@/assets/logo-pokemon.png" />
        <h1 class="my-4">PokeGuía</h1>
        <p>Puedes buscar el pokemon, por su <b>nombre</b> o <b>numero</b></p>
        <form v-on:submit.prevent="bPokemon" class="mt-4">
            <div class="row">
                <div class="col">
                    <label for="name" class="sr-only">Name:</label>
                    <input type="text" readonly class="form-control-plaintext text-right mb-2" id="name" value="Name:"/>
                </div>
                <div class="col">
                    <label for="new-todo" class="sr-only">nombre de pokemon</label>
                    <input v-model="wPoke" @keyup.enter="bPokemon" type="text" class="form-control mb-2" id="new-todo" placeholder="nombre de pokemon"/>
                </div>
                <button type="submit" @click="bPokemon" class="col btn btn-primary mb-2">
                    Buscar
                </button>
            </div>
        </form>
        <img v-bind:src="imgPokemon" width="200px" alt="Pokemon"/>
        <div>
            <h2 class="mb-4">Movimientos</h2>
            <div v-for="(movimiento, index) in movimientos" v-bind:key="index">
                {{ movimiento.move.name }}
            </div>
        </div>
        <div class="mb-5">
            <h2 class="my-4">Habilidades</h2>
            <div v-for="(habilidad, index) in habilidades" v-bind:key="index">
                {{ habilidad.ability.name }}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "App",
    data: () => ({
        pokemon: {
            movimientos: [],
            habilidades: [],
        },
        wPoke: "pikachu",
    }),
    created() {
        this.bPokemon();
    },
    methods: {
        async bPokemon() {
            try {
                const data = await fetch(
                    `https://pokeapi.co/api/v2/pokemon/${this.wPoke}`
                );
                const objeto = await data.json();
                this.pokemon = objeto;
            } catch (e) {
                console.log(e);
            }
        },
    },
    computed: {
        imgPokemon() {
            return this.pokemon.sprites?.front_default ?? "";
        },
        habilidades() {
            return this.pokemon.abilities;
        },
        movimientos() {
            return this.pokemon.moves;
        },
    },
};
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 80px;
}
</style>
