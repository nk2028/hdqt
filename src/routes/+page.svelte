<script lang="ts">
	import DataTable from './DataTable.svelte';

	let 字頭們 = '';
	let results = [] as string[][];
	let pending = false;

	const fetchData = async () => {
		if (字頭們.length) {
			const response = await fetch(`https://chinese-dialect-pronunciation-atlas-beg4zxgxla-wl.a.run.app/query?string=${字頭們}`);
			results = await response.json() as string[][];
		}
		pending = false;
	};

	const handleSubmit = () => {
		pending = true;
		fetchData();
	};
</script>

<style>
.page {
	text-align: center;
}

input {
	background-color: inherit;
	color: inherit;
	font-size: 1em;
}

input[type="text"] {
	border: 1px solid;
	border-radius: 3px;
	padding: 6px 8px;
}

input[type="submit"] {
	border: 1px solid;
	border-radius: 5px;
	padding: 6px 8px;
}

form {
	margin: 1em 0;
}

form > * {
	vertical-align: middle;
}
</style>

<div class="page">
	<h1>漢字音典</h1>

	<form on:submit={handleSubmit}>
		<input type="text" bind:value={字頭們}>
		<input type="submit" value="查詢">
	</form>

	{#if pending}
		<p>正在查詢，請稍候……</p>
	{/if}
	<DataTable results={results}/>

	<footer>
		<p>網站作者：<a href="https://github.com/nk2028/hdqt">nk2028</a> - 資料來源：<a href="https://github.com/osfans/MCPDict">漢字音典（眾專家）</a></p>
	</footer>
</div>
