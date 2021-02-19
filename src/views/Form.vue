<template>
  <div class="holder">
    <Form></Form>
  </div>
</template>
<script>
import Form from "@/components/Form.vue";
export default {
  name: "Forms",
  components: {
    Form
    }
}
</script>

<style>
.holder
{
    border:1px solid red;
    width: 50%;
    margin: auto;
}
</style>