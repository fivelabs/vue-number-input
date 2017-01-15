<template>
    <input
        autocomplete="off"
        class="number-input"
        pattern="\d*"
        :placeholder="filteredPlaceholder"
        ref="el"
        type="text"
        v-model="number"
    >
</template>
<script>
    export default {
        data() {
            return {
                number: '',
            }
        },
        methods: {
            filter (number) {
                if (!number) return ''
                let string = number.toString().replace(/[^0-9]/g, '')
                return this.limit > 0 ? string.substring(0, this.limit) : string
            },
            focus() {
                this.$refs.el.focus()
            },
            select() {
                this.$refs.el.select()
            },
        },
        computed: {
            filteredPlaceholder() {
                return this.filter(this.placeholder)
            }
        },
        watch: {
            number (number) {
                this.$emit('input', this.number = this.filter(number))
                if (!number) this.$emit('empty')
            },
            value (value) {
                this.number = this.filter(value)
            },
        },
        props: {
            limit: {
                type: Number,
                default: 0,
            },
            placeholder: {
                type: [Number, String],
                default: '',
            },
            value: [Number, String],
        },
        created() {
            this.number = this.filter(this.value)
        }
    }
</script>
