---
layout: default
title: Link Buttons
---

<div class="wrap" id="button-container">
  <!-- Buttons get injected here -->
</div>

<script>
  function openInNewTab(url) {
    const win = window.open(url, '_blank');
    if (win) win.focus();
  }

  fetch('{{ site.baseurl }}/assets/data/url.txt')
    .then(response => response.text())
    .then(text => {
      const lines = text.trim().split('\n');
      const container = document.getElementById('button-container');
      let buttons = [];

      // Collect buttons from lines
      for (let i = 0; i < lines.length - 1; i += 2) {
        const label = lines[i].replace(/^---|---$/g, '').trim();
        const url = lines[i + 1].trim();
        buttons.push({ label, url });
      }

      // Render the first 4 as quadrants
      buttons.slice(0, 4).forEach((btn, index) => {
        const div = document.createElement('div');
        div.className = 'quart';
        div.innerText = btn.label;
        div.style.zIndex = 2;
        div.onclick = () => openInNewTab(btn.url);
        container.appendChild(div);
      });

    // Render the 5th (last) as center button
    if (buttons.length >= 5) {
      const centerBtn = buttons[4];
      const div = document.createElement('div');
      div.className = 'center';
      div.innerText = centerBtn.label;
      div.style.zIndex = 10; // ✅ Ensures it’s above all quadrants
      div.onclick = () => openInNewTab(centerBtn.url);
      container.appendChild(div); // ✅ Add last to ensure it's on top
    }
    })
    .catch(err => {
      document.getElementById('button-container').innerHTML =
        '<p style="color:red;">Error loading buttons</p>';
      console.error(err);
    });
</script>