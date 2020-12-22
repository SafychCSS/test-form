<template>
    <div class="form__item col col-xs-12">
        <div class="checkbox">
            <input :id="name" class="visibility-hidden" type="checkbox" @change="isChecked" :name="name" v-model="checked">
            <label class="checkbox__label" :for="name">
                <span class="checkbox__text"><slot></slot></span>
            </label>
        </div>
    </div>
</template>

<script>
export default {
    name: 'CheckboxComponent',
    props: ['name', 'model', 'title'],
    data() {
        return {
            checked: this.model
        }
    },
    methods: {
        isChecked() {
            this.$emit('isChecked', {
                check: this.checked,
                name: this.name
            });
        }
    },

    watch: {
        model() {
            if (!this.model)
                this.checked = false;
        }
    }
}
</script>

<style scoped>

</style>