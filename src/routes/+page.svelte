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

form {
	margin: 1em 0;
}

form > * {
	vertical-align: middle;
}

footer {
	font-size: 90%;
}
</style>

<div class="page">
	<h1>漢字音典</h1>

	<form on:submit={handleSubmit}>
		<input type="text" bind:value={字頭們}>
		<input type="submit" value="查詢">
	</form>

	<DataTable results={results}/>

	<footer>
		<p>網站作者：<a href="https://github.com/nk2028/hdqt">nk2028</a> - 資料來源：漢字音典（眾專家）</p>
	</footer>
</div>
