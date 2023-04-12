<template>
  <div class="container">
    
    <nav aria-label="breadcrumb">
      <ol class="breadcrumb">
        <li class="breadcrumb-item"><RouterLink to="/">About</RouterLink></li>
        <li class="breadcrumb-item active" aria-current="page">About Us</li>
      </ol>
    </nav>
    
    <div class="row mt-5 m">
      <p>This is about page content</p>
    </div>

  </div>
</template>


<script>
  export default {
    name: 'About',
    data() {
      return {

      }
    },
  }

</script>
