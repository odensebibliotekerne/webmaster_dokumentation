## 2 Konfigurationsændring via brugergrænsefladen

Det er muligt, at ændre konfiguration for DPL-CMS via brugergrænsefladen f.eks. tilføje en ny indholdstype eller rette i en eksisterende indholdtype f.eks. Artikel.
Rettelser i eksistende konfiguration (det man ikke selv har oprettet) anbefales det kraftigt, at man <ins>ikke</ins> rette i.
Hvis man retter i eksisterende konfiguration, vil man <ins>ikke</ins> modtage kommende rettelser til denne konfiguration.
Eksterne sites som Go! eller andre elementer på sitet som f.eks. en paragraph kan være afhængig af, at man får disse rettelser - og kan i værste fald ikke virke, hvis man har rettet i eksisterende konfiguration.

Således fungerer konfigurationsflowet i DPL-CMS (september 2025)

1 Når sitet opdateres, hentes konfiguratsfilerne fra ./config/sync i rod folderen af projektet - ca. tusind filer.
2 Konfigurationer som DPL-CMS på forhånd har defineret til ikke at skulle overskrives vha. config ignore modulet f.eks. client id/secret til dbc, trækkes fra ca. 20 filer.  
3 Konfigurationer som man selv har ændret / tilføjet via brugergrænsefladen vha. config ignore auto modulet f.eks. et ny indholdtype eller rettelser i eksisterende indholdtype f.eks. Artikel, trækkes fra 0-X filer
4 Konfigurationer som man har tilføjet via en service i et modul, lægges til. Se webmaster-4-lokalt_modul.
5 Filerne fra punkt 1-4 indlæses til den nye konfiguration efter en opdatering af sitet.

Punkt 2
I projektetet er der installeret config_ignore modulet, hvor der er sat en række konfigurationer, som ikke skal overskrives når sitet opdateres.
https://www.drupal.org/project/config_ignore
admin url: /admin/config/development/configuration/ignore

Punkt 3
Der er desuden installeret config_ignore_auto, som automatisk (når man retter på sitet) danner en liste over konfigurationer, som ikke skal overskrives ved opdatering,
https://www.drupal.org/project/config_ignore_auto
admin url: /admin/config/development/configuration/ignore_auto

For at kunne se config_ignore og config_ignore_auto skal man slå modulet Configuration Manager til og sættet permission, så man må importere konfigurationer.

Hvis man ønsker sig et overblik over, hvilke konfigurationer man fået overskrevet og hvad man har lavet af nye, skal man sammenligne

- filerne i ./config/sync
- config ignore linjerne der ligger i /admin/config/development/configuration/ignore
- config ignore auto linjer der ligger i /admin/config/development/configuration/ignore_auto

Så hvis der er konfigurationer i "ignore auto" som ikke optræder i sync, så er det nye konfigurationer.
Hvis de optræder, så har man overskrevet standard konfigurationer. Hvis disse optræder i "ignore" så er det fint, hvis de ikke gør, kan det give problemer.

## Udvikling

Der er vide muligheder for at ændre og tilføje konfiguration, som på et normalt Drupal site.
Dog skal man være meget varsom med at ændre konfiguration, som DPL-CMS allerede har sat - det er bedre at tilføje ny konfiguration så som et helt nyt view eller indholdstype.  
Hvis man har en række konfigurationer, som danner en samlet funktion - kan det være en fordel at lave disse konfigurationer i et modul (se webmaster-4-lokalt_modul).

## Anbefalinger

- Lav kun konfigurationsændringer som påvirker DPL-CMS generelle konfigurationsfiler, hvis det er meget nødvendigt. Hvis man gør det, vil man ikke få ændringer i disse konfigurationsfiler i fremtidige releases med + der er ting som kan breake, måske specielt ikke-Drupal elementerne som React laget, Go sitet ...
- Navngiv nye konfigurationer, så de ikke får sammenfald med DPL-CMS konfigurationer. Hvis man f.eks. opretter et nyt view kan man med fordel f.eks. give det maskinnavnet BIBLIOTEKSNAVN_personaleoversigt.
- Hvis man virkelig har brug for at ændre en indstilling i f.eks. en formular til oprettelser af aktiviteter, skal man undlade at rette direkte i "indholdstypen" - man kan istedet rette formularen via asset injector javascript eller et hook i et modul.

# Eksempler

- Oprette et View som viser arrangementer på en anden måde f.eks. til print.
- Lave en paragraph, som kan præsentere indhold på en ny måde f.eks. i et andet layout.
- Lave en ny indholdtype f.eks. personale som kan bruges andre steder på sitet f.eks. i et View.
