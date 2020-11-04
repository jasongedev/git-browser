<script lang="ts" context="module">
	const API_ROOT = "https://api.github.com/";

  export async function fetchOrg(orgName: string) {
		const orgFetchString = API_ROOT + "orgs/" + orgName;
		const userFetchString = API_ROOT + "users/" + orgName;

		console.log("attempting to fetch org or user with name: " + orgName);
		const responses = [await fetch(orgFetchString), await fetch(userFetchString)];

		if (responses[0].ok) {
			console.log("org found: " + orgName);
			const org = await responses[0].json();
      return org;
		} else if (responses[1].ok) {
			console.log("user found: " + orgName);
			const user = await responses[1].json();
			return user;
		} else {
			console.log("neither org nor user were found with the name: " + orgName);
			return null;
		}
	}

	export async function fetchIssues(orgName: string, projectName: string, pageIndex: number) {
    const API_ROOT = "https://api.github.com/";
		const paginationSubString = "?page=" + pageIndex + "&per_page=10";
		const issuesFetchString = API_ROOT + "repos/" + orgName + "/" + projectName + "/issues" + paginationSubString;

		const response = await fetch(issuesFetchString);

		if (response.ok) {
			let issues = await response.json();
			return issues;
		} else {
			return null;
		}
	}
</script>