# Convert a CSV stream into a string array

[![NuGet](https://img.shields.io/badge/NuGet-0.1.0-blue.svg)](https://www.myget.org/feed/hilres/package/nuget/Hilres.CSV)

Convert a CSV stream into a string array.

## Using Hilres.CSV

```csharp
namespace ConsoleApp
{
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using Hilres.CSV;

    internal static class Program
    {
        private static void Main(string[] args)
        {
            IEnumerable<Pet> pets;
            string petString = "Id, Name, Type\n"
                             + "1, Rover, Dog\n"
                             + "2, Fluffy, Cat\n";

            using (var stream = new MemoryStream())
            using (var writer = new StreamWriter(stream))
            using (var reader = new StreamReader(stream))
            {
                writer.Write(petString);
                writer.Flush();
                stream.Position = 0;

                // Skip over header.
                string[] header = CSV.Import(reader);

                pets = CSV.ImportToArray(reader)
                    .Select(r => new Pet
                    {
                        Id = int.Parse(r[0]),
                        Name = r[1],
                        Type = r[2]
                    }).ToArray();
            }

            foreach (var pet in pets)
            {
                Console.WriteLine(pet.ToString());
            }
        }
    }

    internal class Pet
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Type { get; set; }

        public override string ToString()
        {
            return $"{Id},{Name},{Type}";
        }
    }
}
```
