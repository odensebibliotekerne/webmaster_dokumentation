# 3 Injector

Man kan bruge [Asset Injector](https://www.drupal.org/project/asset_injector) Asset Injector og [Add to Head](https://www.drupal.org/project/add_to_head) modulerne til at indsætte Javascript, Css mm. på sitet.

Asset Injector giver mulighed for at tilføje brugerdefineret CSS eller JavaScript til dit Drupal-site uden at skulle ændre i f.eks. tema eller bruge lokale moduler. Det er praktisk, hvis man hurtigt vil tilpasse udseendet eller funktionaliteten på specifikke sider eller site-wide.

Add to Head modulet bruges til at tilføje scripts, styles eller meta-tags direkte til <head>-sektionen på sitet.

## Installation

Hent modulerne i Zip format fra https://www.drupal.org/project/MODULENAVN/releases/RELEASENUMMER og upload dem på /admin/modules/install-or-update og installer dem på /admin/modules.

## Udvikling

Den nemmeste måde at komme igang med Asset Injector, er ved at oprette koden direkte på modultest sitet /admin/config/development/asset-injector, teste det og overføre til produktionssitet. Det er dog lidt bøvlet at arbejde med, da man hele tiden skal gemme formularen inden man kan se ændringerne, kan løbe ind i cache issues og ikke kan versionsstyre sin kode.

Slå Drupal cache fra imens man udvikler på
/admin/config/development/settings
Login som admin for, at udgå Varnish cache.
Det kan være nødvendigt at genoprette Asset Injector indførslerne for, at anonyme bruger kan se ændringerne pga. Varnish cache.

Koden kan med fordel udvikles i et custom modul i et lokale udviklingsmiljø (se webmaster-1-udviklingsmiljø), og så kan det senere lægges op som i asset injectoren. Fordelene ved dette er, at man kan komme uden om de mange caching lag der er i Dpl Cms, at man kan arbejde i en desktop editor og at man kan versionsstyre sin kode.

Dette modul, som kan bruges som udgangspunkt. Det loader js og css på alle ikke admin sider.
https://github.com/odensebibliotekerne/webmaster_asset_injector_dev

Det kan være nødvendig, at rydde cache på det lokale udviklingssite, for at se ændringer

```sh
task dev:cli -- drush cr
```

### Anbefalinger

Det kan anbefales, at man skrives sin kode i vanilla Javascript, da det ikke er 100 % sikkert at JQuery er loadet på alle sider.

Man skal være opmærksom på "race conditions" altså hvilken kode indlæses første - specielt, hvis man ønsker at indskyde noget, som ændre i de React genererede sider f.eks. søgesider.

## Eksempler

Asset Injector kan bruges på mange måder, nedenfor en en række eksempler.

For yderligere inspiration kan man på andre webmaster biblioteker f.eks. på herningsbib.dk åbner Dev Tools i browseren. Har kan man enten i head se links eller man kan kigger i netværks tab'en og se js/css, som bliver loaded.

1. Ændre indstillinger i formularer, som egentligt kan gøres i indstillinger, men vil resulterer i configuration overwrites (se webmaster-2-konfiguration). Et eksempel er at ændre "Opret billet i billetsystem" knappen til af være slået fra som default, når man opretter et arrangement.

```js
document.addEventListener("DOMContentLoaded", function () {
  console.log(window.location.pathname);

  if (window.location.pathname === "/events/add/default") {
    const checkbox = document.querySelector(
      "#edit-field-relevant-ticket-manager-value"
    );
    if (checkbox) {
      checkbox.checked = false;
    }
  }
});
```

2.  Indsætte elementer på frontenden, hvor man ellers ikke har adgang f.eks. i footer. Her et eksempel på logoer i footer.

```js
document.addEventListener("DOMContentLoaded", () => {
  const anchor = document.querySelector("ul.footer-columns li:last-child");
  if (!anchor) return;

  const container = document.createElement("div");
  container.style.display = "flex";

  const linkEu = document.createElement("a");
  linkEu.href = "/eu";

  const imgEu = document.createElement("img");
  imgEu.src = "/sites/default/files/2025-02/Europe%20Direct%20Odense.png";
  imgEu.id = "eu_direct";
  Object.assign(imgEu.style, {
    display: "block",
    maxWidth: "165px",
    maxHeight: "163px",
    width: "auto",
    height: "auto",
  });

  linkEu.appendChild(imgEu);
  container.appendChild(linkEu);
  anchor.appendChild(container);
});
```

3. Tematiserere hjemmesiden f.eks. med et stille julesnevejr, skabt med en smule javascript
   https://github.com/odensebibliotekerne/odense_assetinjector_snow

4. Lave styleændringer via Css

Style elementer f.eks. begrænset til alle sider med mønsteret /digital/\*

```css
#main-content {
  background-color: #a97c7c;
}
```

Fjerne elementer

```css
/* Fjerner sidste (det aktuelle) element i breadcrumb */
nav.breadcrumb:not(.breadcrumb_material) a:last-child {
  display: none !important;
}
@media screen and (max-width: 767px) {
  /* Minimerer antallet af breadcrumbs i mobilvisning */
  nav.breadcrumb a {
    display: none;
  }
  nav.breadcrumb a:first-child,
  nav.breadcrumb:not(.breadcrumb_material) a:nth-last-child(2) {
    display: initial;
  }
}
```

## Udviklingsmuligheder og afklaring

Hvordan er den optimale brug af Asset Injector i forhold til cache.
