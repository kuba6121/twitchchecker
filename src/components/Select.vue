<template>
    <div class="custom__select" :tabindex="tabindex" @blur="open = false">
        <div class="selected" :class="{ open: open }" @click="open = !open">
            {{selected}}
        </div>
        <div class="options" :class="{ selectHide: !open }">
            <div class="option" v-for="option in options" :key="option.value" @click="selected = option.name; open = false; $emit('input', option.value);">
                {{option.name}}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'CustomSelect',
    props: {
        options: {
            type: Array,
            required: true
        },
        default: {
            type: String,
            required: false,
            default: null
        },
        tabindex: {
            type: Number,
            required: false,
            default: 0
        }
    },
    data() {
        return {
            selected: this.options.length > 0 ? this.options[0].name : null,
            open: false
        }
    },
    mounted() {
        this.$emit('input', this.selected);
    }
}
</script>

<style lang="scss" scoped>
.custom__select {
    position: relative;
    text-align: left;
    outline: none;
    height: 30px;
    line-height: 30px;
    .selected {
        background-color: #09090B;
        border-radius: 6px;
        border: 1px solid #7f5af0;
        color: #fffffe;
        padding-left: 8px;
        cursor: pointer;
        user-select: none;
        &.open {
            border-radius: 6px 6px 0px 0px;
            &::after {
                transform: rotate(180deg) translateY(5px);
            }
        }
        &::after {
            position: absolute;
            content: '';
            top: 15px;
            right: 10px;
            width: 0;
            height: 0;
            border: 4px solid transparent;
            border-color: #fff transparent transparent transparent;
        }
    }
    .options {
        color: #fffffe;
        border-radius: 0px 0px 6px 6px;
        border-right: 1px solid #7f5af0;
        border-left: 1px solid #7f5af0;
        border-bottom: 1px solid #7f5af0;
        overflow: hidden;
        position: absolute;
        background-color: #09090B;
        left: 0;
        right: 0;
        .option {
            padding-left: 8px;
            cursor: pointer;
            user-select: none;
        }
    }
    .selectHide {
        display: none;
    }
}
</style>