<template>
  <div class="translator">
      <input @input="handleCode" type="text" v-model="code">
      <p ref="text">{{result}}</p>
  </div>
</template>

<script>
export default {
    name: 'CodeTranslator',
    data() {
        return {
            code: '',
            result: ''
        }
    },
    methods: {
        handleCode() {
            this.result = '';
            this.code.replace(/ /g,'');
            const code = this.code.replace(/ /g,'');
            const letters = [
                {
                    letter: 'a',
                    code: '00000'
                },
                {
                    letter: 'b',
                    code: '00001'
                },
                {
                    letter: 'c',
                    code: '00010'
                },
                {
                    letter: 'd',
                    code: '00011'
                },
                {
                    letter: 'e',
                    code: '00100'
                },
                {
                    letter: 'f',
                    code: '00101'
                },
                {
                    letter: 'g',
                    code: '00110',
                },
                {
                    letter: 'h',
                    code: '00111',
                },
                {
                    letter: 'i',
                    code: '01000'
                },
                {
                    letter: 'j',
                    code: '01001'
                },
                {
                    letter: 'k',
                    code: '01010'
                },
                {
                    letter: 'l',
                    code: '01011'
                },
                {
                    letter: 'm',
                    code: '01100'
                },
                {
                    letter: 'n',
                    code: '01101'
                },
                {
                    letter: 'o',
                    code: '01110'
                },
                {
                    letter: 'p',
                    code: '01111'
                },
                {
                    letter: 'q',
                    code: '10000'
                },
                {
                    letter: 'r',
                    code: '10001'
                },
                {
                    letter: 's',
                    code: '10010'
                },
                {
                    letter: 't',
                    code: '10011'
                },
                {
                    letter: 'u',
                    code: '10100'
                },
                {
                    letter: 'v',
                    code: '10101'
                },
                {
                    letter: 'w',
                    code: '10110'
                },
                {
                    letter: 'x',
                    code: '10111'
                },
                {
                    letter: 'y',
                    code: '11000'
                },
                {
                    letter: 'z',
                    code: '11001'
                }
            ];
            const results = code.match(/.{1,5}/g);
            results.forEach(item => {
                const foundLetter = letters.find(letter => letter.code === item);
                this.result += foundLetter.letter;
            })
            this.copyText(this.result);
        },
        copyText(text) {
            const elem = document.createElement('textarea');
            elem.value = text;
            document.body.appendChild(elem);
            elem.select();
            document.execCommand('copy');
            document.body.removeChild(elem);
        }
    }
}
</script>

<style lang="scss" scoped>
.translator {
    input {
        width: 900px;
        height: 400px;
        font-size: 30px;
    }
    p {
        font-size: 32px;
        text-align: center;
    }
}
</style>