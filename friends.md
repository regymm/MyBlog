---
layout: page
title: Friends
permalink: /friends/
---

#### Today RNG blessed ...

<h3><a href="" id="friendname"></a></h3>

<p>Email me to have you added. </p>
<p>Don't F12, please!</p>

<!-- ordered randomly -->
<script>
var friendnamearray = [
	'iBug',
	'taoky',
	'Elsa Granger',
	'Zeka',
	'Totoro',
	'myl7',
	'loliw',
	'Catoverflow',
	'sirius',
	'LibreLiu',
	'Simba',
	'Zi-Yue Wang'
];
var friendurlarray = [
	'https://ibug.io',
	'https://www.taoky.moe',
	'https://elsagranger.com',
	'https://flag.zeka.cloud',
	'https://yyw.moe',
	'https://myl7.moe',
	'https://loliw.moe',
	'https://blog.c-j.dev',
	'https://sirius1242.github.io',
	'https://blog.libreliu.info',
	'http://www.simbaondiet.com',
	'http://home.ustc.edu.cn/~metrictensor'
];
var d = new Date();
var seed = d.getFullYear()*1000 + d.getMonth()*100 + d.getDay();
function randomm() {
	var x = Math.sin(seed++) * 10000;
	return x - Math.floor(x);
}
var r = Math.floor(randomm()*friendurlarray.length);
document.getElementById("friendname").innerHTML = friendnamearray[r];
document.getElementById("friendname").href = friendurlarray[r];
</script>
