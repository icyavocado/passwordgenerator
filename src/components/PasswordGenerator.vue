<template>
  <div class="generate">
    <div class="container">
      <b-form inline class="mb-2">
        <b-input-group prepend="Password" class="col-12">
          <b-form-select :options="passwords" required v-model="selectedPassword"></b-form-select>
          <b-input-group-append>
            <b-button variant="danger" @click="copy()">Copy</b-button>
            <b-button variant="danger" @click="regenerate()">Generate</b-button>
          </b-input-group-append>
        </b-input-group>
      </b-form>

      <b-form inline class="mb-2">
        <b-input-group prepend="Password Length" class="col-12">
          <b-form-select :options="passwordLength" required v-model="selectedLength"></b-form-select>
        </b-input-group>
      </b-form>

      <b-form inline class="mb-2">
        <b-input-group prepend="Options" class="col-12">
          <b-input-group-prepend is-text>
            <b-form-checkbox-group v-model="selectedChar" name="options">
              <b-form-checkbox
                v-for="type in allowCharArray"
                :key="'type-' + type"
                :value="type"
                class="mr-sm-2"
              >{{ type }}</b-form-checkbox>
            </b-form-checkbox-group>
          </b-input-group-prepend>
        </b-input-group>
      </b-form>

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
        '"#$%&': '#$%&()*+,-.:;=?@[]^_{}~',
      },
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
          let string = this.shuffle(this.pool.split('')).join('');
          string += this.shuffle(this.pool.split('')).join('');
          string += this.shuffle(this.pool.split('')).join('');
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
