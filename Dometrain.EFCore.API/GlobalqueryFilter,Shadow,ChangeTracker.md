Voeg een Globalquery filter toe om Geen Films van voor het jaar2000 te tonen als we Queryen
Zorg dat er een film met release date van voor 2000 in de db zit.
Vraag alle films op en verifieer dat deze geen films van voor 2000bevat.
Probeer deze film op te halen aan de hand van zijn id, dit zou niet mogen lukken

Voeg een genreCreated date toe die gegenereerd wordt wanneer een genre wordt aangemaakt maar die niet exposed wordt aan de swagger

Voeg een shadow field deleted op genre toe
Voeg een globalqueryfield toe op dit deleted field
Voeg een SaveChangesInterceptor in een aparte interceptor folder toe
Gebruik de movie context in de interceptor om de change tracker te accessen.
Haal alle deleted Genres op, zet hun status van deleted naar modified en zet de deleted shadow property op true.
Voeg de interceptor toe in de movieContext door de onconfig te overriden
