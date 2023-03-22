---
description: Migration guide from v5 to v6
---

# ⬆ v5 -> v6

### Main script renamed to `keycloakify`

You can search and replace `build-keycloak-theme` -> `keycloakify` in your project.

`package.json`

```diff
 "scripts": {
     "start": "react-scripts start",
     "build": "react-scripts build",
-    "keycloak": "yarn build && build-keycloak-theme"
+    "keycloak": "yarn build && keycloakify"
 },
 "dependencies": {
-    "tss-react": "^3.6.0",
-    "keycloakify": "^5.7.0",
+    "keycloakify": "^6.8.8"
 }
```

.github/workflows/ci.yaml

```diff
-- run: npx build-keycloak-theme
+- run: npx keycloakify
-- run: npx build-keycloak-theme --external-assets
+- run: npx keycloakify --external-assets
```

### Components exported using default export

In order to enable you to use `React.lazy()`, Keyclaokify components are now exported with default exports instead of named exports. &#x20;

```diff
-import { KcApp, defaultKcProps, getKcContext } from "keycloakify";
+import KcApp, { defaultKcProps, getKcContext } from "keycloakify";

-import { Login } from "keycloakify/lib/components/Login";
+import Login from "keycloakify/lib/components/Login";
```

Once you're at it, it might be a good time to update your app to use `<Suspense/>` and `React.lazy()` in order to reduce your bundle size.  See [keycloakify-starter (CSS only)](https://github.com/garronej/keycloakify-starter) or [keycloakify-advanced-starter (component level customization)](https://github.com/garronej/keycloakify-advanced-starter) to see how it's suposed to be setup.

You can also have a look at a real world migration: &#x20;

* [An app using Keycloakify v5](https://github.com/etalab/sill-web/tree/f1b93012555f8a4c1c5e5afd9020b6246421b64e)
* [The same app after upgrade to v6](https://github.com/etalab/sill-web/tree/main/src/ui/components/KcApp)

### i18n: Adding i18n messages keys

In v5 and prior, Keycloakify only provided [a very hacky way](https://docs.keycloakify.dev/v/v5/adding-text-keys) of customizing internatiznalized message. &#x20;

Keycloakify v6 now has a proper i18n api.

{% content-ref url="../adding-text-keys.md" %}
[adding-text-keys.md](../adding-text-keys.md)
{% endcontent-ref %}

### Tems and conditions

The message `termsTitle` ([_Terms and Conditions_ in en.ts](https://github.com/InseeFrLab/keycloakify/blob/f0ae5ea908e0aa42391af323b6d5e2fd371af851/src/lib/i18n/generated\_messages/18.0.1/login/en.ts#L66)) was repmaced by a blank string in v5. If you want to do the same in v6 you have to use the new [i18n API](../adding-text-keys.md).

```diff
 useDownloadTerms({
   kcContext,
   "downloadTermMarkdown": async ({ currentKcLanguageTag }) => {
   
-     kcMessages[currentKcLanguageTag].termsTitle = "";

     const markdownString = await fetch((() => {
       switch (currentKcLanguageTag) {
         case "fr": return tos_fr_url;
         default: return tos_en_url;
       }
     })()).then(response => response.text());

     return markdownString;

   }
 });
 
 const i18n = useI18n({
    kcContext,
    "extraMessages": {
        "en": {
+            "termsTitle": "",
        },
        "fr": {
            /* spell-checker: disable */
+           "termsTitle": "",
            /* spell-checker: enable */
        }
    }
});
```

If you have perfomed an modification at the component level of the Terms.tsx component be mindfull that we now use an [Evt](https://www.evt.land/) to re render when the terms Markdown have been downloaded.

{% embed url="https://github.com/etalab/sill-web/blob/main/src/ui/components/KcApp/Terms.tsx" %}
Example of component level configuration of the Terms page
{% endembed %}

### useFormValidationSlice()

`useFormValidationSlice()` now require you to pass a i18n object, see [I18n API](../adding-text-keys.md).

```diff
 import { useFormValidationSlice } from "keycloakify";
 
 const {
     formValidationState,
     formValidationReducer,
     attributesWithPassword
 } = useFormValidationSlice({
     kcContext,
     passwordValidators,
+    i18n
 });
```
