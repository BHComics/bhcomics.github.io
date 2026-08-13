---
layout: default
title: Download
---

<div class="page-content">

<span class="kicker">The Book</span>
<h1>📚 Black Hole Comics</h1>

<p><strong>Available from 22 August 2026 — get your free copy here!</strong></p>

<div class="countdown" id="launch-countdown">
  <div class="countdown-unit"><span class="num" id="cd-days">–</span><span class="unit-label">Days</span></div>
  <div class="countdown-unit"><span class="num" id="cd-hours">–</span><span class="unit-label">Hours</span></div>
  <div class="countdown-unit"><span class="num" id="cd-mins">–</span><span class="unit-label">Minutes</span></div>
  <div class="countdown-unit"><span class="num" id="cd-secs">–</span><span class="unit-label">Seconds</span></div>
</div>
<p class="countdown-caption">Until the book launch at <a href="/events/">Comic Garden, Copenhagen</a> — 22 August 2026, 17:00 CEST.</p>
<p class="countdown-done" id="launch-done">🎉 The book has launched — get your free copy!</p>

<script>
(function () {
  var target = new Date('2026-08-22T17:00:00+02:00').getTime();
  var el = {
    days: document.getElementById('cd-days'),
    hours: document.getElementById('cd-hours'),
    mins: document.getElementById('cd-mins'),
    secs: document.getElementById('cd-secs')
  };
  var wrap = document.getElementById('launch-countdown');
  var caption = document.querySelector('.countdown-caption');
  var done = document.getElementById('launch-done');

  function tick() {
    var diff = target - Date.now();
    if (diff <= 0) {
      wrap.style.display = 'none';
      caption.style.display = 'none';
      done.style.display = 'block';
      clearInterval(timer);
      return;
    }
    var s = Math.floor(diff / 1000);
    el.days.textContent = Math.floor(s / 86400);
    el.hours.textContent = String(Math.floor((s % 86400) / 3600)).padStart(2, '0');
    el.mins.textContent = String(Math.floor((s % 3600) / 60)).padStart(2, '0');
    el.secs.textContent = String(s % 60).padStart(2, '0');
  }

  tick();
  var timer = setInterval(tick, 1000);
})();
</script>

</div>
