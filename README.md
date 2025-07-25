
# Unsubinator - YouTube Mass Unsubscribe 🚫📺

Automatically unsubscribe from multiple YouTube channels in bulk — no extensions, no installations. Just copy-paste and go.

---

## 🔧 Features

- ✅ Clicks every "Unsubscribe" menu item one-by-one  
- ✅ Confirms each unsubscribe popup automatically  
- ✅ Optional: Scrolls and loads **all channels** before starting  
- ✅ Bookmarklet version for quick re-use

---

## 📦 How to Use

1. **Open** your browser and go to:  
   👉 [https://www.youtube.com/feed/channels](https://www.youtube.com/feed/channels)

2. **Scroll** down to load as many channels as you want  
   (or use the auto-scroll snippet below 👇)

3. **Open Developer Console**:  
   - Right click → Inspect → `Console` tab  
   - Or press `Ctrl + Shift + J` / `F12`

4. **Paste the script** below and press `Enter`

---

### ▶️ Main Script

```javascript
(async () => {
  const dropdownButtons = Array.from(document.querySelectorAll('.yt-spec-button-shape-next__secondary-icon'));
  console.log(`✅ Found ${dropdownButtons.length} menu buttons`);

  for (let i = 0; i < dropdownButtons.length; i++) {
    console.log(`➡️ Opening menu #${i + 1}`);
    dropdownButtons[i].click();

    let unsubscribeButton;
    try {
      unsubscribeButton = await new Promise((resolve, reject) => {
        const timeout = 5000;
        const start = Date.now();
        const check = setInterval(() => {
          const items = Array.from(document.querySelectorAll('tp-yt-paper-item'))
            .filter(item =>
              item.textContent.trim().toLowerCase().includes('unsubscribe') &&
              item.offsetParent !== null
            );
          if (items.length > 0) {
            clearInterval(check);
            resolve(items[0]);
          } else if (Date.now() - start > timeout) {
            clearInterval(check);
            reject(new Error('Unsubscribe button not found in time'));
          }
        }, 200);
      });

      unsubscribeButton.click();
      console.log(`✔️ Clicked Unsubscribe #${i + 1}`);

      const confirmButton = await new Promise((resolve, reject) => {
        const timeout = 5000;
        const start = Date.now();
        const check = setInterval(() => {
          const confirm = document.querySelector('.yt-spec-touch-feedback-shape__fill');
          if (confirm) {
            clearInterval(check);
            resolve(confirm);
          } else if (Date.now() - start > timeout) {
            clearInterval(check);
            reject(new Error('Confirmation button not found in time'));
          }
        }, 200);
      });

      confirmButton.click();
      console.log(`✔️ Confirmed Unsubscribe #${i + 1}`);

    } catch (err) {
      console.warn(`⚠️ Failed at index ${i + 1}: ${err.message}`);
    }

    await new Promise(resolve => setTimeout(resolve, 2000));
  }

  console.log('🎉 Finished unsubscribing all visible channels.');
})();
```

---

## 🔁 Auto-Scroll to Load More

Run this **before the main script** if your subscriptions list is long:

```javascript
let scrollInterval = setInterval(() => {
  window.scrollBy(0, 1000);
  if ((window.innerHeight + window.scrollY) >= document.body.offsetHeight - 500) {
    console.log("✅ Scrolled to bottom");
    clearInterval(scrollInterval);
  }
}, 300);
```

---

## 🔖 Bookmarklet (1-Click Unsubscribe)

1. Create a new bookmark.
2. Paste this as the URL:

```javascript
javascript:(async()=>{const b=Array.from(document.querySelectorAll(".yt-spec-button-shape-next__secondary-icon"));for(let i=0;i<b.length;i++){b[i].click();let u=await new Promise((r,j)=>{let t=Date.now();let c=setInterval(()=>{let d=Array.from(document.querySelectorAll("tp-yt-paper-item")).find(e=>e.textContent.toLowerCase().includes("unsubscribe")&&e.offsetParent!==null);if(d){clearInterval(c);r(d)}else if(Date.now()-t>5000){clearInterval(c);j("Timeout")}},200)});u.click();let f=await new Promise((r,j)=>{let t=Date.now();let c=setInterval(()=>{let d=document.querySelector(".yt-spec-touch-feedback-shape__fill");if(d){clearInterval(c);r(d)}else if(Date.now()-t>5000){clearInterval(c);j("Timeout")}},200)});f.click();await new Promise(r=>setTimeout(r,1500))}alert("Unsubscribe done!")})();
```

3. Visit YouTube's [Subscriptions page](https://www.youtube.com/feed/channels) and click the bookmark.

---

## ⚠️ Disclaimer

This script mimics regular user interaction and is provided **for educational purposes only**.  
You are responsible for any usage. Use with care.

> This tool does not store data, collect tokens, or use any external dependencies.

---

## 📄 License

MIT License.  
Free to use, fork, modify, and distribute.

---

## 🙌 Contributing

PRs and issues are welcome!

If you found this helpful, feel free to ⭐️ star the repo!
