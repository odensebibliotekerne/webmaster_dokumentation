# 4 Udvikling af lokale moduler

## Installation

Lokale moduler lægges på modultest / produktion ved at uploade dem som zip filer til /admin/modules/install-or-update, og opdateres samme sted. De lægger sig i /web/modules/locale mappen.
Når man skal lægge dem på produktion, skal man give sig selv rettigheder til det.

## Udvikling

Lokale moduler til DPL-CMS udvikles på samme måde som almindelige Drupal moduler med følgende undtagelser

- kan ikke installeres med Composer
- da man ikke kan installere med Composer, skal evt. php pakker lægges i selve modulet
- det er ikke muligt at slette filer, som man allerede har uploadet - kun overskrive dem
- konfigurationsfiler skal indlæses med en service

#### Konfiguration

På grund af den måde DPL-CMS deployes på, skal konfigurationsfiler indlæses på en særlig måde.

Det er grundigt beskrevet på:
https://danskernesdigitalebibliotek.github.io/dpl-docs/DPL-CMS/webmaster-modules/#initial-configuration

Udover det der står i dokumentationen, skal man lave en MODULNAVN.services.yml fil, som også kan ses i kdb_brugbyen eksemplet.

Denne måde at skrive konfiguration sikrer at den måde indlæses, når man indlæser modulet og stadig er på sitet efter det er opdateret.

Når man skal installere et module med konfiguration i det lokale udviklingsmiljø, skal man slå config ignore til

```php
$config['config_ignore_auto.settings']['status'] = TRUE;
```

Og derefter teste om konfiguration stadig findes efter et deploy.

```sh
task dev:cli -- drush deploy
```

## Anbefalinger

# Eksempler

1. Personaleoversigt: Modul med både konfiguration og CSS

Moduler der importerer konfigurationen for et view, en indholdstype, 3 taksonomier, et tekstformat, et billedformat og tilføjer noget CSS.

https://github.com/odensebibliotekerne/webmaster_local_staff

Der er ikke lavet demo indhold.

2. Snevejr: Asset Injector kode som modul

Eksemple på et modul, som indlæser JS/CSS istedet for at gøre det i asset injector. Fordelen er at koden er versioneret og det gør det nemmere at udvikle.
Ulempen er, at når man har uploadet kan man kun overskrive, ikke slette filer igen.
https://github.com/odensebibliotekerne/webmaster_local_snow

3. Serie: Javascript

Her kommer et eksempel på et modul, som skaber en side på samme måde som DPL React.

4. PDF generering: Modul med php pakker

Da man ikke kan installere moduler med composer, kan man ikke hente pakker til f.eks. PDF- eller CSV-generering. Det samme er tilfældet for contrib-moduler. Det er ikke best practice at inludere moduler på denne måde pga. opdatering af pakkerne.

Et eksempel på, hvordan man kan inkludere php pakker fra f.eks. Packagist direkte i lokale moduler kan hentes her:
https://github.com/odensebibliotekerne/example_local_package

Modulet danner en PDF udfra en simpel HTML streng. En anden anvendelsesmulighed kunne være at danne en PDF udfra arrangementerne på DPL-CMS, som kan printes til/hentes af brugerne af biblioteket.

## Udviklingsmuligheder og afklaring

Det kunne være godt at gøre specielt test-miljøet mindre "black box" agtigt.

- Man skal erstatte de filer, som man uploader, istedet for at lægge dem oven i (test + produktion).
- Det skal være muligt at se, hvad man har liggende i module/local mappen (test + produktion).
- Det skal være muligt at slette fra module/local mappen (minimum på test).
- Man skal kunne få et database dump fra test til lokal udvikling.
- Se databaselog eller andre logs (test + produktion).
- Man skal kunne trigger et test-deploy (test).
- Asset injector og den hårde caching spiller ikke så godt sammen, hvis man skal teste.
