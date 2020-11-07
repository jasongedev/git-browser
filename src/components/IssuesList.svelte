<script lang="ts">
  import {fetchIssues} from './utils/API.svelte';
  import IssuesNav from '../components/IssuesNav.svelte';
  import Issue from '../components/Issue.svelte';
  
  export let orgName: string;
  export let projectName: string;
  export let fetchIssuesSignal: boolean;

  let issues: [] = null;
  let pageIndex: number;
  let rowIndex: number;
  let currRowId: string;
  let timer: number = 0;
  let responseTime: number = 0;
  let isLoading: boolean = true;

  async function navFetch(pageIndex) {
    isLoading = true;
    issues = await fetchIssues(orgName, projectName, pageIndex);
    isLoading = false;
    responseTime = timer;
    timer = 0;
    rowIndex = 0;
    rowSelect(0);
  }

  function newFetch(fetchIssuesSignal) {
    if (pageIndex != 1) {
      pageIndex = 1;
    } else {
      navFetch(pageIndex);
    }
  }

  function rowSelect(rowIndex) {
    console.log(rowIndex)
    if (issues != null) {
      currRowId = issues[rowIndex]["id"];
    }
  }

  setInterval(() => {
    if (isLoading == true) {
      timer += 1;
    }
  }, 1);

  $:navFetch(pageIndex);
  $:newFetch(fetchIssuesSignal);
  $:rowSelect(rowIndex);
</script>

<div class="issuesListWrapper">
  {#if issues != null} 
    <IssuesNav bind:issues bind:pageIndex bind:rowIndex bind:isLoading bind:timer bind:responseTime/>
    {#each issues as issue, index}
      {#if isLoading == true} 
        <div class="loading issuesWrapper" id="issue#{index}">
          <Issue bind:issue bind:currRowId/>
        </div>
      {:else}
        <div class="issuesWrapper" id="issue#{index}">
          <Issue bind:issue bind:currRowId/>
        </div>
      {/if}
    {/each}
  {/if}
</div>

<style>
	.issuesListWrapper {
		display: flex;
		flex-direction: column;
		flex-wrap: nowrap;
		justify-content: center;
		padding-left: 1em;
    padding-right: 1em;
		margin-top: -2em;
  }
  .loading {
    opacity: 0.5;
  }
	.issuesWrapper {
		color: #041e42;
		display: flex;
		flex-direction: row;
		flex-wrap: nowrap;
		align-items: flex-start
	}
</style>