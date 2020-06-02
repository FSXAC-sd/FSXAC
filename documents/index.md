---
layout: default
title: Documents
description: A collection of course notes and more

nav_active: /documents
permalink: /documents

use_category_instead_of_name: false
show_footer: true
show_navigation: true
---

This page contains academic related notes and documents. I try to fill-in as much content as I can from the courses I've taken. ⚠ Only use content from this page as reference material and always uphold academic integrity.

Please feel free to contact me if there are any mistakes. Alternatively, since this website is also open-source on GitHub, I'm always open to issues and pull requests. &#x1F44C;

<span id="searchFieldIcon">&#128270;&nbsp;</span><input type="text" id="searchField" onkeyup="searchFunc()" placeholder="Search...">

<style>
:root {
	--card-bg-color: #fff;
	--card-sep-color: #000a;
}

@media (prefers-color-scheme: dark) {
	:root {
		--card-bg-color: #000;
		--card-sep-color: #fff4;
	}
}

#searchField {
	border: none;
	background-color: transparent;
	border-bottom: 1px dotted var(--text-color);
	color: var(--text-color);
	margin-top: .8em;
}
#searchFieldIcon {
	font-size: 1.25em;
}

.card { width: 100%; margin-bottom: 1em; border: none; background: transparent; }
.card-header { background-color: transparent; border-bottom: none; }
.card-body { padding: 0; }
.card-body .list-group .list-group-item {
	border: none;
    padding: 0;
    white-space: nowrap;
    text-overflow: ellipsis;
	overflow: hidden;
	margin: 0;
	background: transparent;
}
.card-body .list-group .list-group-item .btn-entry {
	border: 1px solid var(--link-color);
	color: var(--link-color);
	margin-top: .1em;
	margin-bottom: .1em;
}

.card-body .list-group .list-group-item .btn-entry:hover {
	border-color: var(--link-hover-color);
	color: white;
	background-color: var(--link-hover-color);
}

.card-gutter-sizer { width: 0; }
@media screen and (min-width: 992px) {
    .card { width: 32%; }
    .card-gutter-sizer { width: 2%; }
}
@media screen and (min-width: 768px) and (max-width: 992px) {
	.card { width: 49%; }
    .card-gutter-sizer { width: 2%; }
}

.flag-draft {
	border-left: 2px solid gray;
	padding-left: 0.25em;
}
.flag-new {
	border-left: 2px solid green;
	padding-left: 0.25em;
}
.flag-other {
	border-left: 2px solid gold;
	padding-left: 0.25em;
}
</style>

{% for category in site.data.documents %}
<section>
<h2 class='mt-4'>{{ category.category }}</h2>
<div class="card-grid">
<div class="card-gutter-sizer"></div>
{% assign courses = category.courses %}
{% for course in courses %}
	{% if course.entries %}
	<div id="{{ course.course_num | replace: ' ', '-'}}" class="card p-0">
	<div class="card-header p-0">
		<small>{{ course.course_num | upcase }}</small>
		<h4>{{ course.course_name }}</h4>
		<!-- <small>Last updated {{ course.date | default: 'never' }}</small> -->
	</div>
	<div class="card-body">
		<ul class='list-group list-group-flush'>

		{% for entry in course.entries %}

		<li class='list-group-item'>
			{% if entry.group %}
				{{ entry.title }}
				{% for enum_entry in entry.group %}
					<a class="btn btn-xs btn-entry" href="{{ enum_entry.link }}">{{ enum_entry.enum }}</a>
				{% endfor %}
			{% else %}
				{% if entry.flag == 'draft' %}
					<a href="{{ entry.link }}" class="flag-draft" title="{{ entry.flag }} - {{ entry.title }}">{{ entry.title }}</a>
				{% elsif entry.flag == 'new' %}
					<a href="{{ entry.link }}" class="flag-new" title="{{ entry.flag }} - {{ entry.title }}">{{ entry.title }}</a>
				{% elsif entry.flag %}
					<a href="{{ entry.link }}" class="flag-other" title="{{ entry.flag }} - {{ entry.title }}">{{ entry.title }}</a>
				{% else %}
					<a href="{{ entry.link }}" title="{{ entry.title }}">{{ entry.title }}</a>
				{% endif %}
			{% endif %}
		</li>

		{% endfor %}

		</ul>
	</div>
	</div>
	{% endif %}
{% endfor %}
</div>
</section>
{% endfor %}

<script src="https://cdnjs.cloudflare.com/ajax/libs/masonry/4.2.2/masonry.pkgd.min.js" crossorigin="anonymous"></script>
<script>
$('.card-grid').masonry({
    itemSelector: '.card',
    gutter: '.card-gutter-sizer',
    percentPosition: true
});

function searchFunc() {
	let searchInput = document.getElementById('searchField');
	let searchVal = searchInput.value.toLowerCase();

	let allCards = document.getElementsByClassName('card');
	for (let i = 0; i < allCards.length; i++) {

		let cardHeader = allCards[i].getElementsByClassName('card-header')[0];

		if (cardHeader.innerHTML.toLowerCase().includes(searchVal)) {
			allCards[i].style.display = 'flex';
		} else {
			allCards[i].style.display = 'none';
		}
	}

	// check if card grid is empty
	let cardGrids = document.getElementsByClassName('card-grid');
	for (let i = 0; i < cardGrids.length; i++) {
		let cards = cardGrids[i].getElementsByClassName('card');
		let display = false;
		for (let j = 0; j < cards.length; j++) {
			if (cards[j].style.display !== 'none') {
				display = true;
			}
		}
		if (display) {
			cardGrids[i].previousElementSibling.style.display = 'block'
			cardGrids[i].style.display = 'block';
		} else {
			cardGrids[i].previousElementSibling.style.display = 'none'
			cardGrids[i].style.display = 'none';
		}
	}

	// Reload masonry layout
	$('.card-grid').masonry('layout');
}

$(document).ready(function() {
	// alert('hi');
	let regex = /\/documents\/?#\?(.+)/g;
	let url = window.location.href;
	let match = decodeURI(regex.exec(url)[1]);

	console.log(match);

	document.getElementById('searchField').value = match;
	searchFunc();
});
</script>