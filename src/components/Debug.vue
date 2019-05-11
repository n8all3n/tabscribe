<template>
    <div>
    <!-- debug -->

    </div>
</template>
<script>
export default {
    /* eslint-disable */
  name: 'HelloWorld',
   props: [
    "item"
  ],
  created: function() {
    console.log(this.item);
  }
}
</script>