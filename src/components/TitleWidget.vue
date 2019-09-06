<template>
	<h1>{{data.text}}</h1>
</template>
<script lang="ts">
import Vue from "vue";
export default Vue.extend({
	props: {
		data: Object
	}
});
</script>
<style scoped>
</style>