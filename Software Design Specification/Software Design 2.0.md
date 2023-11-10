# Software Architecture Diagram
![alt text](https://github.com/RealSladX/FindMyMeal/blob/5081c52f3004c76ea10ab4f18cb12534b0ba4f4b/Software%20Design%20Specification/Data%20Management%20Strategy/SoftwareDesignSpecification.drawio.png)

## Databases for FindMyMeal
- A database for Ingredients (“Pantry”)
- A database for Recipes (“Recipe Book”)
- A database for User Information
- A database for Planners ("Planner Archive")
- A database for Stores.

# Data Management Strategy

## Guiding Principles

1. <ins>Separate business logic and storage layer</ins>. The software's main functionality should be decoupled from the data storage functions. Changing how and where program data is stored in future releases shouldn't require updating business logic methods.
2. <ins>Data protection is the user's responsibility</ins>. Allow the user to backup or encrypt their data according to their own needs and expectation, which might be higher (or lower) than the developers' of this software. The software should only perform minimum checks to ensure imported data isn't corrupt.
3. <ins>Performance</ins>. The target audience is the individual home user. A typical user might be expected to save dozens, maybe hundreds, of recipes, but probably not thousands or millions. The software only needs to be able to parse saved data fast enough to offer satisfactory performance to the intended users.
4. <ins>Cross-platform</ins>. The software should be releasable for and deployable on multiple platforms, including ones not currently planned for, e.g. mobile devices, smart appliances, web
5. <ins>Open, industry-standard format</ins>. The software should save data in a format which can be read and manipulated by standard industry tools. It should be easy for users to import and export their data from the software. Other software should be able to easily read and understand data saved by this application.
6. <ins>Compatibility</ins>. The user should be able to easily export and import their saved data from the application, including between multiple platforms which (due to limitations of the respective platforms) use different storage formats or providers.

## Implementation

1. The software will include the use of MongoDB, a NoSQL "Database as a Service", for Document Storage, Cloud Management, and Password Salting. Additional providers can be implemented as needed and selected based on configuration. 
2. The software will store user data in external files, cloud storage container, as configured. The DaaS will protect these files by backing them up or using an encrypted filesystem. 
3. The storage layer will be performant enough to accommodate a reasonable number of recipes and ingredients for an individual user on the targeted hardware platform(s).
4. The software will serialize user settings and data as JSON objects, either using built-in language features or common industry standard parsing libraries.
5. To allow users to easily import/export their saved data on platforms where they don't have direct access to or control over the saved data files, the program interface will include a function to export the recipe book and ingredient list as a raw JSON object encoded as a base64 string, and import using the same format.

## Strengths

1. High compatibility. The flexibility of MongoDB to provide driver compatibility to all languages, frameworks, GUI, IDE, etc. MongoDB Databases can be printed as JSON to ensure readability across all devices.
2. Utilization of MongoDB provides intuitive access and control over structure and status. 
3. Avoiding lock-in. As an open industry-standard format, JSON can be read and manipulated by a wide variety of tools, enhancing forward compatibility and potential interoperability with other software.
4. Simplicity. Using a non-relational data store can be implemented without tables or normalization.
5. Performance. Throught the storage of data in the RAM of Mongo DB, executing queries are faster. 

### Weaknesses

1. Scalability. MongoDB requires high amounts of storage to account for duplication data due to it lacking Join functionality.
2. Document Limitation. 16MB for a document and maximum 100 levels of nesting
3. Cost. Using a document-based data store means rewriting the entire store every time a single value changes. This might not be cost-effective when using storage providers that charge for every byte going in or out. (unless the software is also running in that provider's ecosystem, e.g. EC2 -> S3)
## Supported Languages/Storage Targets

### Language
- python: [builtin support](https://www.mongodb.com/languages/python)
- C#: [MongoDB .NET Driver API]((https://mongodb.github.io/mongo-csharp-driver/2.22/html/R_Project_CSharpDriverDocs.htm))
- Java: [MongoDB Java](https://www.mongodb.com/languages/java)
- Javascript: [MongoDB Node.js Driver]((https://mongodb.github.io/node-mongodb-native/6.2/))

### Cloud Storage
- [MongoDB Atlas](https://www.mongodb.com/products/platform/cloud#atlasMDB)

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
