<template>
    <div class="app-game">
        <game-operating class='operating'></game-operating>
    </div>
</template>

<script>
    import GameOperating from '../../components/game/GameOperating'

    export default {
        name: "index",
        components: {
            GameOperating
        }
    }
</script>

<style lang='scss' scoped>
.app-game{
    display: flex;
    justify-content: center;
    align-items: center;
    .operating{
        width: 100px;
        height: 100px;
    }
}
</style>
