<template>
    <div class="form__item form__item--move col col-4 col-xs-12">
        <div class="toggle">
            <input type="checkbox" name="move" class="visibility-hidden" id="move" v-model="moveCheckbox" @change="emitMove">
            <label for="move" class="toggle__label">Готов к переезду</label>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ToggleComponent',
    props: {
        move: {
            type: Boolean,
            required: true
        }
    },
    data() {
        return {
            moveCheckbox: this.move,
        }
    },
    methods: {
        emitMove() {
            this.$emit('emitMove', this.moveCheckbox);
        }
    },
    watch: {
        move() {
            if (!this.move)
                this.moveCheckbox = false;
        }
    }
}
</script>

<style scoped>

</style>