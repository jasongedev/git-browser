<script lang="ts">
  import {createEventDispatcher, onMount} from 'svelte';
  import {fetchOrg} from './utils/API.svelte';
  import Hoverable from './utils/Hoverable.svelte';
  
  export let orgName: string;
  export let projectName: string;
  export let fetchIssuesSignal: boolean;

  const dispatch = createEventDispatcher();
  let org = Promise.resolve([]);
  let orgNameInputElement: HTMLInputElement; 
  let projectNameInputElement: HTMLInputElement;
  
  const enterOrgName = () => {
    focusProjectName();
    org = fetchOrg(orgName);
  }

  const enterProjectName = () => {
    projectNameInputElement.blur();
    fetchIssuesSignal = !fetchIssuesSignal;
  }

  const focusOrgName = () => {
    orgNameInputElement.focus();
    orgNameInputElement.select();
  }

  const focusProjectName = () => {
    projectNameInputElement.select();
    projectNameInputElement.focus();
  }

  const onKeyPressOrgName = e => {
    if (e.charCode === 13) enterOrgName();
  }

  const onKeyPressProjectName = e => {
    if (e.charCode === 13) enterProjectName();
  }

	onMount(async () => {
    orgNameInputElement = document.getElementById("orgNameInputElement") as HTMLInputElement;
    projectNameInputElement = document.getElementById("projectNameInputElement") as HTMLInputElement;
    org = fetchOrg(orgName);
  })

</script>

<div class="inputsWrapper">
  <figure>
    {#if org == null}
      <div class="placeholder"></div>
    {:else}
      {#await org}
        <div class="placeholder"></div>
      {:then org}
        <img alt="Avatar" src="{org["avatar_url"]}">
      {/await}
    {/if}
  </figure>

  <div style="padding-left:0.5em"></div>
  
  <div>
    <div class="topRowOuter">
      <svelte:component this={Hoverable} let:hovering={isHoveringOrgNameInput}>
        <div class="topRowInner" on:click={focusOrgName} class:isHoveringOrgNameInput>
          <input class="orgNameInput" 
          id="orgNameInputElement"
          style="--width: {orgName.length * 0.6}em" 
          bind:value={orgName}
          on:keypress={onKeyPressOrgName}>
          <div class="forwardSlash">/</div>
        </div>
      </svelte:component>
    </div>

    <svelte:component this={Hoverable} let:hovering={isHoveringProjectNameInput}>
      <div on:click={focusProjectName} class:isHoveringProjectNameInput>
        <input class="projectNameInput" 
        id="projectNameInputElement" 
        style="--width: {projectName.length * 0.6}em" 
        bind:value={projectName}  
        on:keypress={onKeyPressProjectName}>
      </div>
    </svelte:component>
  </div>
</div>

<style>
	figure {
		text-align: center;
		margin: 0 0 1em 0;
	}

	img {
		width: 2.5em;
		height: 2.5em;
  }
  
  input {
    border-color: white;
    border-style:solid;
    width: var(--width);
    min-width: 0.6em;
		font-size: 1em;
    font-family: "Andale Mono", Courier, monospace
  }

  .placeholder {
		width: 2.5em;
		height: 2.5em;
  }

	.inputsWrapper {
		display: flex;
		flex-direction: row;
		flex-wrap: nowrap;
  }
  
  .topRowOuter {
    display: flex;
    align-items: flex-start;
  }
    
  .topRowInner {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
  }

  .orgNameInput {
    color: #041e42;
  }

  .projectNameInput {
    color: #0071DC;
  }

  .forwardSlash {
    user-select: none;
    color: #041e42;
    font-family: "Andale Mono", Courier, monospace;
    font-size: 1em;
  }

  .isHoveringOrgNameInput .orgNameInput {
      border-color: #0071DC;
      border-radius: 4px;
  }

  .isHoveringProjectNameInput .projectNameInput {
      border-color: #0071DC;
      border-radius: 4px;
  }
</style>
