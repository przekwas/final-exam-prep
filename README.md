# Covalence Final Exam
#### Full Stack Assessment
#### TypeScript, React, Express, MySQL
#### 4 Hours

## Objective
Your objective is to build a full-stack web project that interacts with a database of books.  You'll need to create the schema, a REST API that can receive requests and respond appropriately, and a React front-end that can display information from your API.

### Project Setup
You can use the [barebones-react-typescript-express](https://github.com/covalence-io/barebones-react-typescript-express) starter template as your starting point.  Go ahead and install any dependencies you think you might need, and ensure you get a clean development build running via `npm run dev` afterward.

**Note** You can use your server/front-end utilities and middleware for this project, so migrate those files via copy/paste for this exam.
 - Front-end
	 - `utils/api.ts`
- Back-end
	- `utils/security/passwords.ts`
	- `utils/security/tokens.ts`
	- `middleware/bearerstrategy.ts`
	- `middleware/localstrategy.ts`

Attach this starter project to your own GitHub repository and let an instructor know your official start time.  You're required to do a commit *at least* every 30min during the exam.

### Database
Before you start the exam, you'll be need to write the schema yourself, populate it with some fake test data, and create the user with privileges for it to connect to your Express code.  You can use stored procedures, write your queries in Node MySQL, or use Knex if you feel comfortable. When it's time to begin getting data in your API.

#### Tables
* Users
	* id INT PK NOT NULL AI
	* email VARCHAR(60) NOT NULL
	* hash VARCHAR(60) NOT NULL
	* role VARHCAR(25) DEFAULT 'admin'
	* _created DATETIME DEFAULT CURRENT_TIMESTAMP
* Tokens
	* id INT PK NOT NULL AI
	* userid INT NOT NULL FK References Users(id)
	* token TEXT NULL
	* _created DATETIME DEFAULT CURRENT_TIMESTAMP
* Categories
	* id INT PK NOT NULL AI
	* name VARCHAR(50) NOT NULL
* Books
	* id INT PK NOT NULL AI
	* categoryid INT NOT NULL FK References Categories(id)
	* title VARCHAR(100) NOT NULL
	* author VARCHAR(100) NOT NULL
	* price DECIMAL(5,2) NOT NULL
	* _created DATETIME DEFAULT CURRENT_TIMESTAMP

#### Schema
```
CREATE TABLE `Categories` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL,
  PRIMARY KEY (`id`)
);

CREATE TABLE `Books` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `categoryid` int(11) NOT NULL,
  `title` varchar(100) NOT NULL,
  `author` varchar(100) NOT NULL,
  `price` decimal(5,2) NOT NULL DEFAULT '0.00',
  `_created` datetime DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `fk_categoryid_category_id_idx` (`categoryid`),
  CONSTRAINT `fk_categoryid_category_id` FOREIGN KEY (`categoryid`) REFERENCES `Categories` (`id`) ON DELETE NO ACTION ON UPDATE NO ACTION
);

CREATE TABLE `Users` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `email` varchar(60) NOT NULL,
  `password` varchar(60) NOT NULL,
  `role` varchar(25) DEFAULT 'admin',
  `_created` datetime DEFAULT CURRENT_TIMESTAMP,
  `name` varchar(60) NOT NULL,
  PRIMARY KEY (`id`)
);

CREATE TABLE `Tokens` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `token` text,
  `_created` datetime DEFAULT CURRENT_TIMESTAMP,
  `userid` int(11) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `fk_userid_users_id_idx` (`userid`),
  CONSTRAINT `fk_userid_users_id` FOREIGN KEY (`userid`) REFERENCES `Users` (`id`) ON DELETE NO ACTION ON UPDATE NO ACTION
);

INSERT INTO `Categories` VALUES (1,'Science Fiction'),(2,'Fantasy'),(3,'Political Fiction'),(4,'Thriller'),(5,'Mystery');
INSERT INTO `Books` VALUES (2,2,'The Hobbit','J.R.R. Tolkien',9.99,'2019-02-26 13:06:09'),(3,3,'1984','George Orwell',7.49,'2019-02-26 13:08:52'),(4,4,'The Outsider','Stephen King',20.63,'2019-02-26 13:11:07'),(5,5,'The Adventures of Sherlock Holmes','Sin Arthur Conan Doyle',8.99,'2019-02-26 13:12:33'),(11,1,'The Martian','Andy Weir',12.99,'2019-02-27 13:28:04');
```

### REST API
You'll need to connect your schema to your Node project, and have API routes that respond with the typical CRUD operations for books and getting categories.  You'll use auth routes for login and register.

#### API Route Behaviors

* Categories
	* Support getting all categories, and getting a single category by id
* Books
	* Support getting all books, creating a new book, and getting, updating, and deleting a single book
	* Follow RESTful principles when defining routes for the above behaviors
* Login
	* Allow a user to login with email and password, and create a token upon login
* Register
	* Allow a user to register themselves in your schema and receive a token upon successful registration

### React Front-End

Create an React site in the client folder of your project
* Use routing
* Use your utility/fetch to interact with your API
* Use localStorage to handle front-end auth

#### Views
* /
	* Show a page welcoming the user to your book store
	* Have a link to your book listing
	* Have a link to login/register views
* /login
	* Show a page with input fields for email and password to login an existing user.  It should send the user back to the list view upon success.
* /register
	* Show a page with input fields for email and password to register a user new user.  It should send the user back to the list view upon success. 
* /books
	* Show a page listing the books you have available. The listing should include the title, author, price (formatted as currency), and category name for each book.
	* Each item in the listing should have a link to the single view
* /books/new
	* Show a page with input fields for title, author, and price. You will also need to have a select (drop-down) box that shows all categories in the system, allowing the book to be assigned a category. The database will not allow a book to be created without a category.
	* Saving the new book successfully should send the user back to the list view.
	* Should require user to be logged in
* /books/:id/update
	* Show a page with input fields prepopulated with the specified book data. The page should include input fields for title, author, and price. You will also need to have a select (drop-down) box that shows all categories in the system, allowing the book to be assigned to a different category.
	* Saving the updated book successfully should send the user back to either the single view or the list view (your choice).
	* Should require user to be logged in.
* /books/:id
	* Show a page that displays information for just the indicated book. The page should include the title, author, price (formatted as currency), and category name for the book.
	* Should also contain Edit and Delete buttons/icons
	* Clicking the delete button should delete the book and send them to the book list
	* Clicking the edit button should send the user to the edit book component

## Submission Instructions
You may want to commit as you go along. When you are finished, make sure you have PUSHed to github. You will not be able to push to the repository once the assessment is finished. 