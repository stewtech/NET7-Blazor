# EF-Core-VS.-Dapper
EF Core VS. Dapper

What is an ORM?
An ORM (Object-Relational Mapper) is a tool that helps your application communicate with a database.

Instead of writing complex SQL queries, an ORM allows you to work with database data as if it were just regular objects in your programming language.

This makes it easier to manage data, as you don't need to write as much SQL code. It's like a translator that turns your code into commands that the database can understand.

CRUD Operations with Entity Framework
Entity Framework (EF/EF Core) is a comprehensive ORM developed by Microsoft. It handles data management tasks like querying, saving, and updating with minimal manual SQL. Let's see how to perform CRUD operations with EF:

1. Create
To add a new video game character to the database:

public void EF_Create()
{
    var character = new GameCharacter
    {
        CharacterName = "Link",
        PowerLevel = 9001,
        Weapon = "Master Sword"
    };
    context.GameCharacters.Add(character);
    context.SaveChanges();
}
Here, we create a new GameCharacter object and add it to the GameCharacters collection in our database context. The SaveChanges() method saves this new character to the database.

2. Read
To retrieve a character's details:

public void EF_Read()
{
    var character = context.GameCharacters.FirstOrDefault(
        c => c.CharacterName == "Link");
    Console.WriteLine($"Character: {character?.CharacterName}, Weapon: {character?.Weapon}, Power Level: {character?.PowerLevel}");
}
We use FirstOrDefault() to find the first character named "Link" in the database. The ?. operator ensures we handle the case where the character isn't found.

3. Update
To update a character's weapon:

public void EF_Update()
{
    var character = context.GameCharacters.FirstOrDefault(
        c => c.CharacterName == "Link");
    if (character != null)
    {
        character.Weapon = "Hylian Shield";
        context.SaveChanges();
    }
}
After finding the character, we change its Weapon property and call SaveChanges() to update the record in the database.

4. Delete
To remove a character from the database:

public void EF_Delete()
{
    var character = context.GameCharacters.FirstOrDefault(
        c => c.CharacterName == "Link");
    if (character != null)
    {
        context.GameCharacters.Remove(character);
        context.SaveChanges();
    }
}
We locate the character, remove it from the context, and then save the changes to delete it from the database.

CRUD Operations with Dapper
Dapper is a lightweight and fast micro-ORM that requires more manual SQL coding. It provides fine-grained control over SQL operations, making it ideal for performance-critical applications.

1. Create
To add a new game character:

public void Dapper_Create()
{
    using (var connection = new SqlConnection(connectionString))
    {
        string insertQuery = "INSERT INTO GameCharacters (CharacterName, PowerLevel, Weapon) VALUES (@CharacterName, @PowerLevel, @Weapon)";
        connection.Execute(insertQuery,
            new { CharacterName = "Mario", PowerLevel = 7500, Weapon = "Fire Flower" });
    }
}
We use a SQL INSERT statement to add a new character, specifying the character's name, power level, and weapon.

2. Read
To fetch a character's details:

public void Dapper_Read()
{
    using (var connection = new SqlConnection(connectionString))
    {
        string selectQuery = "SELECT * FROM GameCharacters WHERE CharacterName = @CharacterName";
        var character = connection.QueryFirstOrDefault(selectQuery,
            new { CharacterName = "Mario" });
        Console.WriteLine($"Character: {character?.CharacterName}, Weapon: {character?.Weapon}, Power Level: {character?.PowerLevel}");
    }
}
We use a SQL SELECT statement to get the details of the character named "Mario" from the database.

3. Update
To update a character's weapon:

public void Dapper_Update()
{
    using (var connection = new SqlConnection(connectionString))
    {
        string updateQuery = "UPDATE GameCharacters SET Weapon = @Weapon WHERE CharacterName = @CharacterName";
        connection.Execute(updateQuery,
            new { CharacterName = "Mario", Weapon = "Super Star" });
    }
}
The SQL UPDATE statement modifies the weapon of the character named "Mario".

4. Delete
To remove a character:

public void Dapper_Delete()
{
    using (var connection = new SqlConnection(connectionString))
    {
        string deleteQuery = "DELETE FROM GameCharacters WHERE CharacterName = @CharacterName";
        connection.Execute(deleteQuery,
            new { CharacterName = "Mario" });
    }
}
We use a SQL DELETE statement to remove the character named "Mario" from the database.
