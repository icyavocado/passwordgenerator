<template>
  <div class="generate">
    <div class="container">
      <label for="passwords">Passwords</label>
      <select name="passwords" v-model="selectedPassword">
        <!-- eslint-disable -->
        <option v-for="password in passwords" :key="password" :value="password">{{ password }}</option>
      </select>
      <button @click="regenerate()">Generate</button>
      <button @click="copy()">Copy</button>
      
      <label for="password-length">Password Length:</label>
      <select name="password-length" id="password-length" v-model="selectedLength">
        <option
          v-for="number in passwordLength"
          :value="number"
          :key="'value-' + number"
        >{{ number }}</option>
      </select>
      {{ selectedLength }}
    </div>

    <div>
      Options:
      Allow Character
      <div v-for="type in allowCharArray" :key="'type-' + type">
        <input type="checkbox" :name="type" :value="type" v-model="selectedChar">
        {{ type }}
      </div>

      <textarea ref="password" id="password" class="hideTextArea" v-model="selectedPassword"></textarea>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PasswordGenerator',
  data() {
    return {
      selectedPassword: '',
      selectedLength: 6,
      numberofPassGenerate: 3,
      selectedChar: ['a-z', 'A-Z'],
      recompute: false,
      allowChar: {
        'a-z': 'abcdefghijklmnopqrstuvwxyz',
        'A-Z': 'ABCDEFGHIJKLMNOPQRSTUVWXYZ',
        '0-9': '0123456789',
        '"#$%&': "#$%&'()*+,-./:;<=>?@[\\]^_`{|}~",
      },
      regexTest: /^(?=.*\d)(?=.*[a-z])(?=.*[A-Z])[0-9a-zA-Z!@#$%^&*?]{6,}$/,
    };
  },
  methods: {
    regenerate() {
      this.recompute = !this.recompute;
      this.selectedPassword = this.passwords[0];
    },
    copy() {
      this.$refs.password.select();
      document.execCommand('Copy');
    },
    shuffle(array) {
      let m = array.length;
      let t;
      let i;
      const arr = array;

      // While there remain elements to shuffle…
      while (m) {
        // Pick a remaining element…
        i = Math.floor(Math.random() * (m -= 1));

        // And swap it with the current element.
        t = arr[m];
        arr[m] = arr[i];
        arr[i] = t;
      }

      return arr;
    },
  },
  computed: {
    allowCharArray: {
      get() {
        return Object.keys(this.allowChar);
      },
    },
    passwordLength: {
      get() {
        const array = [...Array(101).keys()];
        array.shift();
        return array;
      },
    },
    pool: {
      get() {
        let string = '';
        // eslint-disable-next-line
        this.allowCharArray.forEach(el => {
          if (this.selectedChar.includes(el)) {
            string += this.allowChar[el];
          }
        });
        return string;
      },
    },
    passwords: {
      get() {
        if (this.recompute) {
          // Do nothing
        }
        const passwords = [];
        for (let j = 0; j < this.numberofPassGenerate; j += 1) {
          let password = '';
          const string = this.shuffle(this.pool.split('')).join('');
          for (let i = 0; i < this.selectedLength; i += 1) {
            password += string.charAt(
              // eslint-disable-next-line
              Math.floor(Math.random() * string.length)
            );
          }
          passwords.push(password);
        }
        return passwords;
      },
    },
  },
  watch: {
    pool: {
      handler() {
        this.selectedPassword = this.passwords[0];
      },
    },
    selectedLength: {
      handler() {
        this.selectedPassword = this.passwords[0];
      },
    },
  },
  created() {
    this.selectedPassword = this.passwords[0];
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.hideTextArea {
  position: absolute;
  width: 5px;
  height: 5px;
  left: -10px;
}
</style>
