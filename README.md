# Titan Project

## Business Design
- To design a java web sevice to search and recommend events for users.

## General Description

- This service implements three main functions:
	* Search nearby events
	* add events to MyFavorite events
	* recommend events for users based on their favorite events and the distance between them and events
- Frontend: an interactive web page with `AJAX` technology implemented with `HTML`, `CSS` and `JavaScript`
- Backend: some `Java Servlet` to handle logic requests using TicketMaster API, to communicate with `MySQL` or `MongoDB` database and to recommend events based on content-based recommendation algorithm
- `Amazon EC2` server delpoyed with this service: (http://54.183.0.244:8080/Titan/)

## APIs
- Logic tier(Java Servlet)
	* Search
		* searchItems
		* Ticketmaster API
		* parse and clean data, saveItems
		* return response
	* History
		* get, set, delete favorite items
		* query database
		* return response
	* Recommendation
		* recommendItems
		* get favorite history
		* search similar events, sorting
		* return response
	* Login
		* GET: check if the session is logged in
		* POST: verify the user name and password, set session time and marked as logged in
		* query database to verify
		* return response
	* Logout
		* GET: invalid the session if exists and redirect to `index.html`
		* POST: the same as GET
		* return response

- TicketMasterAPI
[Official Doc - Discovery API](https://developer.ticketmaster.com/products-and-docs/apis/discovery-api/v2/)

## Database Design
- MySQL
	* users - store user information
	* items - store item information
	* category - store item-category relationship
	* history - store user favorite history

- MongoDB
	* users - store user information and favorite history = (users + history)
	* items - store item information and item-category relationship = (items + category)
	* logs – store log information

## Implementation Details
- Design pattern
	* Builder pattern: `Item.java`
		* When convert events from TicketMasterAPI to java Items, use builder pattern to freely add fields.
	* Factory pattern: `ExternalAPIFactory.java`, `DBConnectionFactory.java`
		* `ExternalAPIFactory.java`: support multiple function like recommendation of event, restaurant, news, jobs… just link to different public API like TicketMasterAPI. Improve extension ability.
		* `DBConnectionFactory.java`: support multiple database like MySQL and MongoDB. Improve extension ability.
	* Singleton pattern: `MySQLConnection.java`, `MongoDBConnection.java`
		* Only create specific number of instance of database, and the class can control the instance itself, and give the global access to outerclass

