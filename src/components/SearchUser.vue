<template>
<div :class="{emited: emited}" class="search">
    <div :class="{error: error.length > 2}" class="input__wrapper">
        <input @keyup.enter="fetchData" v-model="username" type="text" name="username" id="username" required>
        <label for="username">Username</label>
        <span v-show="error.length > 0" class="error">{{error}}</span>
    </div>
    <button @click="fetchData">Check follows</button>
    <transition name="fade">
        <div v-show="loading" class="lds-ring"><div></div><div></div><div></div><div></div></div>
    </transition>
</div>
    
</template>

<script>
export default {
    name: 'SearchUser',
    data() {
        return {
            username: '',
            error: '',
            emited: false
        }
    },
    props: {
        loading: {
            type: Boolean,
            default: false
        }
    },
    methods: {
        fetchData() {
            if(this.username.length < 2) {
                this.error = 'Username is too short';
                return;
            } else {
                this.error = '';
            }
            this.$emit('fetchFollows', this.username);
            this.emited = true;
        }
    }
}
</script>

<style lang="scss" scoped>
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
.search {
    width: 250px;
    margin: 0 auto;
    transition: all .3s ease-in;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-family: 'Poppins', sans-serif;
    .input__wrapper {
        position: relative;
        display: flex;
        flex-direction: column;
        width: 100%;
        label {
            position: absolute;
            left: 0px;
            top: 9px;
            font-size: 16px;
            pointer-events: none;
            transition: all .2s ease;
            padding: 2px 10px;
            color: #fffffe;
        }
        input {
            background: none;
            padding: 10px;
            z-index: 1;
            outline: none;
            color: #fffffe;
            border: 1px solid #7f5af0;
            font-size: 16px;
            width: 100%;
            box-sizing: border-box;
            &:focus ~ label, &:not(:focus):valid ~ label {
                top: -22px;
                left: 0px;
                background-color: #7f5af0;
                font-size: 12px;
            }
            &:focus {
                border-width: 2px;
            }
        }
        span.error {
            color:#f25042;
            text-align: left;
            padding: 3px 10px;
            font-size: 14px;
        }
    }
    .input__wrapper.error {
        input {
            border-color: #f25042;
            &:focus ~ label, &:not(:focus):valid ~ label {
                background-color: #f25042;
            }
        }
    }
    button {
        width: 100%;
        background-color: #7f5af0;
        border: none;
        padding: 8px 10px;
        margin-top: 5px;
        outline: none;
        transition: all .2s ease;
        font-size: 14px;
        box-sizing: border-box;
        &:hover {
            background-color: #A88FF5;
        }
        &:focus {
            background-color: #6F44EE;
        }
    }
}
.emited {
    top: 50px;
    transform: translate(-50%, 0%);
}
.lds-ring {
  display: block;
  position: relative;
  width: 80px;
  height: 80px;
  margin-left: auto;
  margin-right: auto;
  margin-top: 20px;
}
.lds-ring div {
  box-sizing: border-box;
  display: block;
  position: absolute;
  width: 64px;
  height: 64px;
  margin: 8px;
  border: 8px solid #7f5af0;
  border-radius: 50%;
  animation: lds-ring 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
  border-color: #7f5af0 transparent transparent transparent;
}
.lds-ring div:nth-child(1) {
  animation-delay: -0.45s;
}
.lds-ring div:nth-child(2) {
  animation-delay: -0.3s;
}
.lds-ring div:nth-child(3) {
  animation-delay: -0.15s;
}
@keyframes lds-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>