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
        @blur="blur"
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
                if (!number && number != 0) return '';
                let string = number.toString().replace(/[^0-9]/g, '');
                return this.filterMax(this.limit >= 0 ? string.substring(0, this.limit) : string);
            },
            filterMin(number) {
                if (number < this.min) {
                    return this.min;
                }
                return number;
            },
            filterMax(number) {
                if (number > this.max) {
                    return this.max;
                }
                return number;
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

                if(!number) return this.number = this.filterMax(this.step);

                this.number = this.filterMax(number + this.step);
            },
            decrement() {
                this.number = this.filterMin(parseInt(this.number) - this.step);
            },
            blur() {
                this.number = this.filterMin(this.number);
            },
        },
        computed: {
            filteredPlaceholder() {
                return this.filter(this.placeholder)
            }
        },
        watch: {
            number (number) {
                this.$emit('input', this.number = this.filter(number));
                if (!number) this.$emit('empty');
                if (this.limit && number.length == this.limit) this.$emit('full');
            },
            value (value) {
                this.number = this.filter(value);
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
            },
            min: {
                type: Number,
                default: 0,
            },
            max: {
                type: Number,
                default: 0,
            }
        },
        created() {
            this.number = this.filter(this.value);
        }
    }
</script>
