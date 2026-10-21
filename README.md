
# 🌐 Google Translate Language Switcher

A responsive website language switcher built with HTML, CSS, and JavaScript using the Google Translate Website Translator. This project allows users to switch website content between **English and Russian** using fixed-position buttons.

## 📌 Project Overview

The Google Translate Language Switcher provides a simple way to add multilingual functionality to an existing website. It includes custom language buttons, saves the selected language in browser local storage, attempts to restore the language on page reload, and hides several elements of the default Google Translate interface.

## ✨ Features

- 🌍 **Multiple Languages:** Supports English and Russian.
- 🔄 **Instant Language Switching:** Changes the selected language through Google Translate.
- 💾 **Language Persistence:** Saves the selected language in `localStorage`.
- 🔁 **Automatic Language Restoration:** Attempts to restore the saved language when the page reloads.
- 📱 **Responsive Design:** Adapts the language switcher for mobile screens.
- 📍 **Fixed Positioning:** Keeps language buttons visible in a fixed position.
- 🎨 **Custom Styling:** Includes hover effects, rounded buttons, and box shadows.
- 🚫 **Toolbar Hiding:** Attempts to hide Google Translate's toolbar, tooltips, and feedback elements.

## 🛠️ Technologies Used

- HTML5
- CSS3
- JavaScript
- Google Translate Website Translator
- Browser `localStorage` API

## 📂 Project Structure

```text
google-translate-language-switcher/
│
├── index.html       # Website structure and language buttons
├── style.css        # Language switcher styling and responsive layout
├── script.js        # Google Translate initialization and language logic
└── README.md        # Project documentation
```

## 🚀 Installation and Setup

### Step 1: Clone the Repository

Replace `YOUR-USERNAME` with your GitHub username.

```bash
git clone https://github.com/YOUR-USERNAME/google-translate-language-switcher.git
```

### Step 2: Open the Project

```bash
cd google-translate-language-switcher
```

Open the project folder in Visual Studio Code.

### Step 3: Run the Project

Open `index.html` in your browser or use a local development server.

An active internet connection is required to load Google Translate.

## 💻 Source Code

### 1. HTML — `index.html`

The HTML file creates the website structure, loads the CSS and JavaScript files, and displays the English and Russian language buttons.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Google Translate Language Switcher</title>

    <link rel="stylesheet" href="style.css">
</head>
<body>

    <!-- Language Switcher -->
    <div class="language-switcher">
        <div class="lang-switch">

            <button type="button" onclick="setLang('en')">
                English
            </button>

            <button type="button" onclick="setLang('ru')">
                Russian
            </button>

        </div>

        <!-- Google Translate Element -->
        <div id="google_translate_element"></div>
    </div>

    <!-- Example Website Content -->
    <main class="page-content">
        <h1>Welcome to Our Website</h1>

        <p>
            This website supports English and Russian languages.
            Use the language buttons to change the website language.
        </p>
    </main>

    <!-- Custom JavaScript -->
    <script src="script.js"></script>

    <!-- Google Translate Script -->
    <script src="https://translate.google.com/translate_a/element.js?cb=googleTranslateElementInit"></script>

</body>
</html>
```

**How it works:**

- `language-switcher` contains the custom language buttons.
- `setLang('en')` selects English.
- `setLang('ru')` selects Russian.
- `google_translate_element` is the element used to initialize Google Translate, even though it is hidden.
- The external Google script loads the translation widget.

### 2. CSS — `style.css`

The CSS file controls the appearance, positioning, hover effects, and responsive behavior of the language switcher.

```css
/* Fixed Language Buttons */
.language-switcher {
    position: fixed;
    right: 20px;
    bottom: 229px;
    z-index: 999999;

    background: #ffffff;
    padding: 8px;
    border-radius: 8px;

    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.15);
}

/* Button Layout */
.lang-switch {
    display: flex;
    align-items: center;
    flex-direction: column;
    gap: 8px;
}

/* Button Styling */
.lang-switch button {
    padding: 7px 14px;
    border: 1px solid #ddd;
    background: #fff;
    color: #333;
    border-radius: 5px;

    cursor: pointer;
    font-size: 14px;

    transition: all 0.3s ease;
}

/* Hover Effect */
.lang-switch button:hover {
    background: #4d2d7e;
    color: #fff;
    border-color: #4d2d7e;
}

/* Hide Google Translate Element */
#google_translate_element {
    display: none !important;
}

/* Hide Google Translate Toolbar */
.goog-te-banner-frame,
.goog-te-banner-frame.skiptranslate {
    display: none !important;
}

/* Reset Page Position */
body {
    top: 0 !important;
}

/* Hide Translation Tooltip */
.goog-tooltip,
.goog-tooltip:hover,
.goog-text-highlight {
    display: none !important;
    background: transparent !important;
    box-shadow: none !important;
    border: none !important;
}

