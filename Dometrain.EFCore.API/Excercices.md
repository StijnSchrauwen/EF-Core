Create a new sql server either by installing docker if not already done, and running the DB.PS1 which will spin up a docker image. Or If you already have a running SQL server you can use that as well.
Connect to it with application of your preference Azure Data studio, SSMS and create a MoviesDB.
If you have used the DBscript, You should connect to localhost, with SQL Authentication and 'SA' user. Password you can fetch form the DB.ps file.

Next install the right nugget package to work with EFCore and SQL
Microsoft.EntityFrameworkCore.SqlServer
Note: there are dedicated Packages for almost every different DB engine

Create a "Data" folder
Add your first dbcontext, and name it MoviesContext.
Add the Movie Model to this context using DbSet
Although this is a working dbContext we still have to do some stuff to see it working.

Add the movies context to the MoviesController
try to Implement the getall Controller method

Add the DbContext in the program file. using the AddDbContext 

Next you can execute your first test.
Set a breakpoint in the get all controller method and spin up the code.
From you swagger page, call that getall and inspect the context in your controller, You should get a Movie context.

When you go back to your swagger you will see an expection, we haven't told EF yet which Data base engine it should be using and which db to connect to.
You will read in the error it suggest to fix that in the DB.ContextOnConfiguring()

Go to your movieContext and override the OnConfiguring method
Before calling the base functionallity, use the options builder with Sql extension (Optionsbuilder.UseSQLServer) to pass in the connection string to the database.
Spin up your swagger again

If you are using the latest versions you will get a new error, telling you the certificate isn't trusted, which is okay for now since we are just developing.
You can fix this by adding 'TrustServerCertificate=True;' to your connectionstring.

If al would be okay you should get e new exception, it isn't able to find the object Movie, since we still have an empty db.

Now For us to get the database in a correct shape, in a correct way, we first have to learn a lot more.
And we would rather like to dive in to code fast, don't we?

Lets Quickly fix install a dirty hack to get us going.
But never do this in Production code!!!!
This code will throw away all tables and recreate them so ALL DATA IS LOST every application startup
add this snip below, below this line 'var app = builder.Build();' in you program.cs
	// DIRTY HACK, we WILL come back to fix this
	var scope = app.Services.CreateScope();
	var context = scope.ServiceProvider.GetRequiredService<MoviesContext>();
	context.Database.EnsureDeleted();
	context.Database.EnsureCreated();

If you now again run the app, you don't need to look to your swagger. You can go to your SSMS or Azure data studio and inspect the DB.
You will see the Table Movies is added with the right Columns
Note that for each property a column is created and even a primary key was picked.
Awesome isn't it ?

When going back to the swagger and again querying the get All, you should get back an empty array, which is correct, since our table is still empty.

Go Back to your controller and try implementing the get(by id) en Add methods.
Tip: EF will keep al changes you have made in memory untill you save them in the db. Which it wil then bundle and save as a single transaction.
You can save changes with 'await _context.SaveChangesAsync();'
Set a breakpoint on this line you'll need it later.
Then add this line to return a proper result.         'return CreatedAtAction(nameof(Get), new { id = movie.Id }, movie);'


Adjust the payload below to something you like, and save it here or in a notepad, cause remember our dirty hack, Database will be deleted when we stop and start again.
{
  "title": "string",
  "releaseDate": "2024-02-29T18:43:35.461Z",
  "synopsis": "string"
}

Post this using the swagger to the add method.
You should land onto the breakpoint you set earlier.
Inspect the movie object you are adding to the context.
What do you notice?
Id, is indeed null.
Now step one step further, After the savechanges inspect movie again. Now it should have an Id, Id 1 because it is the first in our Database.
Let the code run through and go back to your browser
Go to the get by ID and try to get the movie by its Id.
Inspect the response If everything works fine.
You can also take a peek in the db if you would like

Next up updating a record
try implementing the update controller method
Fetch a movie from the db => put a breakpoint right behind the fetch
Return not found => if not found
update all of the properties with the ones coming in
Dont forget to call save changes
return the movie as you did in the add method

Now grab the payload you made earlier to test add functionality
use Swagger to add, and afterwards change it a bit and use swagger to update...
You will hit you breakpoint, if you inspect the existing movie you fetched, you 'll notice it is a proxy class en EF keeps the connection to the dbcontext.
As soon as you alter properties on the proxy, it will notify the context there is a change on the object that has to be persisted.
When we call the savechanges, that will make sure we perform an update on that record in the db.
This all works because of the Change tracker each db context has.

As last, implement the delete method.
Play Around with these ones, some work, some I made up...
Question I'll ask you later, one has an advantage over another which one and why?
        _context.Movies.Remove(existingMovie);
        _context.Remove(existingMovie);
        _context.Movies.Remove(id);
        _context.Movies.Remove( new Movie { Id = id });
