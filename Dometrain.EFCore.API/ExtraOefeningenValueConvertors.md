Extra oefeningen ValueConvertors

probeer een value converter te schrijven om Releasedate als char23 op te slaan in db
Als dit is gelukt als char8 => dus zonder - => eigen customconverter schrijven

voeg volgende enum age rating toe op Movie
public enum AgeRating
{
    All = 0,
    ElementarySchool = 6,
    HighSchool = 12,
    Adolescent = 16,
    Adult = 18
}
Voeg wat films toe met verschillende age ratings waaronder minstens één 18
Schrijf een nieuwe controller method op moviecontroller om alle films tot een bepaalde leeftijd op te halen
		optioneel fix dat de enum mooi geformateerd word in de swagger ui door de juiste jsonserializer the gebruiken
Bewaar de enum in db als een string
Ga nu naar swagger en query een film met age rating all.
Je zou nu ook de 18+  film moeten terug krijgen. Zoek uit waarom en hoe je dit kan oplossen.