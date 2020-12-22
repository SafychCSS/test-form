<template>
    <div class="form__item col col-4 col-xs-12">
        <span class="form__field-heading form__field--required">ФИО</span>
        <div class="input-block">
            <input
                    v-model="inputName"
                    @input="emitName"
                    class="input-block__text"
                    :class="{'input-block__text--error': error}"
                    type="text"
                    name="name"
                    placeholder="Введите ФИО"
            >
            <button type="button" class="input-block__reset reset" v-if="inputName" @click="clearName">&times;</button>
        </div>

        <transition name="fade">
            <div class="error" v-if="error">
                <span class="error__text">Необходимо ввести ФИО</span>
            </div>
        </transition>

    </div>
</template>

<script>

export default {
    name: 'InputName',
    props: ['name', 'error'],
    data() {
        return {
            inputName: this.name
        }
    },
    methods: {
        clearName() {
            this.inputName = '';
            this.$emit('emitName', this.inputName);
        },
        emitName() {
            this.$emit('emitName', this.inputName);
        }
    },
    computed: {
    },
    watch: {
        name() {
            if (this.name.length === 0) {
                this.inputName = '';
                //this.$emit('emitName', this.inputName);
            }
        }
    }
}
</script>

<style scoped>
    .fade-enter-active {
        animation: fadeIn .3s;
    }
    .fade-leave-active {
        animation: fadeIn .3s reverse;
    }
    @keyframes fadeIn {
        0% {
            opacity: 0;
        }
        100% {
            opacity: 1;
        }
    }
</style>