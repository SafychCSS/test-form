<template>
    <div class="form__item select col col-4 col-xs-12">
        <span class="select__heading">Образование</span>
        <div class="form__select select__field" @click="showDropdown">
            <span class="select__placeholder" v-if="!dropdownChoose.length">Выберите образование</span>
            <div>
                <span v-for="item in dropdownChoose" class="select__choose" @click.stop>
                    {{ item.title }}
                    <button type="button" class="reset select__reset" @click="removeVariant(item.id)">&times;</button>
                </span>
            </div>
        </div>
        <transition name="bounce">
            <ul class="form__dropdown dropdown" v-if="isOpen">
                <li class="dropdown__item"
                    v-for="item in dropdown"
                    :key="item.id"
                    :class="{ 'dropdown__item--active': item.isActive }"
                    @click="addVariant(item)"
                >
                    {{ item.title }}
                </li>
            </ul>
        </transition>
        <select name="edu[]" multiple style="display:none;">
            <option
                    :value="item.title"
                    v-for="item in dropdown"
                    :key="item.id"
                    :selected="item.selected"
            >
                {{item.title}}
            </option>
        </select>
    </div>
</template>

<script>
export default {
    name: 'SelectComponent',
    props: {
        dropdownItem: {
            type: Array,
            required: true
        }
    },
    data() {
        return {
            dropdown: [
                {
                    id: 1,
                    title: 'Среднее',
                    isActive: false,
                    selected: false
                },
                {
                    id: 2,
                    title: 'Среднее специальное',
                    isActive: false,
                    selected: false
                },
                {
                    id: 3,
                    title: 'Незаконченное высшее',
                    isActive: false,
                    selected: false
                },
                {
                    id: 4,
                    title: 'Высшее',
                    isActive: false,
                    selected: false
                },
                {
                    id: 5,
                    title: 'Магистр, кандидат, доктор наук',
                    isActive: false,
                    selected: false
                }
            ],
            dropdownChoose: this.dropdownItem,
            isOpen: false,
        }
    },
    methods: {
        showDropdown() {
            this.isOpen = !this.isOpen;
        },
        addVariant(obj) {

            let isEqual = false;

            this.dropdownChoose.forEach(item => {
                if (item.id === obj.id) {
                    isEqual = true;
                    this.removeVariant(obj.id);
                }
            });

            if (!isEqual) {
                this.dropdownChoose.push({
                    id: obj.id,
                    title: obj.title,
                    isActive: obj.isActive,
                    selected: true
                });

                this.dropdown.forEach(item => {
                    if (item.id === obj.id) {
                        item.selected = true;
                        item.isActive = true;
                    }
                });

                this.$emit('emitDropdown', this.dropdownChoose.length > 0);
            }
        },
        removeVariant(id) {
            const index = this.dropdownChoose.findIndex(item => item.id === id);
            this.dropdownChoose.splice(index, 1);

            this.dropdown.forEach(item => {
                if (item.id === id) {
                    item.selected = false;
                    item.isActive = false;
                }
            });

            this.$emit('emitDropdown', this.dropdownChoose.length > 0);
        },
    },

    watch: {
        dropdownItem() {
            if (this.dropdownItem.length === 0)
                this.dropdownChoose = [];
        }
    },

    created() {
        const onClickOutside = e => this.isOpen = this.$el.contains(e.target) && this.isOpen;
        document.addEventListener('click', onClickOutside);
        this.$on('hook:beforeDestroy', () => document.removeEventListener('click', onClickOutside));
    },
}
</script>

<style scoped>
    .bounce-enter-active {
        animation: bounce-in .3s;
    }
    .bounce-leave-active {
        animation: bounce-in .3s reverse;
    }
    @keyframes bounce-in {
        0% {
            transform: scale(0);
        }
        100% {
            transform: scale(1);
        }
    }
</style>