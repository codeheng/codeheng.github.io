---
comments: true
---

# Security

!!! abstract
    研究生方向是安全，虽然学了点，但是没完全学  („ಡωಡ„)~~

    记录一些研究生方向相关的知识！


<script> function updateTime() { var date = new Date(); var now = date.getTime(); var startDate = new Date("2022/01/03 09:10:00"); var start = startDate.getTime(); var diff = now - start; var y, d, h, m; y = Math.floor(diff / (365 * 24 * 3600 * 1000)); diff -= y * 365 * 24 * 3600 * 1000; d = Math.floor(diff / (24 * 3600 * 1000)); h = Math.floor(diff / (3600 * 1000) % 24); m = Math.floor(diff / (60 * 1000) % 60); if (y == 0) { document.getElementById("web-time").innerHTML = d + " 天 " + h + " 小时 " + m + " 分钟"; } else { document.getElementById("web-time").innerHTML = y + " 年 " + d + " 天 " + h + " 小时 " + m + " 分钟"; } setTimeout(updateTime, 1000 * 60); } updateTime(); function toggle_statistics() { var statistics = document.getElementById("statistics"); if (statistics.style.opacity == 0) { statistics.style.opacity = 1; } else { statistics.style.opacity = 0; } } </script>