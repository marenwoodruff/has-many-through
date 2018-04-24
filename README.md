
# Many-to-many Books App

## Objectives

- Create a full CRUD has-many-through Books app with views in Express, and Node- using a postgres database.  
- Add in authentication.

<br />

## Requirements (each of these are part of the rubric

- fork + clone this repo
- create a pull request immediately
- good indentation
- have at least **30 commits** with **good commit messages**
- up to date dev branch (also pushed to github)
	- you can add other branches off of dev 
- build out the books (we will start books together) + authors controllers (bonus to add in users + authentication- **challenge yourself!!!**)
- build out all models
- work with PostgreSQL database via sequelize
- build out books + user seeds files
- connect the books + authors controllers through the authorBooks join table
- add links between pages
- add a navbar (in a partial)
- add a footer (in a partial) that is at least as tall as the window, even if there isn't that much content
- create a welcome page

#### suggestions

- make sure that you have saved everything to your package.json
- make sure that you have required packages and your controllers correctly
- make sure that your views are in the correct folder
- make sure that you have a layout.ejs view that is set up correctly
- make it pretty
- create a well-organized readme
- create a github project with user stories

<br />

## And so it begins...

### 1. Create the ERDS

- A `user` has many `books`
- A `book` has many `authors` through `authorBooks`
- An `author` has many `books` through `authorBooks`

```
user
	- id (integer)
	- first_name (string)
	- last_name (string)
	- email (string)
	- password (string)

books
	- id (integer)
	- title (string)
	- description (string)
	- genre (string)
	- user_id (integer)

authors
	- id (integer)
	- name (string)
	- country (string)

authorBooks
	- id (integer)
	- book_id (integer)
	- author_id (integer)
```

<br />

## Set up the app

### 2. install dependencies/get the app up and running

```
express --view=ejs --git myBooksApp
cd myBooksApp
npm install
atom .

npm install --save express-ejs-layouts
npm install --save method-override
npm install --save sequelize
npm install --save pg pg-hstore
npm install --save sequelize-cli
sequelize init
```

- git init
- git status
- git add
- git commit
- git push

#### in the app.js

```
var ejsLayouts = require('express-ejs-layouts');
var methodOverride = require('method-override');

app.use(ejsLayouts);
app.use(methodOverride('_method'));
```

#### update the title on the index controller
- add a `views/layout.ejs`, and DRY up the views/index.ejs

#### in the layout.ejs

```
<!DOCTYPE html>
<html>
  <head>
    <title>My Books App</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <div class="container">
      <%- body %>
    </div>
    <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js" integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q" crossorigin="anonymous"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl" crossorigin="anonymous"></script>
  </body>
</html>
```

#### in the package.json
- add `"start:dev": "nodemon ./bin/www"` to your scripts object


#### update config/config.json- add in postgres

```
{
  "development": {
    "database": "books",
    "host": "127.0.0.1",
    "dialect": "postgres"
  },
  "test": {
    "database": "books_test",
    "host": "127.0.0.1",
    "dialect": "postgres"
  },
  "production": {
    "database": "books_production",
    "host": "127.0.0.1",
    "dialect": "postgres"
  }
}

```

#### create your postgres database, in a new tab

```
createdb books
psql
\l
\c books
\d
\q
```

#### update the index controller 
```
router.get('/', function(req, res, next) {
  res.render('index', { title: 'The Books App' });
});
```

#### in another tab
- start the server- npm run start:dev
- go to localhost:3000 to make sure that you didn't break anything

#### in the first tab
- git commit

<br />

## Books Controller

### 3. set up books_controller.js
`touch routes/books_controller.js`

#### in the books_controller.js

```
const express = require('express');
const router = express.Router();

module.exports = router;
```

#### in the app.js

```
var booksController = require('./routes/books_controller');

...

app.use('/books', booksController);
```

- check in postman/browser
- git commit...

### 4. set up the index route

```
// index route
router.get('/', (req, res) => {
  res.send('we did it');
});
```

- check in postman/browser
- git commit...

<br />

## Models

### 5. create your models

#### user model
```
sequelize model:create --name user --attributes firstName:string,lastName:string,email:string,password:string
```

- check that your model looks correct in your migration

```
sequelize db:migrate
```

- check your psql database/schemas
- git commit...

#### book model
```
sequelize model:create --name book --attributes title:string,genre:string,description:text,userId:integer
```

- check that your model looks correct in your migration

```
sequelize db:migrate
```

- check your psql database/schemas
- git commit...

#### author model
```
sequelize model:create --name author --attributes name:string,country:string
```

- check that your model looks correct in your migration

```
sequelize db:migrate
```

