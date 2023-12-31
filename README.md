# FAQ

## 💻 Settings disappeared after the upgrade to v8

Using Firefox and you've lost all FoxyProxy settings?

FoxyProxy Basic 8.0 was first released in Sep 2023 as a trial run since it had fewer users (26k on Chrome & Firefox).
We waited for 2 months for any feedback & bug reports before releasing FoxyProxy Standard.
Unfortunately, we didn't get any bug report about the data migration sync issue, otherwsie we would have fixed it before releasing the standard version.
FoxyProxy 8.2 went online on Dec 6th.
Due to a bug in version 8.2, previous settings of some users were not migrated after the upgrade.
Versions 8.3-8.6 created with fixes for the bugs immediately, but due to the AMO approval waiting time, version 8.6 came online on Dec 12th.

Previous settings were not deleted and are recoverable. The following options are available if FoxyProxy updated from 7.* and you have encountered the update bug.

<details>
  <summary><b>1. Retrieve Settings and Keep version 8.2</b></summary>

From [this comment](https://github.com/foxyproxy/browser-extension/issues/45#issuecomment-1838719332):

### Look for old data

1. Go to the FoxyProxy Options page
2. Open the Dev Tools (F12)
3. Go to the Console tab
4. Type the following and hit ENTER


### With Sync

```js
  browser.storage.sync.get().then(console.log)
```

If above has some data, then in the Console tab, type the following and hit ENTER

```js
browser.storage.sync.get().then(pref => {
  const data = JSON.stringify(pref, null, 2);
  const blob = new Blob([data], {type: 'application/json'});
  browser.downloads.download({
    url: URL.createObjectURL(blob),
    filename: 'FoxyProxy_sync.json',
    saveAs: true,
    conflictAction: 'uniquify'
  })
  .catch(() => {});
});
```

### Without Sync

```js
  browser.storage.local.get().then(console.log)
```

If above has some data, then in the Console tab, type the following and hit ENTER

```js
browser.storage.local.get().then(pref => {
  const data = JSON.stringify(pref, null, 2);
  const blob = new Blob([data], {type: 'application/json'});
  browser.downloads.download({
    url: URL.createObjectURL(blob),
    filename: 'FoxyProxy_local.json',
    saveAs: true,
    conflictAction: 'uniquify'
  })
  .catch(() => {});
});
```
### Import data

1. Go to **Import Tab -> Import from older versions**
2. Import the `FoxyProxy_sync.json` or `FoxyProxy_local.json` file that you have saved
3. Click SAVE to save the data
</details>

<details>
  <summary><b>2. Downgrade to 7.*</b></summary>

Downgrade may retrieve old settings.

1. Download 7.5.1 (or older) from https://addons.mozilla.org/firefox/addon/foxyproxy-standard/versions/
2. Click the file; firefox will ask you to install that addon. Confirm
3. Go to Firefox settings, addons (about:addons), FoxyProxy, check that it shows version 7.*
4. **Important**: On that same page, set "Allow automatic updates" to off

The settings bug is expected to be fixed in the latest release.
Check [About](https://foxyproxy.github.io/browser-extension/src/content/about.html) for more information.

</details>




## 📱 Firefox for Android

[Downgrade instructions](https://github.com/foxyproxy/browser-extension/issues/107)

[Beta instructions](https://github.com/foxyproxy/browser-extension#beta-installation-guide)

---

# ![FoxyProxy](/src/image/icon.svg) FoxyProxy Browser Extension

[![license](https://img.shields.io/github/license/foxyproxy/browser-extension.svg)](https://github.com/foxyproxy/browser-extension/blob/master/LICENSE)
![GitHub repo size](https://img.shields.io/github/repo-size/foxyproxy/browser-extension?logo=github&logoColor=fff)
![ECMAScript](https://img.shields.io/badge/ECMAScript_2022_(ES13)-blue?style=plastic&logo=javascript)




[About](https://foxyproxy.github.io/browser-extension/src/content/about.html) | [Help](https://foxyproxy.github.io/browser-extension/src/content/help.html) | [Issues](https://github.com/foxyproxy/browser-extension/issues)

After some years of stability, FoxyProxy has been updated to support Manifest Version 3 which is required by Chrome in order for extensions to be compatible with Chrome in 2024. We took advantage of this forced update to implement many feature requests and other changes that were requested over the years.

FoxyProxy has been owned and developed consistently by the same team since 2006.

The repository has the source code for version 8.0+  for *Firefox*, *Chrome*, and other Chromium-based browsers like *Chromium*, *Brave* and *Edge*. Source code for [older versions](https://github.com/foxyproxy/firefox-extension).

## Permissions

These justifications were provided to Google and Mozilla

- **downloads**: Required to export the extension settings to a file. Users can import that file to other Chrome/Firefox instances, or share it with colleagues, in order to keep the same settings. It can also be backed up and used later.
- **proxy**: The core function of the extension is to allow users to set the proxy server used by the browser.
- **storage**: Required to store proxy server settings (hostname, port, username, and which proxy server is enabled by the user).
- **tabs**: Required so that users can set separate proxies to use per tab. It is also needed for "QuickAdd" to quickly add a URL pattern that applies to the current/active tab. It is also used to open a URL to getfoxyproxy.org where there is online help.
- **webRequest**: Required to authenticate with proxy servers via webRequest.onAuthRequired
- **webRequestAuthProvider**: Required to authenticate with proxies servers via webRequest.onAuthRequired
- **browsingData**: Required so the extension can delete cookies, indexedDB, and localStorage when requested by the user on the Options page (*Delete Browsing Data* button)
- **privacy**: Required so the extension can call browser.privacy.network.webRTCIPHandlingPolicy to turn on/off webRTC in Chrome (*Limit WebRTC* checkbox in Options page)
- **host permission**: "<all_urls>" permissions is required in order to supply credentials for [Proxy authorization](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/webRequest/onAuthRequired#proxy_authorization)

No remote code is used in this extension.

### Why is there a crypto library in [lib](https://github.com/foxyproxy/browser-extension/tree/main/src/lib)?

The crypto library was already included in [FoxyProxy 3.x](https://github.com/foxyproxy/Foxyproxy_Chrome/blob/f1bca1c50dfa30908c79a9ea477f31eda2abacf4/app/scripts/stored-credentials.js#L4) to encrypt user credentials. It is needed to migrate encrypted settings from the old version (which had no updates for many years, as you mentioned) to 2023. It is not used to encrypt anything; only to decrypt old data when upgrading from version 3.x -> 8.x.

The library is not used in Firefox and will be removed once users migrate to v8+.

## Screenshots

### Dark Theme

<img src="/screenshots/8.1/dark-theme/popup.jpg" width="200" alt=""> <img src="/screenshots/8.1/dark-theme/options-tab.jpg" width="200" alt="">
<img src="/screenshots/8.1/dark-theme/proxies-tab.jpg" width="200" alt=""> <img src="/screenshots/8.1/dark-theme/import-tab.jpg" width="200" alt="">
<img src="/screenshots/8.1/dark-theme/pattern-tester-tab.jpg" width="200" alt=""> <img src="/screenshots/8.1/dark-theme/log-tab.jpg" width="200" alt="">

### Light Theme

<img src="/screenshots/8.1/light-theme/popup.jpg" width="200" alt=""> <img src="/screenshots/8.1/light-theme/options-tab.jpg" width="200" alt="">
<img src="/screenshots/8.1/light-theme/proxies-tab.jpg" width="200" alt=""> <img src="/screenshots/8.1/light-theme/import-tab.jpg" width="200" alt="">
<img src="/screenshots/8.1/light-theme/pattern-tester-tab.jpg" width="200" alt=""> <img src="/screenshots/8.1/light-theme/log-tab.jpg" width="200" alt="">

## Releases

<table>
  <thead>
    <tr>
      <th></th>
      <th>Chrome</th>
      <th>Firefox</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Standard</td>
      <td>
        <a href="https://chromewebstore.google.com/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp"><img src="https://img.shields.io/chrome-web-store/v/gcknhkkoolaabfmlnjonogaaifnjlfnp?logo=googlechrome&logoColor=fff&label=Chrome%20108%2B" alt=""></a><br>
        <a href="https://chromewebstore.google.com/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp"><img src="https://img.shields.io/chrome-web-store/users/gcknhkkoolaabfmlnjonogaaifnjlfnp" alt=""></a>
      </td>
      <td>
        <a href="https://addons.mozilla.org/firefox/addon/foxyproxy-standard/"><img src="https://img.shields.io/amo/v/foxyproxy-standard?logo=firefoxbrowser&logoColor=fff&label=Firefox%2093%2B" alt=""></a><br>
        <a href="https://addons.mozilla.org/firefox/addon/foxyproxy-standard/"><img src="https://img.shields.io/amo/users/foxyproxy-standard" alt=""></a>
        <a href="https://addons.mozilla.org/firefox/addon/foxyproxy-standard/"><img src="https://img.shields.io/amo/dw/foxyproxy-standard" alt=""></a>
      </td>
    </tr>
    <tr>
      <td>Basic</td>
      <td>
        <a href="https://chromewebstore.google.com/detail/foxyproxy-basic/dookpfaalaaappcdneeahomimbllocnb"><img src="https://img.shields.io/chrome-web-store/v/dookpfaalaaappcdneeahomimbllocnb?logo=googlechrome&logoColor=fff&label=Chrome%20108%2B" alt=""></a><br>
        <a href="https://chromewebstore.google.com/detail/foxyproxy-basic/dookpfaalaaappcdneeahomimbllocnb"><img src="https://img.shields.io/chrome-web-store/users/dookpfaalaaappcdneeahomimbllocnb" alt=""></a>
      </td>
      <td>
        <a href="https://addons.mozilla.org/firefox/addon/foxyproxy-basic/"><img src="https://img.shields.io/amo/v/foxyproxy-basic?logo=firefoxbrowser&logoColor=fff&label=Firefox%2093%2B" alt=""></a><br>
        <a href="https://addons.mozilla.org/firefox/addon/foxyproxy-basic/"></a><img src="https://img.shields.io/amo/users/foxyproxy-basic" alt="">
        <a href="https://addons.mozilla.org/firefox/addon/foxyproxy-basic/"></a><img src="https://img.shields.io/amo/dw/foxyproxy-basic" alt="">
      </td>
    </tr>
    <tr>
      <td>Source Code Beta</td>
      <td colspan="2"><a href="https://github.com/foxyproxy/browser-extension/tree/main/src"><img src="https://img.shields.io/badge/dynamic/json?&url=https%3A%2F%2Fraw.githubusercontent.com%2Ffoxyproxy%2Fbrowser-extension%2Fmain%2Fsrc%2Fmanifest-firefox.json&query=%24.version&logo=github&logoColor=fff&label=FoxyProxy%20Beta&color=f60&prefix=v" alt=""></a></td>
    </tr>
    <tr>
      <td>Source Code Release</td>
      <td colspan="2"><a href="https://github.com/foxyproxy/browser-extension/releases">Releases</a></td>
    </tr>
    <tr>
      <td>Source Code Old</td>
      <td><a href="https://github.com/foxyproxy/Foxyproxy_Chrome">3.0.7.1</a></td>
      <td><a href="https://github.com/foxyproxy/firefox-extension/">7.5.1</a></td>
    </tr>
  </tbody>
</table>


### Browser Minimum

<table>
  <thead>
    <tr>
      <th>Chrome</th>
      <th>Firefox</th>
      <th>Firefox for Android</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>version 108<br><i>(released 2022-11-29)</i></td>
      <td>version 93<br><i>(released 2021-10-05)</i></td>
      <td>version 113 (manifest)<br><i>(API minimum 102)</i></td>
    </tr>
  </tbody>
</table>


## Beta Installation Guide
- Backup your FoxyProxy settings
- Download repo *(or use `git`)*
  - browser-extension *(this page)* -> Code *(green button)* -> Download ZIP
  - Unzip the downloaded file

- **Chrome**
  - Rename `manifest-chrome.json` in `src` folder to `manifest.json`
  - Go to `chrome://extensions/`
  - Enable Developer Mode *(top right)*
  - Click "Load Unpacked"
  - Select above `manifest.json` *(or `src` folder)*

- **Firefox** *(Nightly/Beta/Developer Edition)*
  - Rename `manifest-firefox.json` in `src` folder to `manifest.json`
  - Go to `about:debugging#/runtime/this-firefox`
  - Click "Load Temporary Add-on..."
  - Select above `manifest.json`

- **Firefox for Android**
  - Install Firefox Nightly
    - [Firefox Nightly for Developers](https://play.google.com/store/apps/details?id=org.mozilla.fenix&hl=en&gl=US)
  - Enable Debug Menu
    - Go to: menu -> Settings ->  About Firefox Nightly
    - Tap a few times on the Firefox icon to enable debug menu
    - Go to: `about:config`
    - Find `xpinstall.signatures.required`
    - Toggle to `false`
  - Install Beta version
    - Prepare `src` for Firefox as mentioned above
    - Create a zip file from the content of `src`
    - Make the file available to the Android device
  - Install add-on from file
    - Go to: menu -> Settings -> Advanced -> Install add-on from file

## Building for Distribution

### With [grunt](https://gruntjs.com/getting-started):

- Install grunt locally:
    - `npm i -D grunt-cli`
- Run one of:
  - `grunt --target=chrome-standard`<br>
  - `grunt --target=chrome-basic`<br>
  - `grunt --target=firefox-standard`<br>
  - `grunt --target=firefox-basic`

The target is built in `foxyproxy-XXX-YYY.zip`; e.g. `foxyproxy-chrome-standard.zip`.

### Without grunt:

- copy the appropriate manifest-xxx.json file to manifest.json; e.g. `mv manifest-chrome.json manifest.json`
- zip the `src` directory into the top of an archive. The `src/` directory should **not** be in the zip archive.
