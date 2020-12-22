<template>
    <div id="app" class="wrapper">
        <div class="container">
            <form class="form" @submit.prevent="send" ref="form">
                <div class="row row--v-bottom">
                    <input-name @emitName="emitName" :name="name" :error="error" />

                    <select-component @emitDropdown="emitDropdown" :dropdown-item="dropdownChoose" />

                    <toggle-component @emitMove="emitMove" :move="move" />
                </div>

                <div class="row row--v-center">
                    <tabs-component />

                    <checkbox-component
                            class="form__item--time"
                            name="fullTime"
                            :model="fullTime"
                            @isChecked="isChecked"
                    >
                        Полный рабочий день
                    </checkbox-component>

                    <checkbox-component
                            class="form__item--remote"
                            name="remote"
                            :model="remote"
                            @isChecked="isChecked"
                    >
                        Удаленная работа
                    </checkbox-component>
                </div>


                <div class="row row--last">
                    <div class="form__item col col-12 form__item--btn btn-group">
                        <button type="button" class="btn btn-group__mobile" :disabled="isDisabled" @click="clear">Отменить</button>
                        <button class="btn btn--save btn--offset-left btn-group__mobile"
                                :disabled="isDisabled"
                                @click.prevent="formValidate"
                        >
                            Сохранить
                        </button>
                    </div>
                </div>

                <transition name="fade">
                    <alert-success v-if="success" />
                </transition>

            </form>
        </div>
    </div>
</template>

<script>

import InputName from "@/components/InputName";
import SelectComponent from "@/components/SelectComponent";
import ToggleComponent from "@/components/ToggleComponent";
import TabsComponent from "@/components/TabsComponent";
import CheckboxComponent from "@/components/CheckboxComponent";
import AlertSuccess from "@/components/AlertSuccess";

export default {
    name: 'App',
    components: {
        InputName,
        SelectComponent,
        ToggleComponent,
        TabsComponent,
        CheckboxComponent,
        AlertSuccess
    },
    data() {
        return {
            name: '',
            move: false,
            dropdownChoose: [],
            fullTime: false,
            remote: false,
            error: false,
            success: false,
            validate: {
                name: false,
                fullTime: false,
                remote: false,
                move: false,
                education: false
            }
        }
    },
    methods: {
        formValidate() {
            if (!this.name) {
                this.error = true;
            } else {
                this.success = true;
                setTimeout(() => {
                    this.success = false;
                }, 3000)
                this.send();
            }
        },

        // test
        send() {
            let formData = new FormData(this.$refs.form);
            fetch('asd.php', {
                method: 'POST',
                body: formData
            }).then(res => {
                console.log(res)
            }).catch(e => {
                console.log(e)
            })
        },

        clear() {
            this.name = '';
            this.move = false;
            this.dropdownChoose = [];
            this.fullTime = false;
            this.remote = false;

            Object.keys(this.validate).forEach(item => {
                this.validate[item] = false;
            })
        },

        emitDropdown(val) {
            this.validate.education = val;
        },

        emitMove(val) {
            this.move = val;
            this.validate.move = val;
        },

        emitName(val) {
            this.name = val;
            this.validate.name = val.length > 0;
        },

        isChecked(obj) {
            if (obj.name === 'fullTime') {
                this.fullTime = obj.check;
                this.validate.fullTime = obj.check;
            }

            if (obj.name === 'remote') {
                this.remote = obj.check;
                this.validate.remote = obj.check;
            }
        }
    },

    computed: {
        isDisabled() {
            return !Object.values(this.validate).some(item => item);
        },
    },

    watch: {
        name() {
            if (this.name)
                this.error = false;
        }
    },
}
</script>

<style lang="scss">
    .fade-enter-active {
        animation: fadeInFlip .5s;
    }
    .fade-leave-active {
        animation: fadeInFlip .5s reverse;
    }
    @keyframes fadeInFlip {
        0% {
            opacity: .4;
            transform: perspective(400px) rotateX(90deg);
        }
        100% {
            opacity: 1;
            transform: perspective(400px) rotateX(0);
        }
    }
</style>
