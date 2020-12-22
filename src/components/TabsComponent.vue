<template>
    <div class="form__item col col-xs-12 form__item--tabs">
        <div class="tabs">
            <div class="tabs__item">
                <input
                        id="male" class="visibility-hidden"
                        type="radio" name="gender" value="male"
                        v-model="gender"
                >
                <label class="tabs__label tabs__label--left" for="male">Мужчина</label>
            </div>
            <div class="tabs__item">
                <input
                        id="female" class="visibility-hidden"
                        type="radio" name="gender" value="female"
                        v-model="gender"
                >
                <label class="tabs__label tabs__label--right" for="female">Женщина</label>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TabsComponent',
    data() {
        return {
            gender: 'male',
        }
    },
}
</script>

<style scoped>

</style>