<script lang="ts" context="module">
  export async function getResults(): string {
    let response = await fetch('https://api.github.com/repos/TypeStrong/ts-loader/issues?page=3&per_page=10');
    
    return "ok";
  }
      




</script>