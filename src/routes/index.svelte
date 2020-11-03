<script lang="ts">
	import successkid from 'images/successkid.jpg';
  import { onMount } from 'svelte';
	import Inputs from '../components/Inputs.svelte';
	import Issue from '../components/Issue.svelte';

  const apiRoot = "https://api.github.com/"

	let orgName: string;
	let projectName: string;
	let orgAvatarUrl = Promise.resolve([]);
	let projectIssues = Promise.resolve([]);
	let avatarIsLoaded: boolean = false;
	let issuesIsLoaded: boolean = false;
	let currIndex: number;
	let currPageLength: number;
	let loadTime: number;

	async function getOrgAvatarUrl() {
		const orgFetchString = apiRoot + "orgs/" + orgName;
		let response = await fetch(orgFetchString);

		if (response.ok) {
			console.log(orgFetchString + " org found")
			const org = await response.json();
	  	avatarIsLoaded = true;
  		return org;
		} else {
			const userFetchString = apiRoot + "users/" + orgName;
			response = await fetch(userFetchString);

			if (response.ok) {
				console.log(userFetchString + " user found")
				const user = await response.json();
				avatarIsLoaded = true;
				return user;
			} else {
				return null;
			}
		}
	}

	async function getProjectIssues(pageIndex) {
		const paginationString = "?page=" + pageIndex + "&per_page=10";
		const issuesFetchString = apiRoot + "repos/" + orgName + "/" + projectName + "/issues" + paginationString;

		const requestTime = Date.now();
		let response = await fetch(issuesFetchString);

		if (response.ok) {
			console.log(issuesFetchString + " issues found")
			const responseTime = Date.now();
      loadTime = responseTime - requestTime;
			const issues = await response.json();
			issuesIsLoaded = true;
	  	currIndex = pageIndex;
			return issues;
		} else {
			return null;
		}
	}

	const orgNameEntered = () => {
		orgAvatarUrl = getOrgAvatarUrl();
	}

	const projectNameEntered = () => {
		projectIssues = getProjectIssues(1);
	}

	const navigateLeft = () => {
		if (currIndex > 1) {
					currIndex -= 1;
					projectIssues = getProjectIssues(currIndex);
				}
	}

	const navigateRight = () => {
		//hard-coded for now
		if (currIndex < 5) {
					currIndex += 1;
					projectIssues = getProjectIssues(currIndex);
				}
	}


	const handleKeydown = e => {
		let key = e.keyCode;
		switch (key) {
			case 37:
				navigateLeft();
				break;
			case 39:
			  navigateRight();
				break;
		}
	}

	onMount(async () => {
		orgNameEntered();
		projectNameEntered();
	})
</script>

<style>
	figure {
		text-align: center;
		margin: 0 auto;
	}

	h1 {
		font-weight: 600;
		margin: 0;
	}

	figure {
		margin: 0 0 1em 0;
	}

	img {
		width: 2.5em;
		height: 2.5em;
	}

	body {
		margin: 1em auto;
		align-self: center;
	}

	.headerWrapper {
		display: flex;
		flex-direction: row;
		flex-wrap: nowrap;
	}
	.subHeaderWrapper {
		margin-top: -1em;
		display: flex;
		flex-direction: row;
		flex-wrap: nowrap;
		align-items: center;
		justify-content: center;
	}
  .leftAngleBracket {
		padding-right: 1em;
		cursor: pointer;
	}
  .rightAngleBracket {
		padding-left: 1em;
		cursor: pointer;
	}

	.bodyWrapper {
		display: flex;
		flex-direction: column;
		flex-wrap: nowrap;
		align-items: flex-start;
		justify-content: center;
		padding-left: 1em;
		padding-right: 1em;
	}
	.issueWrapper {

		color: #041e42;
		display: flex;
		flex-direction: row;
		flex-wrap: nowrap;
		align-items: flex-start
	}

	@media (min-width: 960px) {
		h1 {
			font-size: 4em;
		}
	}
</style>

<svelte:head>
	<title>Sapper project template</title>
</svelte:head>

<svelte:window on:keydown={handleKeydown}/>

<h1>
	<div class="headerWrapper">
		<figure>
			{#if avatarIsLoaded == true}
				{#await orgAvatarUrl}
					<img alt="Loading" src="{successkid}">
				{:then data}
					<img alt="Avatar" src="{data["avatar_url"]}">
				{/await}
			{:else}
			  <img alt="Loading" src="{successkid}">
			{/if}
		</figure>
		<div style="padding-left:0.5em"></div>
		<Inputs bind:orgName on:orgNameEntered={orgNameEntered}
		bind:projectName on:projectNameEntered={projectNameEntered}/>
	</div>
</h1>

<div class="subHeaderWrapper">
	{#if issuesIsLoaded == true}
	  <div class="leftAngleBracket" on:click="{navigateLeft}"> &lt&lt </div>
		<div>Loaded page {currIndex} in {loadTime} ms</div>
	  <div class="rightAngleBracket" on:click="{navigateRight}"> &gt&gt </div>
	{/if}
</div>

<body>
	<div class="bodyWrapper">
		{#if issuesIsLoaded == true}
			{#await projectIssues}
			  <div></div>
			{:then projectIssues}
				{#each projectIssues as issue, index}
				  <div class="issueWrapper" id="issue#{index}">
						<div></div>
						<Issue bind:title="{issue.title}"
						bind:number="{issue.number}"
						bind:state="{issue.state}"
						bind:userLogin="{issue.user.login}"
						bind:body="{issue.body}"
						bind:created="{issue.created_at}"
						bind:updated="{issue.updated_at}"/>
			  	</div>
				{/each}
			{/await}
		{/if}
	</div>
</body>