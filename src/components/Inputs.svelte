<script lang="ts">
  import Hoverable from './Hoverable.svelte';
  import { createEventDispatcher } from 'svelte';

  export let orgName: string = "walmartlabs";
  export let projectName: string = "thorax";

  const dispatch = createEventDispatcher();

  const enterOrgName = () => {
    document.getElementById("projectNameInput").focus();
    dispatch('orgNameEntered');
  }

  const enterProjectName = () => {
    document.getElementById("projectNameInput").blur();
    dispatch('projectNameEntered');
  }

  const onKeyPressOrgName = e => {
    if (e.charCode === 13) enterOrgName();
  }

  const onKeyPressProjectName = e => {
    if (e.charCode === 13) enterProjectName();
  }

  const onFocusOrgName = () => {
    let orgNameInput = document.getElementById("orgNameInput") as HTMLInputElement;
    orgNameInput.select();
  }

  const onFocusProjectName = () => {
    let projectNameInput = document.getElementById("projectNameInput") as HTMLInputElement;
    projectNameInput.select();
  }

</script>

<style>
  input {
    border-color: white;
    border-style:solid;
    width: var(--width);
    min-width: 3em;
		font-size: 1em;
    font-family: "Andale Mono", Courier, monospace
  }
  .inputsTop {
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
    color: #041e42;
    font-size: 1em;
    font-family: "Andale Mono", Courier, monospace
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

<div>
  <div class="inputsTop">
    <svelte:component this={Hoverable} let:hovering={isHoveringOrgNameInput}>
      <div class:isHoveringOrgNameInput>
        <input class="orgNameInput" id="orgNameInput"
        style="--width: {orgName.length * 0.6}em" 
        bind:value={orgName} 
        on:keypress={onKeyPressOrgName} 
        on:focus={onFocusOrgName}>
      </div>
    </svelte:component>
    <div class="forwardSlash">/</div>
  </div>
  <svelte:component this={Hoverable} let:hovering={isHoveringProjectNameInput}>
    <div class:isHoveringProjectNameInput>
      <input class="projectNameInput" id="projectNameInput" 
      style="--width: {projectName.length * 0.6}em" 
      bind:value={projectName}  
      on:keypress={onKeyPressProjectName}
      on:focus={onFocusProjectName}>
    </div>
  </svelte:component>
</div>