- check your psql database/schemas
- git commit...

#### authorBook model
```
sequelize model:create --name authorBook --attributes bookId:integer,authorId:integer
```

- check that your model looks correct in your migration

```
sequelize db:migrate
```


- check your psql database/schemas
- git commit...


### 6. add the books model to your Books controller
`const Book = require('../models').book;`

- git commit...

<br />

## Seeds

### 7. add a user seeds file

```
mkdir db
touch db/users_data.js
```

#### in db/users_data.js

##### set up the seeds:

```
module.exports = {
  [
    {
      first_name: "Schmitty",
      last_name: "Schmitt",
      email: "schmitty@gmail.com",
      passwored: "123456"
    },
    {
      first_name: "Mike",
      last_name: "Dang",
      email: "mikeyD@gmail.com",
      passwored: "123456"
    }
  ]
};
```

#### sequelize seed:generate

```
sequelize seed:generate --name user-data
```

#### sequelize db:seed your seeds

- add in a reference to your user data

```
var userData = require('../db/users_data');
```

- add the variable to your up/down functions
- git commit...



### 8. add a book seeds file

```
touch db/books_data.js
```

#### in db/books_data.js

##### set up the seeds:

```
module.exports = {
  [
    {
      title: "The Eyre Affair: A Thursday Next Novel",
      genre: "fiction",
      description: "A literary detective pursues a master criminal through the world of Charlotte Bronte's Jane Eyre",
      user_id: 1
    }, 
    {
      title: "Bad Feminist: Essays",
      genre: "non-fiction",
      description: "Bad Feminist explores being a feminist while loving things that could seem at odds with feminist ideology",
      user_id: 2
    }
  ]
};
```

#### sequelize seed:generate

```
sequelize seed:generate --name books-data
```

#### sequelize db:seed your seeds

- add in a reference to your book data

```
var bookData = require('../db/books_data');
```

- add the variable to your up function
- git commit...


## Index Route

### 9. add an index view

```
mkdir views/books
touch views/books/index.ejs
```

- add an h1- `<h1>my books</h1>`
- check in postman/browser
- git commit...



### 10. update your index route

#### in the books_controller.js

```
// index route
router.get('/', (req, res) => {
  // res.send('we did it');
  
  Book.findAll()
    .then((books) => {
      res.render('books/index', {
        books
      });
    })
    .catch((err) => {
      res.status(400).render('error');
    });
});
```

- check in postman/browser
- git commit...


### 11. update the index view

#### in views/books/index.ejs

```
...

<% if (books && books) { %>
  <ul>
    <% books.forEach((book) => { %>
      <li class="margin-bottom">
        <h2><a href="/books/<%= id %>"><%= book.name %></a></h2>
      </li>
    <% }); %>

    <a class="btn btn-outline-info margin-top" href="/books/new">add a new book</a>
  </ul>
<% } else { %>
  <h3>You don't have any books!</h3>
  
  <a class="btn btn-outline-info margin-top" href="/books/new">add a new book</a>
<% } %> 
```

#### in public/stylesheets/styles.css

```
body {
  margin: 10vh 0px;
  font: 14px "Lucida Grande", Helvetica, Arial, sans-serif;
  font-size: 1em;
  text-align: center;
}

h1, .margin-bottom {
  margin-bottom: 50px;
}

.margin-top {
  margin-top: 50px;
}

.sm-margin-top {
  margin-top: 25px;
}

ul {
  list-style-type: none;
  padding-left: 0px;
}

a {
  color: #68b3c2;
}
```

- check in postman/browser
- git commit...

<br />

## Show Route

### 12. add a show route

```
// show route
router.get('/:id', (req, res) => {
  res.send('we showed it');
});
```

- check in postman/browser
- git commit...


### 13. add a show view
`touch views/books/show.ejs`

- add in `<h1><%= book.title %></h1>`
- git commit...

### 14. update the books_controller.js

```
// show route
router.get('/:id', (req, res) => {
  // res.send('we showed it');
  
  Book.findById(req.params.id)
    .then((book) => {
      res.render('books/show', {
         book
      });
    })
    .catch((err) => {
      res.status(400).render('error');
    });
});
```

- check in postman/browser
- git commit...

### 15. update the show.ejs

```
...

<h3>genre: <%= book.genre %></h3>
<h3>description: <%= book.description %></h3>
<br />

<a class="btn btn-outline-info margin-top" href="/books/<%= book.id %>/edit">edit book</a>
```

- check in postman/browser
- git commit...

<br />

## New Route

### 16. add a new route

```
router.get('/new', (req, res) => {
  res.send('we knew it');
});
```

- check in postman/browser
- git commit...


