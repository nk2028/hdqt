<script lang="ts">
	export let results: string[][];
	const headers: { [key: string]: string[] } = {
		廣韻: [
			'l切拼', 'l白一平', 'l古韻', 'l有女',
			'i髙本漢', 'i王力{1957}', 'i王力{1985}', 'i李榮', 'i邵榮芬', 'i蒲立本', 'i鄭張尙芳', 'i潘悟雲{2000}', 'i潘悟雲{2013}', 'i潘悟雲{2023}', 'iunt{2020}', 'iunt{2022}', 'iunt{通俗}', 'imsoeg',
			'h描述{切韻音系}', '#攝', 'h描述{方音字彙}',
			'h廣韻{韻目原貌}', 'h平水{折合韻目}',
			'h反切',
		],
		中唐: ['iunt', 'i辛克'],
		中原音韻: ['i楊耐思', 'i寧繼福', 'i薛鳳生{音位形式}', 'iunt{音位形式}', 'iunt'],
		東干語: ['c拼寫', 'i音標'],
	};
	const dialectsOfRomanization = ['普通話', '香港', '臺灣', '越南', '朝鮮', '日語吳音', '日語漢音', '日語其他'];
	const dialectsWithBold = ['中原音韻', '日語吳音', '日語漢音', '日語其他'];
	function wrapIPA(字音: string) {
		return `<span lang="zh-Latn-fonipa">${字音}</span>`;
	}
	function wrapRomanization(字音: string, script: string = 'Latn') {
		return `<span lang="zh-${script}">${字音}</span>`;
	}
	function process字音s(字音: string, 簡稱: string) {
		return 字音
			.split('\t')
			.map(s => {
				let [音標, 解釋] = s.replace('}', '').split('{');
				let isBold = dialectsWithBold.includes(簡稱) && 音標.includes('*');
				if (headers[簡稱]) {
					if (isBold) 音標 = 音標.replace(/\*/g, '');
					音標 = 音標
						.split('/')
						.map((s, i) => {
							let header = headers[簡稱][i];
							if (!s || !header || header[0] === '#') return '';
							switch (header[0]) {
								case 'i': s = wrapIPA(s); break;
								case 'l': s = wrapRomanization(s); break;
								case 'c': s = wrapRomanization(s, 'Cyrl'); break;
							}
							header = header.slice(1);
							if (isBold) s = `*${s}*`;
							return header ? header + '：' + s : s;
						})
						.filter(Boolean)
						.join('<br>');
				} else if (dialectsOfRomanization.includes(簡稱)) {
					音標 = wrapRomanization(音標);
				} else {
					音標 = wrapIPA(音標);
				}
				if (isBold) 音標 = 音標.replace(/\*(.*?)\*/g, '<strong>$1</strong>');
				音標 = 音標.replace(/\|(.*?)\|/g, '<span style="opacity: 0.5;">$1</span>');
				解釋 = 解釋?.replace(/ /g, '');
				if (解釋) {
					if (headers[簡稱]) 音標 += '<br>';
					音標 += `{${解釋}}`;
				}
				return 音標;
			})
			.join(headers[簡稱] ? '<br><br>' : ' ')
			.replace(/\{(.*?)\}/g, '<span class="desc">$1</span>');
	}
</script>

<style>
table {
	border-collapse: collapse;
	margin: 0 auto;
	overflow-x: auto;
	line-height: 1.5;
}

th, td {
	padding: 0.25em;
	width: 12em;
	text-align: start;
	vertical-align: top;
}

th:first-child, td:first-child {
	width: auto;
	text-align: end;
}

.name-tag {
	border-radius: 0.5em;
	color: white;
	padding: 0.15em 0.2em;
	white-space: nowrap;
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
						<td class="text">{@html process字音s(字音, 簡稱)}</td>
					{/each}
				</tr>
			{/each}
		</tbody>
	</table>
{/if}
