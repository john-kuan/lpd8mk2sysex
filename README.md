<html><head><meta http-equiv="Content-Type" content="text/html; charset=utf-8"/><title>LPD8 MK2 SysEx Implementation</title><style>
/* cspell:disable-file */
/* webkit printing magic: print all background colors */
html {
	-webkit-print-color-adjust: exact;
}
* {
	box-sizing: border-box;
	-webkit-print-color-adjust: exact;
}

html,
body {
	margin: 0;
	padding: 0;
}
@media only screen {
	body {
		margin: 2em auto;
		max-width: 900px;
		color: rgb(55, 53, 47);
	}
}

body {
	line-height: 1.5;
	white-space: pre-wrap;
}

a,
a.visited {
	color: inherit;
	text-decoration: underline;
}

.pdf-relative-link-path {
	font-size: 80%;
	color: #444;
}

h1,
h2,
h3 {
	letter-spacing: -0.01em;
	line-height: 1.2;
	font-weight: 600;
	margin-bottom: 0;
}

.page-title {
	font-size: 2.5rem;
	font-weight: 700;
	margin-top: 0;
	margin-bottom: 0.75em;
}

h1 {
	font-size: 1.875rem;
	margin-top: 1.875rem;
}

h2 {
	font-size: 1.5rem;
	margin-top: 1.5rem;
}

h3 {
	font-size: 1.25rem;
	margin-top: 1.25rem;
}

.source {
	border: 1px solid #ddd;
	border-radius: 3px;
	padding: 1.5em;
	word-break: break-all;
}

.callout {
	border-radius: 3px;
	padding: 1rem;
}

figure {
	margin: 1.25em 0;
	page-break-inside: avoid;
}

figcaption {
	opacity: 0.5;
	font-size: 85%;
	margin-top: 0.5em;
}

mark {
	background-color: transparent;
}

.indented {
	padding-left: 1.5em;
}

hr {
	background: transparent;
	display: block;
	width: 100%;
	height: 1px;
	visibility: visible;
	border: none;
	border-bottom: 1px solid rgba(55, 53, 47, 0.09);
}

img {
	max-width: 100%;
}

@media only print {
	img {
		max-height: 100vh;
		object-fit: contain;
	}
}

@page {
	margin: 1in;
}

.collection-content {
	font-size: 0.875rem;
}

.column-list {
	display: flex;
	justify-content: space-between;
}

.column {
	padding: 0 1em;
}

.column:first-child {
	padding-left: 0;
}

.column:last-child {
	padding-right: 0;
}

.table_of_contents-item {
	display: block;
	font-size: 0.875rem;
	line-height: 1.3;
	padding: 0.125rem;
}

.table_of_contents-indent-1 {
	margin-left: 1.5rem;
}

.table_of_contents-indent-2 {
	margin-left: 3rem;
}

.table_of_contents-indent-3 {
	margin-left: 4.5rem;
}

.table_of_contents-link {
	text-decoration: none;
	opacity: 0.7;
	border-bottom: 1px solid rgba(55, 53, 47, 0.18);
}

table,
th,
td {
	border: 1px solid rgba(55, 53, 47, 0.09);
	border-collapse: collapse;
}

table {
	border-left: none;
	border-right: none;
}

th,
td {
	font-weight: normal;
	padding: 0.25em 0.5em;
	line-height: 1.5;
	min-height: 1.5em;
	text-align: left;
}

th {
	color: rgba(55, 53, 47, 0.6);
}

ol,
ul {
	margin: 0;
	margin-block-start: 0.6em;
	margin-block-end: 0.6em;
}

li > ol:first-child,
li > ul:first-child {
	margin-block-start: 0.6em;
}

ul > li {
	list-style: disc;
}

ul.to-do-list {
	padding-inline-start: 0;
}

ul.to-do-list > li {
	list-style: none;
}

.to-do-children-checked {
	text-decoration: line-through;
	opacity: 0.375;
}

ul.toggle > li {
	list-style: none;
}

ul {
	padding-inline-start: 1.7em;
}

ul > li {
	padding-left: 0.1em;
}

ol {
	padding-inline-start: 1.6em;
}

ol > li {
	padding-left: 0.2em;
}

.mono ol {
	padding-inline-start: 2em;
}

.mono ol > li {
	text-indent: -0.4em;
}

.toggle {
	padding-inline-start: 0em;
	list-style-type: none;
}

/* Indent toggle children */
.toggle > li > details {
	padding-left: 1.7em;
}

.toggle > li > details > summary {
	margin-left: -1.1em;
}

.selected-value {
	display: inline-block;
	padding: 0 0.5em;
	background: rgba(206, 205, 202, 0.5);
	border-radius: 3px;
	margin-right: 0.5em;
	margin-top: 0.3em;
	margin-bottom: 0.3em;
	white-space: nowrap;
}

