# Data Management Strategy (Proposal W)

## Guiding Principles

1. <ins>Separate business logic and storage layer</ins>. The software's main functionality should be decoupled from the data storage functions. Changing how and where program data is stored in future releases shouldn't require updating business logic methods.
2. <ins>Data protection is the user's responsibility</ins>. Allow the user to backup or encrypt their data according to their own needs and expectation, which might be higher (or lower) than the developers' of this software. The software should only perform minimum checks to ensure imported data isn't corrupt.
3. <ins>Performance</ins>. The target audience is the individual home user. A typical user might be expected to save dozens, maybe hundreds, of recipes, but probably not thousands or millions. The software only needs to be able to parse saved data fast enough to offer satisfactory performance to the intended users.
4. <ins>Cross-platform</ins>. The software should be releasable for and deployable on multiple platforms, including ones not currently planned for, e.g. mobile devices, smart appliances, web
5. <ins>Open, industry-standard format</ins>. The software should save data in a format which can be read and manipulated by standard industry tools. It should be easy for users to import and export their data from the software. Other software should be able to easily read and understand data saved by this application.
6. <ins>Compatibility</ins>. The user should be able to easily export and import their saved data from the application, including between multiple platforms which (due to limitations of the respective platforms) use different storage formats or providers.

## Implementation

1. The software will include a self-contained storage provider for data persistence which can be transparently injected into the business logic. Additional providers can be implemented as needed and selected based on configuration. 
2. The software will store user data in external files, either on the local filesystem or a cloud storage container, as configured. The user can protect these files by backing them up or using an encrypted filesystem, if desired, using purpose-built tools.  The default local storage locations will observe best practice guidelines for each supported operating system, e.g. %APPDATA% on Windows, ~/ on \*nix
3. The storage layer must be performant enough to accommodate a reasonable number of recipes and ingredients for an individual user on the targeted hardware platform(s).
4. The software will be able to operate self-contained without any external dependencies, e.g. local server, cloud database provider, but can make use of them if available and storage providers for them are implemented.
5. The software will serialize user settings and data as JSON objects, either using built-in language features or common industry standard parsing libraries.
6. To allow users to easily import/export their saved data on platforms where they don't have direct access to or control over the saved data files, the program interface will include a function to export the recipe book and ingredient list as a raw JSON object encoded as a base64 string, and import using the same format.

## Strengths & Weaknesses

1. High compatibility. Virtually every platform can store and read JSON flat files with standard CRUD functions. Wrapping the storage layer in an injectable interface allows for implementing more sophisticated storage options, i.e. cloud, later if desired.
2. Minimal dependencies. Storing local flat files doesn't rely on any other service, e.g. SQL server, which may not be available on some platforms like mobile devices. Support for parsing JSON is either built in to most languages or available with well-supported free libraries.
3. Avoiding lock-in. As an open industry-standard format, JSON can be read and manipulated by a wide variety of tools, enhancing forward compatibility and potential interoperability with other software.
4. Simplicity. Using a non-relational data store can be implemented without tables or normalization.

### Weaknesses

1. Scalability. There are upper limits to the size of a document based data store while providing satisfactory performance. While these limits should accommodate most individual users, if the design requirements change, a new data management plan will need to be developed.
2. Cost. Using a document-based data store means rewriting the entire store every time a single value changes. This might not be cost-effective when using storage providers that charge for every byte going in or out. (unless the software is also running in that provider's ecosystem, e.g. EC2 -> S3)

## Supported Languages/Storage Targets

### Language
- python: [builtin support](https://docs.python.org/3/library/json.html)
- C#: [builtin since .NET Core 3.0](https://learn.microsoft.com/en-us/dotnet/standard/serialization/system-text-json/how-to?pivots=dotnet-8-0)
- Java: multiple open source libraries, including [Gson](https://github.com/google/gson)
- Javascript: [native support](https://262.ecma-international.org/14.0/#sec-json-object)

### Cloud Storage
- [aws](https://docs.aws.amazon.com/documentdb/latest/developerguide/how-it-works.html)
- [gcp](https://cloud.google.com/firestore/docs/create-database-server-client-library)
- [azure](https://learn.microsoft.com/en-us/azure/cosmos-db/nosql/quickstart-template-json)

## Format Example

### Sample Raw JSON Recipe Book
```javascript
{
	version: 1,
	recipebook: [
	{
		id: 0,
		name: "recipe one",
		servings: 2,
		ingredients: [
			["water", 250, "mL"],
			["flour", 750, "g"],
			["apple", 5, "each"],
		]
	},
	{
		id: 1,
		name: "recipe two",
		servings: 1,
		ingredients: [
			["noodles", 200, "g"],
			["ground beef", 250, "g"],
			["onion", 100, "g"],
		]
	},
	
	],
	pantry: [
		["sugar", 500, "g"],
		["rice", 2, "kg"],
		["apple", 25, "each"],
		["milk", 750, "mL"],
	]
}
```

### Same RecipeBook exported as base64 

`eyJ2ZXJzaW9uIjoxLCJyZWNpcGVib29rIjpbeyJuYW1lIjoicmVjaXBlIG9uZSIsInNlcnZpbmdzIjoyLCJpbmdyZWRpZW50cyI6W1sid2F0ZXIiLDI1MCwibUwiXSxbImZsb3VyIiw3NTAsImciXSxbImFwcGxlIiw1LCJlYWNoIl1dfSx7Im5hbWUiOiJyZWNpcGUgdHdvIiwic2VydmluZ3MiOjEsImluZ3JlZGllbnRzIjpbWyJub29kbGVzIiwyMDAsImciXSxbImdyb3VuZCBiZWVmIiwyNTAsImciXSxbIm9uaW9uIiwxMDAsImciXV19XSwicGFudHJ5IjpbWyJzdWdhciIsNTAwLCJnIl0sWyJyaWNlIiwyLCJrZyJdLFsiYXBwbGUiLDI1LCJlYWNoIl0sWyJtaWxrIiw3NTAsIm1MIl1dfQ==`