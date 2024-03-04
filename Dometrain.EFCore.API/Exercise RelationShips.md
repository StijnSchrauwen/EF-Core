Exercise RelationShips

Voeg deze classe toe aan het Project,
public class ExternalInformation
{
    public string? ImdbUrl { get; set; }
    public string? RottenTomatoesUrl { get; set; }
    public string? TmdbUrl { get; set; }
}

Voeg er de primaryKey en Navigation Property naar Movies toe in een one-one relatie
Voeg ExternalInformation toe aan Movies
Voeg een nieuwe mapping toe 
Voeg de dbset aan de context toe en de configuratie aan de modelbuilder
Voeg een nieuwe migratie toe

Voeg deze classe toe aan het Project,
public class Actor
{
    public int Id { get; set; }
    public required string FirstName { get; set; }
    public required string LastName { get; set; }
}
Voeg een Many-Many relatie tussen Movies en actors in 
Voeg een nieuwe mapping toe 
Voeg de dbset aan de context toe en de configuratie aan de modelbuilder
Voeg een nieuwe migratie toe