.collection-title {
	display: inline-block;
	margin-right: 1em;
}

.page-description {
	margin-bottom: 2em;
}

.simple-table {
	margin-top: 1em;
	font-size: 0.875rem;
	empty-cells: show;
}
.simple-table td {
	height: 29px;
	min-width: 120px;
}

.simple-table th {
	height: 29px;
	min-width: 120px;
}

.simple-table-header-color {
	background: rgb(247, 246, 243);
	color: black;
}
.simple-table-header {
	font-weight: 500;
}

time {
	opacity: 0.5;
}

.icon {
	display: inline-block;
	max-width: 1.2em;
	max-height: 1.2em;
	text-decoration: none;
	vertical-align: text-bottom;
	margin-right: 0.5em;
}

img.icon {
	border-radius: 3px;
}

.user-icon {
	width: 1.5em;
	height: 1.5em;
	border-radius: 100%;
	margin-right: 0.5rem;
}

.user-icon-inner {
	font-size: 0.8em;
}

.text-icon {
	border: 1px solid #000;
	text-align: center;
}

.page-cover-image {
	display: block;
	object-fit: cover;
	width: 100%;
	max-height: 30vh;
}

.page-header-icon {
	font-size: 3rem;
	margin-bottom: 1rem;
}

.page-header-icon-with-cover {
	margin-top: -0.72em;
	margin-left: 0.07em;
}

.page-header-icon img {
	border-radius: 3px;
}

.link-to-page {
	margin: 1em 0;
	padding: 0;
	border: none;
	font-weight: 500;
}

p > .user {
	opacity: 0.5;
}

td > .user,
td > time {
	white-space: nowrap;
}

input[type="checkbox"] {
	transform: scale(1.5);
	margin-right: 0.6em;
	vertical-align: middle;
}

p {
	margin-top: 0.5em;
	margin-bottom: 0.5em;
}

.image {
	border: none;
	margin: 1.5em 0;
	padding: 0;
	border-radius: 0;
	text-align: center;
}

.code,
code {
	background: rgba(135, 131, 120, 0.15);
	border-radius: 3px;
	padding: 0.2em 0.4em;
	border-radius: 3px;
	font-size: 85%;
	tab-size: 2;
}

code {
	color: #eb5757;
}

.code {
	padding: 1.5em 1em;
}

.code-wrap {
	white-space: pre-wrap;
	word-break: break-all;
}

.code > code {
	background: none;
	padding: 0;
	font-size: 100%;
	color: inherit;
}

blockquote {
	font-size: 1.25em;
	margin: 1em 0;
	padding-left: 1em;
	border-left: 3px solid rgb(55, 53, 47);
}

.bookmark {
	text-decoration: none;
	max-height: 8em;
	padding: 0;
	display: flex;
	width: 100%;
	align-items: stretch;
}

.bookmark-title {
	font-size: 0.85em;
	overflow: hidden;
	text-overflow: ellipsis;
	height: 1.75em;
	white-space: nowrap;
}

.bookmark-text {
	display: flex;
	flex-direction: column;
}

.bookmark-info {
	flex: 4 1 180px;
	padding: 12px 14px 14px;
	display: flex;
	flex-direction: column;
	justify-content: space-between;
}

.bookmark-image {
	width: 33%;
	flex: 1 1 180px;
	display: block;
	position: relative;
	object-fit: cover;
	border-radius: 1px;
}

.bookmark-description {
	color: rgba(55, 53, 47, 0.6);
	font-size: 0.75em;
	overflow: hidden;
	max-height: 4.5em;
	word-break: break-word;
}

.bookmark-href {
	font-size: 0.75em;
	margin-top: 0.25em;
}

