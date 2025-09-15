# Webmaster DPL CMS

Dette er dokumentation til lokale tilpasninger af DPL-CMS for webmaster biblioteker.

Følgende udviklingsmetoder bliver beskrevet:

- Konfigurationsændring via brugergrænsefladen
- Asset Injector / Add to Head
- Lokale Drupal moduler

Desuden dokumenteres det, hvordan man sætter et lokalt udviklingsmiljø op.

Der er følgende begrænsninger i lokale tilpasning, men ellers er der mange muligheder.

- det kan skabe konflikter, hvis man overskriver eksisterende konfiguration
- det er ikke muligt, at rette i det React genererede indhold
- der er ikke muligt, at tilføje et nyt tema
- det er ikke muligt at installere med composer install, så ekstra php pakker skal loades direkte i et lokalt modul, hvilke ikke er best practice

Der findes desuden dokumentation på:

DPL-Docs
https://danskernesdigitalebibliotek.github.io/dpl-docs/

Folkebibliotekernes Cms Manual
https://www.folkebibliotekernescms.dk/main/
