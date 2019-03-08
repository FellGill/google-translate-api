# Google Translate API
Modul [Node.JS](https://nodejs.org) zur Nutzung der kostenlosen Website Google Translate.

[![GitHub release](https://img.shields.io/github/release/FellGill/google-translate-api.svg?style=flat)](https://github.com/FellGill/google-translate-api/releases)
[![Dependencies](https://david-dm.org/FellGill/google-translate-api.svg)](https://david-dm.org/k3rn31p4nic/google-translate-api)
[![Known Vulnerabilities](https://snyk.io/test/github/FellGill/google-translate-api/badge.svg?targetFile=package.json)](https://snyk.io/test/github/FellGill/google-translate-api?targetFile=package.json)
[![license](https://img.shields.io/github/license/FellGill/google-translate-api.svg)](LICENSE)

### Feature Highlights
* Quellsprache automatisch erkennen
* Automatische Rechtschreibkorrekturen
* Automatische Sprachkorrektur
* Schnell und zuverlässig

## Inhaltsverzeichnis
* [Installation](#installation)
* [Verwendungszweck](#verwendungszweck)
* [Beispiele](#beispiele)
* [Credits, usw](#extras)

## Installation
```bash
# Stabile Version aus dem NPM repository
npm install --save @fellgill/google-translate-api

# Neueste Version aus dem GitHub repository
npm install --save fellgill/google-translate-api
```

## Verwendungszweck
```js
// Wenn Sie von npm aus installiert haben, führen Sie Folgendes aus:
const translate = require('@fellgill/google-translate-api');

// Wenn Sie von GitHub installiert haben, gehen Sie wie folgt vor:
const translate = require('@fellgill/google-translate-api');
```

#### Methode: `translate(text, options)`
```js
translate(text, options).then(console.log).catch(console.error);
```
| Parameter | Art | Wahlweise | Standard | Beschreibung |
|-|-|-|-|-|
| `text` | `String` | Nein | - | Der zu übersetzende Text. |
| `options` | `Object` | - | - | Die Optionen zum Übersetzen. |
| `options.from` | `String` | Ja | `'auto'` | Der Name der sprache/ISO 639-1-Code, aus dem übersetzt werden soll. Wenn keine angegeben ist, wird die Quellsprache automatisch erkannt. |
| `options.to` | `String` | Ja | `'en'` | Der sprachenname/ISO 639-1-Code, in den übersetzt werden soll. Wenn keine angegeben ist, wird es ins Englische übersetzt. |
| `options.raw` | `Boolean` | Ja | `false` | Wenn `true`, wird die von Google Translate empfangene Rohausgabe zurückgegeben. |

#### Kehrt zurück: `Promise<Object>`
**Antwortobjekt:**

| Schlüssel | Art | Beschreibung |
|-|-|-|
| `text` | `String` | Der übersetzte Text. |
| `from` | `Object` | - |
| `from.language` | `Object` | - |
| `from.language.didYouMean` | `Booelan` | Ob die API eine Korrektur in der Quellsprache vorschlägt oder nicht. |
| `from.language.iso` | `String` | Der ISO 639-1-Code der Sprache, die die API im Text erkannt hat. |
| `from.text` | `Object` | - |
| `from.text.autoCorrected` | `Booelan` | Ob die API den ursprünglichen Text automatisch korrigiert hat oder nicht. |
| `from.text.value` | `String` | Der automatisch korrigierte Text oder der Text mit Korrekturvorschlägen. Wird nur zurückgegeben wenn `from.text.autoCorrected` oder `from.text.didYouMean` ist `true`. |
| `from.text.didYouMean` | `Booelan` | Ob die API Korrekturen am Text vorgeschlagen hat oder nicht |
| `raw` | `String` | Die rohe Antwort von Google Translate-Servern. Wird nur zurückgegeben wenn `options.raw` ist `true` in den Anforderungsoptionen. |


## Beispiele
#### Von der automatischen Spracherkennung zu Englisch:
```js
translate('Ich bin ein text', { to: 'en' }).then(res => {
  console.log(res.text); // OUTPUT: I am a text
}).catch(err => {
  console.error(err);
});
```

#### Von Englisch nach Französisch mit einem Tippfehler:
```js
translate('Ich bn ein text', { from: 'de', to: 'en' }).then(res => {
  console.log(res.text); // OUTPUT: I am a text
  console.log(res.from.autoCorrected); // OUTPUT: true
  console.log(res.from.text.value); // OUTPUT: Ich [bin] ein text
  console.log(res.from.text.didYouMean); // OUTPUT: false
}).catch(err => {
  console.error(err);
});
```

#### Manchmal wird Google Translate nicht automatisch korrigiert:
```js
translate('Ich bn ein text', { from: 'en', to: 'fr' }).then(res => {
  console.log(res.text); // OUTPUT: ''
  console.log(res.from.autoCorrected); // OUTPUT: false
  console.log(res.from.text.value); // OUTPUT: Ich [bin] ein text
  console.log(res.from.text.didYouMean); // OUTPUT: true
}).catch(err => {
  console.error(err);
});
```

## Extras

Forked from https://github.com/k3rn31p4nic/google-translate-api, translated and minimised by FellGill