.sans { font-family: ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI Variable Display", "Segoe UI", Helvetica, "Apple Color Emoji", Arial, sans-serif, "Segoe UI Emoji", "Segoe UI Symbol"; }
.code { font-family: "SFMono-Regular", Menlo, Consolas, "PT Mono", "Liberation Mono", Courier, monospace; }
.serif { font-family: Lyon-Text, Georgia, ui-serif, serif; }
.mono { font-family: iawriter-mono, Nitti, Menlo, Courier, monospace; }
.pdf .sans { font-family: Inter, ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI Variable Display", "Segoe UI", Helvetica, "Apple Color Emoji", Arial, sans-serif, "Segoe UI Emoji", "Segoe UI Symbol", 'Twemoji', 'Noto Color Emoji', 'Noto Sans CJK JP'; }
.pdf:lang(zh-CN) .sans { font-family: Inter, ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI Variable Display", "Segoe UI", Helvetica, "Apple Color Emoji", Arial, sans-serif, "Segoe UI Emoji", "Segoe UI Symbol", 'Twemoji', 'Noto Color Emoji', 'Noto Sans CJK SC'; }
.pdf:lang(zh-TW) .sans { font-family: Inter, ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI Variable Display", "Segoe UI", Helvetica, "Apple Color Emoji", Arial, sans-serif, "Segoe UI Emoji", "Segoe UI Symbol", 'Twemoji', 'Noto Color Emoji', 'Noto Sans CJK TC'; }
.pdf:lang(ko-KR) .sans { font-family: Inter, ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI Variable Display", "Segoe UI", Helvetica, "Apple Color Emoji", Arial, sans-serif, "Segoe UI Emoji", "Segoe UI Symbol", 'Twemoji', 'Noto Color Emoji', 'Noto Sans CJK KR'; }
.pdf .code { font-family: Source Code Pro, "SFMono-Regular", Menlo, Consolas, "PT Mono", "Liberation Mono", Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK JP'; }
.pdf:lang(zh-CN) .code { font-family: Source Code Pro, "SFMono-Regular", Menlo, Consolas, "PT Mono", "Liberation Mono", Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK SC'; }
.pdf:lang(zh-TW) .code { font-family: Source Code Pro, "SFMono-Regular", Menlo, Consolas, "PT Mono", "Liberation Mono", Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK TC'; }
.pdf:lang(ko-KR) .code { font-family: Source Code Pro, "SFMono-Regular", Menlo, Consolas, "PT Mono", "Liberation Mono", Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK KR'; }
.pdf .serif { font-family: PT Serif, Lyon-Text, Georgia, ui-serif, serif, 'Twemoji', 'Noto Color Emoji', 'Noto Serif CJK JP'; }
.pdf:lang(zh-CN) .serif { font-family: PT Serif, Lyon-Text, Georgia, ui-serif, serif, 'Twemoji', 'Noto Color Emoji', 'Noto Serif CJK SC'; }
.pdf:lang(zh-TW) .serif { font-family: PT Serif, Lyon-Text, Georgia, ui-serif, serif, 'Twemoji', 'Noto Color Emoji', 'Noto Serif CJK TC'; }
.pdf:lang(ko-KR) .serif { font-family: PT Serif, Lyon-Text, Georgia, ui-serif, serif, 'Twemoji', 'Noto Color Emoji', 'Noto Serif CJK KR'; }
.pdf .mono { font-family: PT Mono, iawriter-mono, Nitti, Menlo, Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK JP'; }
.pdf:lang(zh-CN) .mono { font-family: PT Mono, iawriter-mono, Nitti, Menlo, Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK SC'; }
.pdf:lang(zh-TW) .mono { font-family: PT Mono, iawriter-mono, Nitti, Menlo, Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK TC'; }
.pdf:lang(ko-KR) .mono { font-family: PT Mono, iawriter-mono, Nitti, Menlo, Courier, monospace, 'Twemoji', 'Noto Color Emoji', 'Noto Sans Mono CJK KR'; }
.highlight-default {
	color: rgba(50, 48, 44, 1);
}
.highlight-gray {
	color: rgba(115, 114, 110, 1);
	fill: rgba(115, 114, 110, 1);
}
.highlight-brown {
	color: rgba(159, 107, 83, 1);
	fill: rgba(159, 107, 83, 1);
}
.highlight-orange {
	color: rgba(217, 115, 13, 1);
	fill: rgba(217, 115, 13, 1);
}
.highlight-yellow {
	color: rgba(203, 145, 47, 1);
	fill: rgba(203, 145, 47, 1);
}
.highlight-teal {
	color: rgba(68, 131, 97, 1);
	fill: rgba(68, 131, 97, 1);
}
.highlight-blue {
	color: rgba(51, 126, 169, 1);
	fill: rgba(51, 126, 169, 1);
}
.highlight-purple {
	color: rgba(144, 101, 176, 1);
	fill: rgba(144, 101, 176, 1);
}
.highlight-pink {
	color: rgba(193, 76, 138, 1);
	fill: rgba(193, 76, 138, 1);
}
.highlight-red {
	color: rgba(205, 60, 58, 1);
	fill: rgba(205, 60, 58, 1);
}
.highlight-default_background {
	color: rgba(50, 48, 44, 1);
}
.highlight-gray_background {
	background: rgba(248, 248, 247, 1);
}
.highlight-brown_background {
	background: rgba(244, 238, 238, 1);
}
.highlight-orange_background {
	background: rgba(251, 236, 221, 1);
}
.highlight-yellow_background {
	background: rgba(251, 243, 219, 1);
}
.highlight-teal_background {
	background: rgba(237, 243, 236, 1);
}
.highlight-blue_background {
	background: rgba(231, 243, 248, 1);
}
.highlight-purple_background {
	background: rgba(248, 243, 252, 1);
}
.highlight-pink_background {
	background: rgba(252, 241, 246, 1);
}
.highlight-red_background {
	background: rgba(253, 235, 236, 1);
}
.block-color-default {
	color: inherit;
	fill: inherit;
}
.block-color-gray {
	color: rgba(115, 114, 110, 1);
	fill: rgba(115, 114, 110, 1);
}
.block-color-brown {
	color: rgba(159, 107, 83, 1);
	fill: rgba(159, 107, 83, 1);
}
.block-color-orange {
	color: rgba(217, 115, 13, 1);
	fill: rgba(217, 115, 13, 1);
}
.block-color-yellow {
	color: rgba(203, 145, 47, 1);
	fill: rgba(203, 145, 47, 1);
}
.block-color-teal {
	color: rgba(68, 131, 97, 1);
	fill: rgba(68, 131, 97, 1);
}
.block-color-blue {
	color: rgba(51, 126, 169, 1);
	fill: rgba(51, 126, 169, 1);
}
.block-color-purple {
	color: rgba(144, 101, 176, 1);
	fill: rgba(144, 101, 176, 1);
}
.block-color-pink {
	color: rgba(193, 76, 138, 1);
	fill: rgba(193, 76, 138, 1);
}
.block-color-red {
	color: rgba(205, 60, 58, 1);
	fill: rgba(205, 60, 58, 1);
}
.block-color-default_background {
	color: inherit;
	fill: inherit;
}
.block-color-gray_background {
	background: rgba(248, 248, 247, 1);
}
.block-color-brown_background {
	background: rgba(244, 238, 238, 1);
}
.block-color-orange_background {
	background: rgba(251, 236, 221, 1);
}
.block-color-yellow_background {
	background: rgba(251, 243, 219, 1);
}
.block-color-teal_background {
	background: rgba(237, 243, 236, 1);
}
.block-color-blue_background {
	background: rgba(231, 243, 248, 1);
}
.block-color-purple_background {
	background: rgba(248, 243, 252, 1);
}
.block-color-pink_background {
	background: rgba(252, 241, 246, 1);
}
.block-color-red_background {
	background: rgba(253, 235, 236, 1);
}
.select-value-color-default { background-color: rgba(84, 72, 49, 0.08); }
.select-value-color-gray { background-color: rgba(84, 72, 49, 0.15); }
.select-value-color-brown { background-color: rgba(210, 162, 141, 0.35); }
.select-value-color-orange { background-color: rgba(224, 124, 57, 0.27); }
.select-value-color-yellow { background-color: rgba(236, 191, 66, 0.39); }
.select-value-color-green { background-color: rgba(123, 183, 129, 0.27); }
.select-value-color-blue { background-color: rgba(93, 165, 206, 0.27); }
.select-value-color-purple { background-color: rgba(168, 129, 197, 0.27); }
.select-value-color-pink { background-color: rgba(225, 136, 179, 0.27); }
.select-value-color-red { background-color: rgba(244, 171, 159, 0.4); }

.checkbox {
	display: inline-flex;
	vertical-align: text-bottom;
	width: 16;
	height: 16;
	background-size: 16px;
	margin-left: 2px;
	margin-right: 5px;
}

.checkbox-on {
	background-image: url("data:image/svg+xml;charset=UTF-8,%3Csvg%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%0A%3Crect%20width%3D%2216%22%20height%3D%2216%22%20fill%3D%22%2358A9D7%22%2F%3E%0A%3Cpath%20d%3D%22M6.71429%2012.2852L14%204.9995L12.7143%203.71436L6.71429%209.71378L3.28571%206.2831L2%207.57092L6.71429%2012.2852Z%22%20fill%3D%22white%22%2F%3E%0A%3C%2Fsvg%3E");
}

.checkbox-off {
	background-image: url("data:image/svg+xml;charset=UTF-8,%3Csvg%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%0A%3Crect%20x%3D%220.75%22%20y%3D%220.75%22%20width%3D%2214.5%22%20height%3D%2214.5%22%20fill%3D%22white%22%20stroke%3D%22%2336352F%22%20stroke-width%3D%221.5%22%2F%3E%0A%3C%2Fsvg%3E");
}
	
</style></head><body><article id="2340f46d-3902-8049-b586-d5a04ce4e260" class="page sans"><header><h1 class="page-title">LPD8 MK2 SysEx Implementation</h1><p class="page-description"></p></header><div class="page-body"><h2 id="2340f46d-3902-80ba-beb8-dff97f801c8b" class="">SysEx Message Structure</h2><script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/prism.min.js" integrity="sha512-7Z9J3l1+EYfeaPKcGXu3MS/7T+w19WtKQY/n+xzmw4hZhJ9tyYmcUS+4QqAlzhicE5LAfMQSF3iFTK9bQdTxXg==" crossorigin="anonymous" referrerPolicy="no-referrer"></script><link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/themes/prism.min.css" integrity="sha512-tN7Ec6zAFaVSG3TpNAKtk4DOHNpSwKHxxrsiw4GHKESGPs5njn/0sMCUMl2svV4wo4BK/rCP7juYz+zx+l6oeQ==" crossorigin="anonymous" referrerPolicy="no-referrer"/><pre id="2340f46d-3902-80ae-a584-e44a10832238" class="code"><code class="language-Plain Text" style="white-space:pre-wrap;word-break:break-all">
F0  47  &lt;DevID&gt;  4C  &lt;Cmd&gt;  &lt;Sub‑ID/Payload…&gt;  F7
│   │       │    │      │             │
│   │       │    │      │             → SysEx end
│   │       │    │      → command‑specific data
│   │       │    → Product ID (4C = LPD8 mk2)
│   │       → Device ID (00–7F or 7F = “All”)
│   → Akai manufacturer ID
→ SysEx start

</code></pre><hr id="2340f46d-3902-8045-afec-ec516d4e1dd8"/><h2 id="2340f46d-3902-80ec-84a7-caef7ac27090" class="">Pad Assignments &amp; Addresses</h2><table id="2340f46d-3902-80d1-b0e0-ee415bbe3d85" class="simple-table"><thead class="simple-table-header"><tr id="2340f46d-3902-806f-a8dc-e7c053837f28"><th id="Fx[c" class="simple-table-header-color simple-table-header">Pad #</th><th id="e[nr" class="simple-table-header-color simple-table-header">SysEx Index</th><th id="QV:^" class="simple-table-header-color simple-table-header">ID Byte</th><th id="^QJm" class="simple-table-header-color simple-table-header">Note Number</th><th id="d;Jo" class="simple-table-header-color simple-table-header">Default Channel</th></tr></thead><tbody><tr id="2340f46d-3902-80d1-b16f-ff5ad9378ff0"><td id="Fx[c" class="">1</td><td id="e[nr" class="">0x06 (6)</td><td id="QV:^" class="">0x09</td><td id="^QJm" class="">36</td><td id="d;Jo" class="">Ch 1 (0x00)</td></tr><tr id="2340f46d-3902-80d9-8d59-e4947fd17e1e"><td id="Fx[c" class="">2</td><td id="e[nr" class="">0x07 (7)</td><td id="QV:^" class="">0x09</td><td id="^QJm" class="">37</td><td id="d;Jo" class="">Ch 1 (0x00)</td></tr><tr id="2340f46d-3902-8026-924c-d2f5562fbd6c"><td id="Fx[c" class="">3</td><td id="e[nr" class="">0x08 (8)</td><td id="QV:^" class="">0x09</td><td id="^QJm" class="">38</td><td id="d;Jo" class="">Ch 1 (0x00)</td></tr><tr id="2340f46d-3902-8055-887c-d17ac7cb25b5"><td id="Fx[c" class="">4</td><td id="e[nr" class="">0x09 (9)</td><td id="QV:^" class="">0x09</td><td id="^QJm" class="">39</td><td id="d;Jo" class="">Ch 1 (0x00)</td></tr><tr id="2340f46d-3902-80b9-9399-fa88b37775bf"><td id="Fx[c" class="">5</td><td id="e[nr" class="">0x0A (10)</td><td id="QV:^" class="">0x09</td><td id="^QJm" class="">40</td><td id="d;Jo" class="">Ch 1 (0x00)</td></tr><tr id="2340f46d-3902-8088-9185-f3a3bfdd36fc"><td id="Fx[c" class="">6</td><td id="e[nr" class="">0x0B (11)</td><td id="QV:^" class="">0x09</td><td id="^QJm" class="">41</td><td id="d;Jo" class="">Ch 1 (0x00)</td></tr><tr id="2340f46d-3902-80d9-aefc-f90246b06136"><td id="Fx[c" class="">7</td><td id="e[nr" class="">0x0C (12)</td><td id="QV:^" class="">0x09</td><td id="^QJm" class="">42</td><td id="d;Jo" class="">Ch 1 (0x00)</td></tr><tr id="2340f46d-3902-8013-a89b-c4bd3e464db8"><td id="Fx[c" class="">8</td><td id="e[nr" class="">0x0D (13)</td><td id="QV:^" class="">0x09</td><td id="^QJm" class="">43</td><td id="d;Jo" class="">Ch 1 (0x00)</td></tr></tbody></table><hr id="2340f46d-3902-80c0-8337-d3e2735bee76"/><h2 id="2340f46d-3902-8086-896b-c3bc8a931815" class="">All‑8‑Pad Color Examples</h2><h3 id="2340f46d-3902-8041-a5c5-c2d1f5720b45" class="">1) Static White (all pads)</h3><script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/prism.min.js" integrity="sha512-7Z9J3l1+EYfeaPKcGXu3MS/7T+w19WtKQY/n+xzmw4hZhJ9tyYmcUS+4QqAlzhicE5LAfMQSF3iFTK9bQdTxXg==" crossorigin="anonymous" referrerPolicy="no-referrer"></script><link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/themes/prism.min.css" integrity="sha512-tN7Ec6zAFaVSG3TpNAKtk4DOHNpSwKHxxrsiw4GHKESGPs5njn/0sMCUMl2svV4wo4BK/rCP7juYz+zx+l6oeQ==" crossorigin="anonymous" referrerPolicy="no-referrer"/><pre id="2340f46d-3902-80a2-93c2-d990ea2c9c55" class="code"><code class="language-Plain Text" style="white-space:pre-wrap;word-break:break-all">
F0 47 7F 4C 06 00 30
  00 7F 00 7F 00 7F   ← Pad 1 (white)
  00 7F 00 7F 00 7F   ← Pad 2
  00 7F 00 7F 00 7F   ← Pad 3
  00 7F 00 7F 00 7F   ← Pad 4
  00 7F 00 7F 00 7F   ← Pad 5
  00 7F 00 7F 00 7F   ← Pad 6
  00 7F 00 7F 00 7F   ← Pad 7
  00 7F 00 7F 00 7F   ← Pad 8
F7

</code></pre><p id="2340f46d-3902-804a-89c5-d5c3d39f50b3" class="">Each <code>(127,127,127)</code> splits into <code>[00 7F] [00 7F] [00 7F]</code>.</p><hr id="2340f46d-3902-8057-a01a-d61468add4d8"/><h3 id="2340f46d-3902-8031-825b-f23476b324dd" class="">2) Rainbow Across All Pads</h3><table id="2340f46d-3902-8052-9e65-d0d5e8b50545" class="simple-table"><thead class="simple-table-header"><tr id="2340f46d-3902-8081-bdf6-ca503d63d7fc"><th id="_\Kk" class="simple-table-header-color simple-table-header">Pad</th><th id="fD~L" class="simple-table-header-color simple-table-header">Color (R,G,B)</th><th id="FoT[" class="simple-table-header-color simple-table-header">Hex pairs (hi,lo)</th></tr></thead><tbody><tr id="2340f46d-3902-8024-8551-c09e9467e8c4"><td id="_\Kk" class="">1</td><td id="fD~L" class="">(127,  0,  0)</td><td id="FoT[" class="">[00 7F] [00 00] [00 00]</td></tr><tr id="2340f46d-3902-8037-8894-ce37b4528537"><td id="_\Kk" class="">2</td><td id="fD~L" class="">(127, 63,  0)</td><td id="FoT[" class="">[00 7F] [00 3F] [00 00]</td></tr><tr id="2340f46d-3902-80b6-bbd7-c4169e5c71ca"><td id="_\Kk" class="">3</td><td id="fD~L" class="">(127,127,  0)</td><td id="FoT[" class="">[00 7F] [00 7F] [00 00]</td></tr><tr id="2340f46d-3902-80d4-8460-e96306c37870"><td id="_\Kk" class="">4</td><td id="fD~L" class="">(  0,127,  0)</td><td id="FoT[" class="">[00 00] [00 7F] [00 00]</td></tr><tr id="2340f46d-3902-8050-9028-fdbdfaaad02f"><td id="_\Kk" class="">5</td><td id="fD~L" class="">(  0, 63,127)</td><td id="FoT[" class="">[00 00] [00 3F] [00 7F]</td></tr><tr id="2340f46d-3902-8038-af35-d0e484b94041"><td id="_\Kk" class="">6</td><td id="fD~L" class="">(  0,  0,127)</td><td id="FoT[" class="">[00 00] [00 00] [00 7F]</td></tr><tr id="2340f46d-3902-802f-ac96-d46882edf272"><td id="_\Kk" class="">7</td><td id="fD~L" class="">( 63,  0,127)</td><td id="FoT[" class="">[00 3F] [00 00] [00 7F]</td></tr><tr id="2340f46d-3902-80fe-880d-f9c802e1668d"><td id="_\Kk" class="">8</td><td id="fD~L" class="">(127,  0, 63)</td><td id="FoT[" class="">[00 7F] [00 00] [00 3F]</td></tr></tbody></table><p id="2340f46d-3902-80ba-bfc6-e8d3c7d3b360" class="">Full message:</p><script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/prism.min.js" integrity="sha512-7Z9J3l1+EYfeaPKcGXu3MS/7T+w19WtKQY/n+xzmw4hZhJ9tyYmcUS+4QqAlzhicE5LAfMQSF3iFTK9bQdTxXg==" crossorigin="anonymous" referrerPolicy="no-referrer"></script><link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/themes/prism.min.css" integrity="sha512-tN7Ec6zAFaVSG3TpNAKtk4DOHNpSwKHxxrsiw4GHKESGPs5njn/0sMCUMl2svV4wo4BK/rCP7juYz+zx+l6oeQ==" crossorigin="anonymous" referrerPolicy="no-referrer"/><pre id="2340f46d-3902-8032-a4b1-c469e7fd59e4" class="code"><code class="language-Plain Text" style="white-space:pre-wrap;word-break:break-all">
F0 47 7F 4C 06 00 30
  00 7F 00 00 00 00   ← Pad 1: red
  00 7F 00 3F 00 00   ← Pad 2: orange
  00 7F 00 7F 00 00   ← Pad 3: yellow
  00 00 00 7F 00 00   ← Pad 4: green
  00 00 00 3F 00 7F   ← Pad 5: teal
  00 00 00 00 00 7F   ← Pad 6: blue
  00 3F 00 00 00 7F   ← Pad 7: violet
  00 7F 00 00 00 3F   ← Pad 8: magenta
F7

</code></pre><hr id="2340f46d-3902-808a-903e-dc0481c8a453"/><h3 id="2340f46d-3902-8071-8020-e6e647bb7d12" class="">3) Alternate On/Off Pattern</h3><script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/prism.min.js" integrity="sha512-7Z9J3l1+EYfeaPKcGXu3MS/7T+w19WtKQY/n+xzmw4hZhJ9tyYmcUS+4QqAlzhicE5LAfMQSF3iFTK9bQdTxXg==" crossorigin="anonymous" referrerPolicy="no-referrer"></script><link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/themes/prism.min.css" integrity="sha512-tN7Ec6zAFaVSG3TpNAKtk4DOHNpSwKHxxrsiw4GHKESGPs5njn/0sMCUMl2svV4wo4BK/rCP7juYz+zx+l6oeQ==" crossorigin="anonymous" referrerPolicy="no-referrer"/><pre id="2340f46d-3902-806a-a90d-eb77028dfe9e" class="code"><code class="language-Plain Text" style="white-space:pre-wrap;word-break:break-all">
F0 47 7F 4C 06 00 30
  00 7F 00 7F 00 7F   ← Pad 1: white
  00 00 00 00 00 00   ← Pad 2: off
  00 7F 00 7F 00 7F   ← Pad 3: white
  00 00 00 00 00 00   ← Pad 4: off
  00 7F 00 7F 00 7F   ← Pad 5: white
  00 00 00 00 00 00   ← Pad 6: off
  00 7F 00 7F 00 7F   ← Pad 7: white
  00 00 00 00 00 00   ← Pad 8: off
F7

</code></pre><table id="2340f46d-3902-80c5-afa9-e9692cca875b" class="simple-table"><thead class="simple-table-header"><tr id="2340f46d-3902-80f5-9c15-d727e85007aa"><th id=";^Gn" class="simple-table-header-color simple-table-header"><strong>Function</strong></th><th id="&gt;|dY" class="simple-table-header-color simple-table-header"><strong>Cmd Byte</strong></th><th id="EiNn" class="simple-table-header-color simple-table-header"><strong>Sub‑ID / Header</strong></th><th id="Xgf;" class="simple-table-header-color simple-table-header"><strong>Payload</strong></th><th id="d|vR" class="simple-table-header-color simple-table-header"><strong>Description</strong></th></tr></thead><tbody><tr id="2340f46d-3902-8081-9f7a-fcface63e7f7"><td id=";^Gn" class="">Get Program</td><td id="&gt;|dY" class="">0x03</td><td id="EiNn" class="">00 01 &lt;Prog#&gt;</td><td id="Xgf;" class="">—</td><td id="d|vR" class="">Request the current Program (1 or 2) from the unit.</td></tr><tr id="2340f46d-3902-80cc-892a-f0e0b33dcad8"><td id=";^Gn" class="">Program Data Response</td><td id="&gt;|dY" class="">0x03</td><td id="EiNn" class="">01 …</td><td id="Xgf;" class="">128‑byte block starting with 0x29 (see “Program Data Format” below)</td><td id="d|vR" class="">The unit replies with the full Program settings (pads, knobs assignments, colours, etc.).</td></tr><tr id="2340f46d-3902-8080-893e-e01d11770ddd"><td id=";^Gn" class="">Send Program</td><td id="&gt;|dY" class="">0x01</td><td id="EiNn" class="">01 29 …</td><td id="Xgf;" class="">128‑byte block (0x29 + 15 entries of 8‑byte pad/knob configs + end)</td><td id="d|vR" class="">Overwrites Program 1 on the unit with your custom pad‑knob mappings, ranges, modes, colours, etc.</td></tr><tr id="2340f46d-3902-8039-97aa-e20eec038370"><td id=";^Gn" class=""><strong>Pad LED Color Update</strong></td><td id="&gt;|dY" class=""><strong>0x06</strong></td><td id="EiNn" class=""><strong>00 30</strong></td><td id="Xgf;" class=""><strong>24 bytes: [R₁_hi,R₁_lo,G₁_hi,G₁_lo,B₁_hi,B₁_lo] … ×8 pads</strong></td><td id="d|vR" class="">Set the 8 RGB pad LEDs. Each channel = 0–127, split into two 7‑bit bytes.</td></tr><tr id="2340f46d-3902-80fd-88d7-c877f0fe0ce6"><td id=";^Gn" class="">Set Global Channel</td><td id="&gt;|dY" class="">0x01</td><td id="EiNn" class="">01 29 … 0E 00 01 00</td><td id="Xgf;" class="">…</td><td id="d|vR" class="">Change the MIDI channel for all pads/knobs. 0E byte = channel (0–0F).</td></tr><tr id="2340f46d-3902-80be-9c0e-debd0265842f"><td id=";^Gn" class="">Pad Mode (Momentary/Toggle)</td><td id="&gt;|dY" class="">0x01</td><td id="EiNn" class="">01 29 … 0E 00 01 01</td><td id="Xgf;" class="">…</td><td id="d|vR" class="">Flip between momentary (0) and toggle (1) pad behaviour.</td></tr><tr id="2340f46d-3902-809d-9040-d69b2b6e8d2f"><td id=";^Gn" class="">Pressure Message Mode</td><td id="&gt;|dY" class="">0x01</td><td id="EiNn" class="">01 29 … 0E 01 01 01</td><td id="Xgf;" class="">…</td><td id="d|vR" class="">0x00=off, 0x01=Channel Pressure, 0x02=Polyphonic Pressure.</td></tr><tr id="2340f46d-3902-80a2-80a1-ee87e550b728"><td id=";^Gn" class="">Full‑Level Velocity</td><td id="&gt;|dY" class="">0x01</td><td id="EiNn" class="">01 29 … 0E 02 00 01</td><td id="Xgf;" class="">…</td><td id="d|vR" class="">Enable (1) / disable (0) full‑level velocity output on pads.</td></tr><tr id="2340f46d-3902-80f5-a331-dfa73663c4a7"><td id=";^Gn" class=""><strong>Reset to Default</strong></td><td id="&gt;|dY" class=""><strong>0x01</strong></td><td id="EiNn" class=""><strong>01 29 01 00 00</strong></td><td id="Xgf;" class=""><strong>… default 0x24/0x25/0x26/0x27/0x28/0x29 blocks …</strong></td><td id="d|vR" class="">Restores all pad/knob mappings, ranges, colours, modes back to factory defaults.</td></tr></tbody></table><h3 id="2340f46d-3902-803f-982a-dbf64f3e33c7" class="">Program Data Format (0x29 block)</h3><p id="2340f46d-3902-80ed-b2e8-e77cf1bdbc3e" class="">Each pad/knob entry in the Send‑Program message uses this 8‑byte structure:</p><script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/prism.min.js" integrity="sha512-7Z9J3l1+EYfeaPKcGXu3MS/7T+w19WtKQY/n+xzmw4hZhJ9tyYmcUS+4QqAlzhicE5LAfMQSF3iFTK9bQdTxXg==" crossorigin="anonymous" referrerPolicy="no-referrer"></script><link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/themes/prism.min.css" integrity="sha512-tN7Ec6zAFaVSG3TpNAKtk4DOHNpSwKHxxrsiw4GHKESGPs5njn/0sMCUMl2svV4wo4BK/rCP7juYz+zx+l6oeQ==" crossorigin="anonymous" referrerPolicy="no-referrer"/><pre id="2340f46d-3902-8087-ace9-ca52cc9797fa" class="code"><code class="language-Plain Text" style="white-space:pre-wrap;word-break:break-all">
0x29
  &lt;Index&gt;        ; 00=K1, 01=K2, …, 06=Pad1, … 0D=Pad8
  &lt;ID&gt;           ; 0C=Knob, 09=Pad
  &lt;Chan&gt;         ; MIDI channel (0–0x0F)
  &lt;PC/Note&gt;      ; CC number for knobs, Note number for pads
  &lt;Min&gt; &lt;Max&gt;    ; Min/max value (0–127) for knobs
  &lt;Flags&gt;        ; bit‑flags: momentary/toggle, pressure on/off, full‑level, etc.

</code></pre></div></article><span class="sans" style="font-size:14px;padding-top:2em"></span></body></html>

## Sources

1. [Akai LPD8 MK2 Mapping (VirtualDJ Forum)](https://www.virtualdj.com/forums/249938/Wishes_and_new_features/Mapping__Akai_LPD8_MK2.html) — with thanks to DJdad for the clues  
2. [MPC Forums Discussion](https://www.mpc-forums.com/viewtopic.php?f=37&t=215728)