/* Hide Google Translate Feedback Elements */
.VIpgJd-ZVi9od-aZ2wEe-wOHMyf,
.VIpgJd-ZVi9od-ORHb-OEVmcd,
.VIpgJd-ZVi9od-aZ2wEe {
    display: none !important;
}

/* Example Page Content */
.page-content {
    padding: 50px 20px;
    text-align: center;
}

/* Mobile Responsive Design */
@media (max-width: 767px) {

    .language-switcher {
        right: 10px;
        bottom: 198px;
        padding: 6px;
    }

    .lang-switch {
        gap: 5px;
    }

    .lang-switch button {
        padding: 6px 10px;
        font-size: 12px;
    }
}
```

**How it works:**

- `position: fixed` keeps the buttons in the same screen position while scrolling.
- `z-index: 999999` helps keep the switcher above other page elements.
- The hover styles change the button background and text color.
- The media query reduces spacing and button size on mobile devices.
- Google Translate interface elements are hidden using CSS selectors. Google may change these selectors, so toolbar hiding is not guaranteed.

### 3. JavaScript — `script.js`

The JavaScript file initializes Google Translate, changes languages, stores the selected language, restores saved preferences, and repeatedly attempts to hide the translation toolbar.

```javascript
/* =========================
   GOOGLE TRANSLATE INIT
========================= */

function googleTranslateElementInit() {

    new google.translate.TranslateElement({
        pageLanguage: 'en',
        includedLanguages: 'en,ru',
        autoDisplay: false
    }, 'google_translate_element');

}


/* =========================
   CHANGE LANGUAGE
========================= */

function setLang(lang) {

    // Save selected language
    localStorage.setItem('siteLang', lang);

    let attempts = 0;

    let checkGoogleTranslate = setInterval(function () {

        let combo = document.querySelector('.goog-te-combo');

        if (combo) {

            // Set selected language
            combo.value = lang;

            // Trigger Google Translate change
            combo.dispatchEvent(
                new Event('change', {
                    bubbles: true
                })
            );

            clearInterval(checkGoogleTranslate);

            removeGoogleBar();

        }

        attempts++;

        // Stop after 10 seconds
        if (attempts >= 50) {
            clearInterval(checkGoogleTranslate);
        }

    }, 200);

}


/* =========================
   RESTORE LANGUAGE
========================= */

window.addEventListener('load', function () {

    let savedLang = localStorage.getItem('siteLang');

    if (savedLang) {

        setTimeout(function () {
            setLang(savedLang);
        }, 1500);

    }

});


/* =========================
   REMOVE GOOGLE BAR
========================= */

function removeGoogleBar() {

    let frame = document.querySelector('.goog-te-banner-frame');

    if (frame) {
        frame.style.display = 'none';
    }

    document.body.style.top = '0px';
    document.documentElement.style.top = '0px';

}


/* =========================
   KEEP GOOGLE BAR HIDDEN
========================= */

setInterval(function () {
    removeGoogleBar();
}, 500);
```

**How it works:**

1. `googleTranslateElementInit()` initializes the translation widget with English as the source language and Russian as an additional language.
2. `setLang(lang)` stores the selected language and searches for Google's language dropdown.
3. The script updates `.goog-te-combo` and dispatches a change event to request translation.
4. The `load` event checks whether a previously selected language exists in `localStorage`.
5. `removeGoogleBar()` attempts to hide the toolbar and reset the page position.
6. The interval retries toolbar hiding because Google Translate may insert interface elements after the page loads.

## ⚙️ Configuration

### Change the Available Languages

Update the `includedLanguages` property in `script.js`.

```javascript
new google.translate.TranslateElement({
    pageLanguage: 'en',
    includedLanguages: 'en,ru',
    autoDisplay: false
}, 'google_translate_element');
```

For example, `en,ru` enables English and Russian in the widget. If you add another language, also add a corresponding button that calls `setLang()` with that language's code.

### Change Button Position

Update these CSS properties:

```css
.language-switcher {
    right: 20px;
    bottom: 229px;
}
```

Change `right` and `bottom` to position the switcher where you want it.

### Change Button Colors

Update the hover background color:

```css
.lang-switch button:hover {
    background: #4d2d7e;
    color: #fff;
    border-color: #4d2d7e;
}
```

## 🔍 Troubleshooting

**Language buttons do not work**
- Check your internet connection.
- Confirm that the Google Translate script loads successfully.
- Check the browser console for JavaScript errors.
- Confirm that `.goog-te-combo` appears after Google Translate initializes.

**Saved language does not restore**
- Confirm that browser local storage is enabled.
- Open the website using the same origin and browser.
- Check whether Google Translate has loaded before the restoration attempt.

**Google Translate toolbar remains visible**
- Google may change its internal CSS classes and interface.
- The supplied CSS and JavaScript use best-effort hiding techniques.
- Avoid relying on these selectors as a guaranteed way to remove Google's toolbar.


