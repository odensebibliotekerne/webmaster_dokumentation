## 2 Konfigurationsændring via brugergrænsefladen

Det er muligt, at ændre konfiguration via brugergrænsefladen, man skal dog være meget opmærksom, da det kan overskrive konfigurationer som bliver sat via DPL-CMS.

I projektetet er der installeret config_ignore modulet, hvor der er sat en række konfigurationer, som ikke skal overskrives når sitet opdateres.
https://www.drupal.org/project/config_ignore
admin url: /admin/config/development/configuration/ignore

Der er desuden installeret config_ignore_auto, som automatisk (når man retter på sitet) danner en liste over konfigurationer, som ikke skal overskrives ved opdatering,
https://www.drupal.org/project/config_ignore_auto
admin url: /admin/config/development/configuration/ignore_auto

## Installation

For at kunne se config_ignore og config_ignore_auto skal man slå modulet Configuration Manager til og sættet permission, så man må importere konfigurationer.

For at få config ignore auto til at virke på et lokalt udviklingssite, skal man sættet følgende i web/settings/default/local.settings.php

```php
$config['config_ignore_auto.settings']['status'] = TRUE;
```

For at teste opførsel ved opdatering af sitet skal man køre:

```bash
task dev:cli -- drush deploy
```

Standard configurationen, som hentes ved opdateringer, ligger i roden af projektet i "./config"

## Udvikling

Det er vide muligheder for at ændre og tilføje konfiguration som på et normalt Drupal site. Dog skal man være meget varsom med at ændre konfiguration, som DPL-CMS allerede har sat - det er bedre at tilføje ny konfiguration såsom et helt nyt view eller indholdstype.  
Hvis man har en række konfigurationer, som danner en samlet funktion - kan det være en fordel at lave disse konfigurationer i et modul se webmaster-4-lokalt_modul.

## Anbefalinger

- Lav kun konfigurationsændringer som påvirker DPL-CMS generelle konfigurationsfiler, hvis det er meget nødvendigt. Hvis man gør det, vil man ikke få ændringer i disse konfigurationsfiler i fremtidige releases med + der er ting som kan breake, måske specielt ikke Drupal elementerne som React laget, Go sitet ...
- Navngiv nye konfigurationer, så de ikke får sammenfald med DPL-CMS konfigurationer. Hvis man f.eks. opretter et nyt view kan man med fordel f.eks. give det maskinnavnet BIBLIOTEKSNAVN_personaleoversigt.

# Eksempler

- Oprette et View som viser arrangementer på en anden måde f.eks. til print.
- Lave en paragraph, som kan præsentere indhold på en ny måde f.eks. i et andet layout.
- Lave en ny indholdtype f.eks. personale som kan bruges andre steder på sitet f.eks. i et View.

## Udviklingsmuligheder og afklaring

Det kunne være fint med et modul, som kunne sammenligne config ignore / config ignore auto med filerne i ./config/sync, så det var muligt at se om der var noget, som man uforsætlig var kommet til at overskrive.
