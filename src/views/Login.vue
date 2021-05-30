<template>
  <div id="login">
    <h1 class="title">
      Hello. <br />
      Welcome Back
    </h1>

    <form action="/" class="login-form">
      <label for="username">USERNAME</label>
      <input type="text" placeholder="USERNAME" />

      <label for="password">PASSWORD</label>
      <input type="password" placeholder="PASSWORD" />

      <input id="submit" type="submit" value="LOGIN" />
    </form>
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
h1 {
  font-size: 3em;
  margin-top: 2em;
}

label {
  color: var(--grey);
}

form {
  margin-top: 4em;
  display: flex;
  flex-direction: column;
}

input {
  font-style: inherit;
  margin-bottom: 4em;
  background-color: transparent;
  background-image: none;
  border: none;
  border-bottom: 1px solid var(--grey);
  padding: 0.5em 0;
  color: white;
  font-size: inherit;
  -webkit-appearance: none;
  outline: none;
  box-shadow: none;
}

#submit {
  background-color: var(--blue);
  border-radius: 0.5em;
  border: none;
  padding: 1em 0;
}

#submit:hover {
  cursor: pointer;
}
</style>
