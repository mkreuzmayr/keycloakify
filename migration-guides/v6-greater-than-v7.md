# ⬆ v6 -> v7

Many things have changed, I invite you to have a look at the latest version of the starter to see what's up: &#x20;

{% embed url="https://github.com/codegouvfr/keycloakify-starter" %}

If you want to update with as little refactoring as possible here are the main takeway is most things have been moved uder the `/login` dir (since we now ave account theme support).

```diff
import { createKeycloakAdapter } from "keycloakify";
-import { useDownloadTerms, getKcContext } from "keycloakify";
+import { useDownloadTerms, getKcContext } from "keycloakify/login";
```

Instead of useI18n is exported an hier order function createUseI18n. You should be able to update quite easyly just by looking at the new setup:

{% embed url="https://github.com/codegouvfr/keycloakify-starter/blob/main/src/keycloak-theme/login/i18n.ts" %}

If you experience issues upgrading to v7 do not hesitate to [ask for help](https://github.com/InseeFrLab/keycloakify/discussions).
