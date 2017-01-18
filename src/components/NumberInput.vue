<template>
    <input
        autocomplete="off"
        class="number-input"
        pattern="\d*"
        :placeholder="filteredPlaceholder"
        ref="el"
        type="text"
        v-model="number"
        @keydown.up="increment"
        @keydown.down="decrement"
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
                if (!number) return '';
                let string = number.toString().replace(/[^0-9]/g, '');
                return this.limit > 0 ? string.substring(0, this.limit) : string;
            },
            focus() {
                this.$refs.el.focus();
                return this;
            },
            select() {
                this.$refs.el.select();
                return this;
            },
            increment() {
                let number = parseInt(this.number);

                if(!number) return this.number = this.step;

                this.number = number + this.step;
            },
            decrement() {
                this.number = parseInt(this.number) - this.step;
            }
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
            step: {
                type: Number,
                default: 1,
            }
        },
        created() {
            this.number = this.filter(this.value)
        }
    }
</script>
