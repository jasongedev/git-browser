<script lang="ts">
  import {onMount} from 'svelte';
  import compromise from "compromise";
  export let issue;
  export let currRowId;

  let topics: string[] = null;
  let isSelected: boolean = false;

  async function getEntities(body) {
    let doc = compromise(body)
    topics = await doc.toLowerCase().nouns().first(5).out('array');
    topics.forEach( (topic, index) => {
      topics[index] = topic.replace("[^a-zA-Z]+$", "");
    })
  }

  function setRowIndex() {
    currRowId = issue.id;
  }

  function checkIfSelected(currRowId) {
    if (currRowId === issue.id) {
      isSelected = true;
    } else {
      isSelected = false;
    }
  }

  $:getEntities(issue.body);
  $:checkIfSelected(currRowId);
</script>

<div>
  <div>
    <h3 on:click="{setRowIndex}">
      {issue.title}
    </h3>
    <div>Issue #{issue.number} is {issue.state}</div>
      {#if isSelected != true}
        <div class="expandedIssue">
          <div>{issue.user.login} says...</div>
          <div>{issue.body}</div>
          <small class ="timeStamp">Issue created at {issue.created_at}</small>
        </div>
      
      {/if}
      {#if topics != null}
      <div class="topics">
      {#each topics as topic}
        <div class="topic">{topic}</div>
      {/each}
      </div>
    {/if}
  </div>
</div>

<style>
  p {
		align-items: left;
    justify-content: left;
  }
  h3 {
    cursor:pointer;
    display: flex;
    flex-direction: frow;
    color: #0071DC;
  }
  div {
    padding-top: 0.25em;
    padding-bottom: 0.25em;
  }
  .topics {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
  }
  .topic {
    border-width: 0.15em;
    border-radius: 2em;
    border-color: #0071DC;
    border-style: solid;
    margin-right: 0.5em;
    padding-left: 0.5em;
    padding-right: 0.5em;
  }
  .expandedIssue {
    background-color: rgba(0, 113, 220, 0.1);
    border-radius: 1.5em;
    padding: 1em;
    margin: 0.5em;
  }
  .timeStamp {
    font-size: 12px;
    opacity: 0.75;
  }
</style>
