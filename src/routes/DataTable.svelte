<script lang="ts">
	export let results: string[][];
</script>

<style>
table {
	border-collapse: collapse;
	margin: 0 auto;
	overflow-x: auto;
}

th, td {
	border: 1px solid;
	padding: 4px 6px;
}

td:first-child {
	white-space: nowrap;
}

td:not(:first-child) {
	width: 6em;
	word-break: break-all;
}

.name-tag {
	border-radius: 10px;
	color: white;
	font-size: 0.8em;
	padding: 3px 5px;
}

.text {
	font-size: 90%;
}
</style>

{#if results.length}
	<table>
		<thead>
			<tr>
				{#each results[0] as 字頭}
					<th>{字頭}</th>
				{/each}
			</tr>
		</thead>
		<tbody>
			{#each results.slice(1) as [簡稱, 分區, 顏色, ...字音們]}
				<tr>
					<td><span class="name-tag" style="background-color: {顏色};" title="{分區}">{簡稱}</span></td>
					{#each 字音們 as 字音}
						<td lang="zh-x-fonipa" class="text">{字音}</td>
					{/each}
				</tr>
			{/each}
		</tbody>
	</table>
{/if}
