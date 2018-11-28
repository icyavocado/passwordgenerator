<template>
  <div class="hello"></div>
</template>

<script>
export default {
  name: "PasswordGenerator",
  data() {
    return {
          ascii_lowercase = 'abcdefghijklmnopqrstuvwxyz',
    ascii_uppercase = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ',
    digits = '0123456789',
    hexdigits = '0123456789abcdefABCDEF',
    octdigits = '01234567',
      regexTest: /^(?=.*\d)(?=.*[a-z])(?=.*[A-Z])[0-9a-zA-Z!@#$%^&*?]{6,}$/,

    };
  }
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
</style>
