<template>
  <div class="about">
    <div style="margin-bottom: 20px">
      <div class="container" style="padding: 10px 20px;">
        <!-- <span class="breadcrumb-item active">About</span> -->
      </div>
    </div>
    <div class="centre" style="max-width: 1264px;width: 100%;">
      <h4>About Library System</h4>
      <br />Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla venenatis arcu eu nulla sollicitudin,
      nec porttitor nulla egestas. Morbi vel velit efficitur, faucibus dolor in, dapibus sem. Integer ut
      ipsum sit amet nulla condimentum maximus. Vestibulum commodo lectus eget lectus porta elementum.
      Sed pellentesque efficitur orci, at facilisis sapien pellentesque non. Nullam blandit,
      velit vitae pulvinar consequat, turpis nisi blandit ligula, id convallis neque quam in est.
      Donec condimentum ullamcorper lorem ac laoreet. Donec neque quam, ultrices nec maximus at,
      sagittis sagittis lorem. Morbi feugiat viverra porta. Phasellus congue pellentesque libero,
      a venenatis odio tristique ac. Vivamus vel dapibus orci. Morbi dui mi, faucibus ut ipsum sed,
      pretium molestie eros. Suspendisse tincidunt massa ut mattis sollicitudin. Nunc at purus at velit
      malesuada imperdiet quis at nulla. Mauris euismod vulputate quam, non fringilla urna vehicula ut.
      Sed interdum, tellus sed feugiat varius, felis tortor congue risus, efficitur interdum tellus massa ac mi.
    </div>
  </div>
</template>

<script>
export default {
  name: "about",
  created() {
    this.$store.commit("startLoading");
    this.$store.commit("changePage", [
      { text: "About", disabled: false, to: "/about" }
    ]);
  },
  mounted() {
    this.$store.commit("stopLoading");
  }
};
</script>

<style scoped>
div.centre {
  text-align: justify;
  width: 50%;
  margin: auto;
  margin-bottom: 20px;
}
</style>

