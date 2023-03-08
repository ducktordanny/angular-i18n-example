# Angular i18n

## Internationalization vs Localization

### Internationalization

- Separating content for translation
- Update app to support bidirectional text (left2right or right2left)

### Localization

> building versions for different locales

#### Locale
- A region where people speak a language or language variant
- Measurements of time
- Numbers and currencies
- Translations of names

#### Locales format:

[Language ID]-[locale extension] e.g.: en-US, en-GB, fr-CA

## Installation & configuration

Install: `ng add @angular/localize`

Use `i18n` and `i18n-...` tags on text elements and attributes. For example:

```angular2html
<img src="cat.png" title="Alma" alt="A cute cat image" i18n-title i18n-alt i18n-src>
<p i18n>Cats are great...</p>
```

Find content for OTI (Opportunity To Internationalize):

```angular2html
<p>Expires: {{'01-01-2024' | date}}</p>
<p>Cat food: {{40 | currency}}</p>
```

Modify the `angular.json` file:

```json
{
  "projects": {
    "angular-i18n-example": {
      "i18n": {
        "sourceLocale": "en-US",
        "locales": {
          "hu-HU": "src/locale/messages.hu.xlf"
        }
      },
      "projectType": "application",
      "architect": {
        "build": {
          "options": {
            "localize": ["hu-HU"]
          }
        }
      } 
    }
  }
}
```

Create the translation file: `ng extract-i18n --output-path src/locale`. This will create the `src/locale/messages.xlf` file (`--output-file` to rename).

Duplicate it and name it to `src/locale/message.<lang>.xlf` (e.g. in our case `<lang>` is *hu*: `src/locale/message.hu.xlf`). In our new file `src/locale/message.hu.xlf` we can specify the translation with the `target` tag like so:

```xml
<trans-unit>
  <source>Cats are cute!</source>
  <target>A cicák aranyosak!</target>
  ...
</trans-unit>
```

Run `ng build --localize` to get all of our locales -> See `dist/angular-i18n-example` folder it'll have a `hu-HU` and a `en-US` build folders.

## Build time localization -> Why?

> In Angular's approach the translation is made when we build the app. So if we change the language we have to reload the page.

Why not changing the translation in real time?

- Can be a huge performance cost
- Each translation string will create a new binding
- To change the language during runtime requires shipping the translation library -> increases app size
- Translation files aren't tree shakable
  - We don't know which translations are not used
  - We will have to load all the translation strings (even if it's not used)

With Angular's approach we get:

- It only includes translation strings that are used in the app
- Optimized for load and performance
