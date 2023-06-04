<script lang="ts">
	import DataTable from './DataTable.svelte';

	let 字頭們 = '';
	let results = [] as string[][];

	const fetchData = async () => {
		if (字頭們.length) {
			const response = await fetch(`https://hdqt.nk2028.shn.hk/query?string=${字頭們}`);
			results = await response.json() as string[][];
		}
	};

	const handleSubmit = () => {
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

.query-para > * {
	vertical-align: middle;
}
</style>

<div class="page">
	<h1>漢字音典</h1>

	<p class="query-para">
		<input type="text" bind:value={字頭們}>
		<input type="submit" on:click={handleSubmit} value="查詢">
	</p>

	<DataTable results={results}/>

	<p>網站作者：<a href="https://github.com/nk2028">nk2028</a> - 資料來源：漢字音典（眾專家）</p>
</div>
