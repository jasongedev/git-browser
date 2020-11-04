<script lang="ts">
	import Inputs from '../components/Inputs.svelte';
	import IssuesList from '../components/IssuesList.svelte';

	let orgName: string = "jasongedev";
	let projectName: string = "issue-browser";
	let fetchIssuesSignal: boolean = false;
</script>

<svelte:head>
	<title>NLP Issue Browser</title>
</svelte:head>

<h1>
	<Inputs bind:orgName bind:projectName bind:fetchIssuesSignal/>
</h1>

<body>
	<IssuesList bind:orgName bind:projectName bind:fetchIssuesSignal/>
</body>

<style>
	h1 {
		font-weight: 600;
		margin-bottom: 0;
	}

	body {
		align-self: center;
	}

	@media (min-width: 960px) {
		h1 {
			font-size: 4em;
		}
	}
</style>
