<script lang="ts">
  import { slide } from 'svelte/transition';
  export let title: string;
  export let number: number;
  export let state: string;
  export let userLogin: string; 
  export let body: string;
  export let created: string;


  let isExpanded: boolean = false;

  const expand = () => {
    isExpanded = !isExpanded;
  }

</script>

<style>
  p {
		align-items: left;
    justify-content: left;
  }
  h3 {
    color: #0071DC;
    cursor:pointer;
  }
  div {
    padding-top: 0.5em;
    padding-bottom: 0.5em;
  }
  .expandedIssue {
    background-color: lightgrey;
    border-radius: 1em;
    padding: 1em;
  }
  .timeStamp {
    font-size: 12px;
    opacity: 0.75;
  }
</style>

<div>
	<p >
    <h3 on:click="{expand}">
      {title}
    </h3>
    <div>Issue #{number} is {state}</div>
    {#if isExpanded == true}
      <div class="expandedIssue">
        <div>{userLogin} says...</div>
        <div>{body}</div>
        <small class ="timeStamp">Issue created at {created}</small>
      </div>
    {/if}
</div>