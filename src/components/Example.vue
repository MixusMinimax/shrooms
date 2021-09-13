<template>
  <h2>Subtitle.</h2>
  <p>My name is {{ firstName }} {{ lastName }}.</p>
</template>

<script>
export default {
  name: 'Example',
  data() {
    return {
      firstName: 'Maxi',
      lastName: 'Barmetler',
    }
  },
}
</script>