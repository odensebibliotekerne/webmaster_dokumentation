# 1 Udviklingsmiljø

Reload har i DPL-CMS projektet lavet et Docker miljø, som kan bruges til lokal udvikling og indeholder webserver, database etc.

Installation af lokalt udviklingsmiljø kræver installation af en Docker klient f.eks. Docker Desktop eller Orbstack.
Derudeover skal Task https://taskfile.dev/ være installeret.
Kan afvikles på Linux, Mac og Windows Subsystem for Linux.

```sh
git clone https://github.com/danskernesdigitalebibliotek/dpl-cms.git
cd dpl-cms
# hvis man ønsker særlige udgaver af Dpl Cms kan man bruge følgende ellers får man den seneste develop udgave.
# lister releases
git tag -l
# bruge den bestemt branch f.eks. en release f.eks.
git checkout -b 2025-33-0
# bygger et helt nyt Dpl Cms site og henter DPL React
task dev:reset
# sætter bruger 1 adgangskode
task dev:cli -- drush upwd admin etHeltNytUniktPassword
# list kørende docker tjenester
docker ps
```

I "docker ps" listen vil der være en Varnish docker for DPL CMS, som er den man skal tilgå for at se sitet i en browser.
Find varnish under Images hedder f.eks. dpl-cms-varnish og tjek porten under PORTS f.eks. 80/tcp, 8443/tcp, 0.0.0.0:32770->8080/tcp, [::]:32770->8080/tcp.
Sitet kan tilgåes i en browser på f.eks. localhost:32770.

Når sitet er klart, skal man indsætte api nøgler til fbi - men ellers burde indstillinger og rettigheder være klart til, at man kan starte med at udvikle.

Se desuden den officielle dokumentation for flere detaljer såsom certifikater og virtual host (så man ikke skal lede efter en ip adresse).
https://danskernesdigitalebibliotek.github.io/dpl-docs/DPL-CMS/local-development/#docker-setup

Der er lavet en masse kommandoer til f.eks. at starte Xdebug, køre drush kommandoer osv. De kan ses i Taskfile.yml i roden af DPL CMS projektet.

```sh
# kør drush kommando
task dev:cli -- drush ...
# slå dev moduler og indstillinger til
task dev:enable-dev-tools
# slå xdebug til
task dev:enable-xdebug
# stop og start site
task dev:stop
task dev:start
```

## Xdebug

Ved VS Code og Chrome

Dan vscode/launch.json fil
Start Xdebug i docker projektet: task dev:enable-xdebug
Tilføg og start f.eks. Xdebug Chrome Extension
https://chromewebstore.google.com/detail/xdebug-chrome-extension/oiofkammbajfehgpleginfomeppgnglk?utm_source=ext_app_menu
Start debug i VS Code

## Tokens

Når fbi nøgler er indsat, kan man få udleveret fbi token på /dpl-react/user-tokens.

```
window.dplReact = window.dplReact || {};
window.dplReact.setToken("library", "0a4e11b2e3b90c350cf7a440cf9ddfda504051c2")
```

Man kan ikke logge ind som låner, i det lokale udviklingsmiljø. Hvis man har brug for at kunne logge ind som bruger / få et user token - er det nødvendigt, at udvikle direkte på en server (eller bruge en tunnel) go DBC skal tilføje ekstra adresser, som kan logge ind via login.dbc.dk.

## DPL React / DPL Designsystem

Det er muligt, at hente og starte både DPL React og DPL Designsystem, men det er ikke praktisk muligt at implementere ændringer herfra på de lokale sites. Derfor dokumenteres disse dele af tech stacked ikke.

## Udviklingsmuligheder og afklaring
