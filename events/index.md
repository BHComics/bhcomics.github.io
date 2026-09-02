---
layout: default
title: Events
---

<section class="events">
  <div class="section-inner">
    <div class="section-head">
      <div>
        <span class="kicker">Project events</span>
        <h2>Where to find the book</h2>
      </div>
    </div>
    <div class="events-grid">
      <div class="event-card">
        <a href="https://the-center-of-gravity.com/features/black-hole-week-2026/" target="_blank" rel="noopener noreferrer" class="event-tag">Black Hole Week 2026</a>
        <span class="date">20 – 31 August</span>
        <h3>Story Posters on Display</h3>
        <p>Kongens Nytorv, Copenhagen — walk the full stories as large-format posters, open to the public all week.</p>

        <div class="photo-strip" tabindex="0" role="button" aria-label="See photos from Story Posters on Display"
             onclick="openLB('posters')" onkeydown="if(event.key==='Enter')openLB('posters')">
          <img class="cover-img" src="/images/events/posters/poster-16.jpg" alt="Black Hole comic posters on display at Kongens Nytorv">
          <div class="scrim"></div>
          <div class="see-photos">See photos <span class="count">16</span></div>
          <div class="stack-edge"></div>
        </div>

        <a href="https://visitcopenhagen.cruncho.co/en-GB/place/JdVPpj/BlackHoleWeek?origin=%2F%3F" target="_blank" class="link">Visit Copenhagen →</a>
      </div>
      <div class="event-card">
        <a href="https://the-center-of-gravity.com/features/black-hole-week-2026/" target="_blank" rel="noopener noreferrer" class="event-tag">Black Hole Week 2026</a>
        <span class="date">22 August · 17:00</span>
        <h3>Book Launch at Comic Garden</h3>
        <p>Join our premier in a panel discussion with the scientists and artists behind the book.</p>

        <div class="photo-strip" tabindex="0" role="button" aria-label="See photos from Book Launch at Comic Garden"
             onclick="openLB('launch')" onkeydown="if(event.key==='Enter')openLB('launch')">
          <img class="cover-img" src="/images/events/launch/launch-02.jpg" alt="Black Hole Comics book launch at Comic Garden">
          <div class="scrim"></div>
          <div class="see-photos">See photos <span class="count">12</span></div>
          <div class="stack-edge"></div>
        </div>

        <a href="https://comicgarden.dk" target="_blank" class="link">Comic Garden details →</a>
      </div>
    </div>
  </div>
</section>

<div class="lightbox" id="lightbox" aria-hidden="true">
  <div class="lightbox-inner">
    <div class="lb-frame" id="lb-frame">
      <button class="lb-close" onclick="closeLB()" aria-label="Close">✕</button>
      <button class="lb-nav prev" onclick="step(-1)" aria-label="Previous photo">‹</button>
      <button class="lb-nav next" onclick="step(1)" aria-label="Next photo">›</button>
      <!-- slides injected by JS -->
    </div>
    <div class="lb-meta">
      <span class="lb-caption" id="lb-caption"></span>
      <span class="lb-count" id="lb-count"></span>
    </div>
    <div class="lb-dots" id="lb-dots"></div>
  </div>
</div>

<script>
(function () {
  const GALLERIES = {
    posters: {
      count: 16, prefix: '/images/events/posters/poster-', pad: 2,
      alt: 'Photo from the Kongens Nytorv poster exhibition',
      captions: []
    },
    launch: {
      count: 12, prefix: '/images/events/launch/launch-', pad: 2,
      alt: 'Photo from the Comic Garden book launch',
      captions: []
    }
  };

  let current = null, index = 0, slideEls = [];
  const lb = document.getElementById('lightbox');
  const frame = document.getElementById('lb-frame');
  const captionEl = document.getElementById('lb-caption');
  const countEl = document.getElementById('lb-count');
  const dotsEl = document.getElementById('lb-dots');

  function srcFor(g, i) {
    return g.prefix + String(i + 1).padStart(g.pad, '0') + '.jpg';
  }

  // Build empty slide containers (no <img> yet — no network cost) for the gallery.
  function buildSlides(g) {
    frame.querySelectorAll('.lb-slide').forEach(n => n.remove());
    slideEls = [];
    for (let i = 0; i < g.count; i++) {
      const el = document.createElement('div');
      el.className = 'lb-slide';
      frame.appendChild(el);
      slideEls.push(el);
    }
  }

  // Lazily create the <img> for slide i the first time it's needed.
  function ensureSlide(i) {
    const g = GALLERIES[current];
    if (i < 0 || i >= g.count) return;
    const el = slideEls[i];
    if (!el || el.querySelector('img')) return;
    const img = document.createElement('img');
    img.src = srcFor(g, i);
    img.alt = `${g.alt} (${i + 1} of ${g.count})`;
    img.loading = 'eager';
    el.appendChild(img);
  }

  function render() {
    const g = GALLERIES[current];
    ensureSlide(index);
    slideEls.forEach((el, i) => el.classList.toggle('active', i === index));

    const cap = g.captions[index];
    captionEl.textContent = cap || '';
    captionEl.style.display = cap ? '' : 'none';
    countEl.textContent = `${index + 1} / ${g.count}`;

    dotsEl.innerHTML = '';
    for (let i = 0; i < g.count; i++) {
      const b = document.createElement('button');
      b.className = i === index ? 'active' : '';
      b.setAttribute('aria-label', `Photo ${i + 1}`);
      b.onclick = () => goTo(i);
      dotsEl.appendChild(b);
    }

    // Preload neighbors shortly after showing the active slide, not all at once.
    setTimeout(() => {
      ensureSlide(index + 1 >= g.count ? 0 : index + 1);
      ensureSlide(index - 1 < 0 ? g.count - 1 : index - 1);
    }, 150);
  }

  window.openLB = function (key) {
    current = key;
    index = 0;
    buildSlides(GALLERIES[key]);
    lb.classList.add('open');
    lb.setAttribute('aria-hidden', 'false');
    render();
  };
  window.closeLB = function () {
    lb.classList.remove('open');
    lb.setAttribute('aria-hidden', 'true');
  };
  window.step = function (d) {
    const n = GALLERIES[current].count;
    index = (index + d + n) % n;
    render();
  };
  window.goTo = function (i) {
    index = i;
    render();
  };

  document.addEventListener('keydown', e => {
    if (!lb.classList.contains('open')) return;
    if (e.key === 'Escape') closeLB();
    if (e.key === 'ArrowLeft') step(-1);
    if (e.key === 'ArrowRight') step(1);
  });
  lb.addEventListener('click', e => { if (e.target === lb) closeLB(); });

  let touchX = null;
  frame.addEventListener('touchstart', e => touchX = e.touches[0].clientX);
  frame.addEventListener('touchend', e => {
    if (touchX === null) return;
    const dx = e.changedTouches[0].clientX - touchX;
    if (Math.abs(dx) > 40) step(dx > 0 ? -1 : 1);
    touchX = null;
  });
})();
</script>