### 17. add a new view
`touch views/books/new.ejs`

- add in `<h1>add a new book!</h1>`
- git commit...


### 18. update new route

```
// new route
router.get('/new', (req, res) => {
  // res.send('we knew it');

  res.render('books/new')
});
```

- check in postman/browser
- git commit...


### 19. update new view

```
...

<form>
   <div class="form-group">
      <label for="title">book title <span class="red">(required)</span></label>
      <input class="form-control" type="text" id="title" name="title" required>
   </div>
   
   <div class="form-group">
      <label for="genre">genre</label>
      <input class="form-control" type="text" id="genre" name="genre">
   </div>

   <div class="form-group">
      <label for="description">description</label>
      <input class="form-control" type="text" id="description" name="description">
   </div>

   <div class="form-group">
      <input class="btn btn-outline-info"" type="submit" value="submit">
   </div>
</form>


<a class="btn btn-outline-info margin-top" href="/books">back</a>
```

#### update new route

```
// new route
router.get('/new', (req, res) => {
  // res.send('we knew it');

  res.render('books/new', {
    // building an instance of book
    book: Book.build()
  })
  .catch((err) => {
    res.status(400).render('error');
  });
});
```

- check in postman/browser
- git commit...


### 20. add in a post route

```
// create route
router.post('/', (req, res) => {
  // requires an object with properties that map to the properties of the object
  Book.create(req.body)
    .then((hobby) => {
      res.redirect('/books');
    })
    .catch((err) => {
      res.status(400).render('error');
    });
});
```

#### change the form action/method
`<form action="/books" method="POST">`

- check in postman/browser
- git commit...

<br />

## Edit Route

### 21. set up an edit route

```
// edit route
router.get('/:id/edit', (req, res) => {
  res.send('we do it');
});
```

- check in postman/browser
- git commit...


### 22. set up an edit view
`touch views/books/edit.ejs`

- add in `<h1>Edit your '<%= book.title %>'</h1>`
- git commit...


#### 23. update edit route

```
// edit route
router.get('/:id/edit', (req, res) => {
  // res.send('we do it');
  Book.findById(req.params.id)
    .then((book) => {
      res.render('books/edit', {
      	  id: req.params.id,
         book
      });
    })
    .catch((err) => {
      res.status(400).render('error');
    });
});
```

- check in postman/browser
- git commit...


### 24. update edit.ejs

```
...

<form action="/books/<%= book.id %>?_method=PUT" method="POST">
   <div class="form-group">
      <label for="title">Book Title <span class="red">(required)</span></label>
      <input class="form-control" type="text" id="title" name="title" value="<%= book.title %>">
   </div>
   
   <div class="form-group">
      <label for="genre">Genre</label>
      <input class="form-control" type="text" id="genre" name="genre" value="<%= book.genre %>">
   </div>

   <div class="form-group">
      <label for="description">Description</label>
      <input class="form-control" type="text" id="description" name="description" value="<%= book.description %>">
   </div>

   <div class="form-group">
      <input class="btn btn-outline-info margin-top" type="submit" value="Submit">
   </div>
</form>

<a class="btn btn-outline-info margin-top" href="/hobbies/<%= id %>">back</a>
```

- check in postman/browser
- git commit...


### 25. add a PUT route

```
// update route
router.put('/:id', (req, res) => {
  Book.findById(req.params.id)
    .then((book) => {
      return book.update(req.body); // update method returns a promise
    })
    .then((updatedBook) => { // the book parameter is the updated hobby
      res.render('books/show', {
        hobby: updatedBook
      });
    })
    .catch((err) => {
      res.status(400).render('error');
    });
});
```

- check in postman/browser
- git commit...

<br />

## Delete Route

### 14. add a delete route

```
// delete route
router.delete('/:id', (req, res) => {
  Book.findById(req.params.id)
    .then((book) => {
      return book.destroy(); // destroy method is an asynchronous call that returns a promise
    })
    .then(() => {
      // redirect back to index route
      res.redirect('/books');
    })
    .catch((err) => {
      res.status(400).render('error');
    });
});
```

#### on the show page

```
<form action="/books/<%= book.id %>?_method=DELETE" method="POST">
  <input class="btn btn-outline-danger sm-margin-top" type="submit" value="delete this book" />
</form>
```

- check in postman/browser
- git commit...

<br />

---

# CONGRATS!!!

![high five](https://media.giphy.com/media/OcZp0maz6ALok/giphy.gif)

---

<br />

# Next Steps

- Add in your authors controllers
- Connect your books + authors controllers
- Add in your authors views

# Bonus

- Work on authentication with bcrypt
- connect your books + users controllers
- build out your users controller + views

