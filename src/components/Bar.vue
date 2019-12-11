<template>

    <div className="array-container"
        class="subheading mx-3 array-bar"
        target="_blank"
        :style="{
            backgroundColor: PRIMARY_COLOR,
            height: `${val}px`,
            width: `${barWidth}px`,
            margin: `1px !important`
        }"
    >
        {{val}}
    </div>

</template>
<script>
    export default {
        name: 'Bar',
        props:{
            val: Number,
            barWidth: Number,
        },
        data: () => ({
            PRIMARY_COLOR: 'grey',
        })
    }

</script>

<style scoped>
  .array-bar {
      display: inline-block;
  }
</style>