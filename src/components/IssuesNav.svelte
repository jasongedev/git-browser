<script type="ts">
  export let issues: [];
  export let pageIndex: number;
  export let rowIndex: number;
  export let isLoading: boolean;
  export let timer: number;
  export let responseTime: number;

  const navigateLeft = () => {
     if (pageIndex > 1) pageIndex--;
  }

  const navigateRight = () => {
    if (issues.length > 9) pageIndex++;
  }

  const navigateUp = () => {
    if (rowIndex > 0) rowIndex--;
  }

  const navigateDown = () => {
    if (rowIndex < issues.length - 1) rowIndex++;
  }

  const handleKeydown = e => {
    let key = e.keyCode;
    switch (key) {
      case 37:
        navigateLeft();
        break;
      case 38:
        navigateUp();
        break;
      case 39:
        navigateRight();
        break;
      case 40:
        navigateDown();
        break;
    }
  }

</script>

<svelte:window on:keydown={handleKeydown}/>
<div class="issuesNavWrapper">
  <div class="leftAngleBracket" on:click="{navigateLeft}">←</div>
  {#if isLoading == true}
    <div class="issuesNavLabel">Page {pageIndex} loaded in {timer} ms</div>
  {:else}
    <div class="issuesNavLabel">Page {pageIndex} loaded in {responseTime} ms</div>
  {/if}
  <div class="rightAngleBracket" on:click="{navigateRight}">→</div>
</div>

<style>
  .issuesNavWrapper {
		display: flex;
		flex-direction: row;
		flex-wrap: nowrap;
    justify-content: center;
    align-content: center;
	}

	.rightAngleBracket ,.leftAngleBracket {
    padding: 0.5em;
    font-size: 2em;
    font-weight: 200;
    justify-content: center;
    align-content: center;
		color: #0071DC;
		cursor: pointer;
  }
  .issuesNavLabel {
    display: flex;
    align-self: center;
    color: #041e42;
  }
</style>