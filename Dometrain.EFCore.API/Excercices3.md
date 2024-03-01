rename ID on movie to Identifier
To keep this working add [Key] attribute above

Set a Maxlenght for Title using attributes

Change The type of title from NVARCHAR to VARCHAR

rename the class movies to pictures using the Table attribute 
rename the property synopsis to plot and Type to varcharmax 

Limit the Releasedate to just the data

rename movie class/table to pictures using attribute

Because we still have the hack in place, the db will still be dropped and recreated.
Therefore you can see the changes in database by spinning up the application, go take a look


You might have noticed that Title in the database is nullable. Try making it non nullable using attributes.
so Leave the string? as is.

remove all attributes atributes again and try the fluent api approach by adding it in the movie context 
by overriding OnModelCreating(ModelBuilder modelBuilder)
use the modelBuilder.Entity and modelBuilder.Entity<>().Property() to build the custom mapping again

you notice this will get a mess real fast when our application will start to grow.
Create a new folder Entitymapping with A separated MovieMapping class and a GenreMapping class
override in both from IEntityTypeConfiguration<Movie or Genre> and copy paste you movie mapping from the Moviecontext to one file and genre to the other.

The OnModelCreating in the context should be empty again. use applyConfiguration method on the modelbuilder there twice with new movieMapping and new GenreMapping as params.
You could also register all mappings from the assembly in one statement.
 

Next You may add Genre To Movie as a property,
Spin up the application and take a look into the db.
Pay attention to new colums and to Foreign keys

Add the Movies to the Genre class using a collection    public ICollection<Movie> Movies { get; set; } = new HashSet<Movie>();

Now Add a property GenreId as integer to movies and spinn up again.
Check your database, because we are following the conventions, Nothing should have Been changed EF knows the two belong togheter.
But lets now say we cannot follow the conventions and want to rename that to MainGenreId
Spin up again
And you will see EF added the column because it couldn't figure out the two belong togheter.

        builder
            .HasOne(movie => movie.Genre)
            .WithMany(genre => genre.Movies)
            .HasPrincipalKey(genre => genre.Id)
            .HasForeignKey(movie => movie.MainGenreId);
			
check if this works in the db.


We don't want to keep manually putting data in the db each time we spin up and that we can fix using a seed.
        // Seed - data that needs to be created always
        builder.HasData(new Movie
        {
            Identifier = 1,
            Title = "Fight Club",
            ReleaseDate = new DateTime(1999, 9, 10),
            Synopsis = "Ed Norton and Brad Pitt have a couple of fist fights with each other.",
            MainGenreId = 1,
            AgeRating = AgeRating.Adolescent
        });
		
		builder.HasData(new Genre { Id = 1, Name = "Drama" });

		
Add the seeds to the builders and verify it works by spinning up the swagger and trying to get this movie by it's ID
You will notice everything looks fine except for the genre, that is null
Thats the default of EF core
You can Solve this by using an Include genre, on movie in the controller when fetching it.
Now when you run this, you will get a circular reference, a Movie with a genre, which has movies, which have genres
To fix this add the JsonIgnore attribute in the genre class on movies