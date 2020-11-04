<script lang="ts">
  import {onMount} from 'svelte';
  import compromise from "compromise";
  export let rowIndex;
  export let issue;

  let topics: Promise<[]> = Promise.resolve([]);
  let isExpanded: boolean = false;

  async function getEntities(body) {
    let doc = compromise(body)
    topics = doc.topics().json();
  }

  const expand = () => {
    isExpanded = !isExpanded;
    console.log(issue);
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
    padding-top: 0.25em;
    padding-bottom: 0.25em;
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
      {issue.title}
    </h3>
    <div>Issue #{issue.number} is {issue.state}</div>
    {#await topics}
    <div>{topics}</div>
    {/await}
    {#if isExpanded == true}
      <div class="expandedIssue">
        <div>{issue.user.login} says...</div>
        <div>{issue.body}</div>
        <small class ="timeStamp">Issue created at {issue.created_at}</small>
      </div>
    {/if}
</div>