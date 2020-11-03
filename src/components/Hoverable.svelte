<script lang="ts">
	let hovering;
	function enter() {
		hovering = true;
	}
	function leave() {
		hovering = false;
	}
</script>

<div on:mouseenter={enter} on:mouseleave={leave}>
	<slot hovering={hovering}></slot>
</div>