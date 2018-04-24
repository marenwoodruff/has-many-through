
# Hobbies App

## Objective

### Part 1

- Create a full CRUD hobbies app with views in Express, and Node- using dummy seed data.  

### Part 2

- Add a postgres database and the sequelize CLI.
- Update routes to accomodate this.

<br />

## Part 1

### 1. Create ERDS for models
hobbies
   - name (string/varchar(255))
   - description (string/varchar(255))
   - difficulty (float/integer)
   - levelOfProfficiency (float/integer)     
   - hoursPracticed (float/integer)

### 2. install dependencies/get app up and running

```
express --view=ejs --git myApp
cd myApp
npm install
atom .
npm install express-ejs-layouts
npm install method-override
```

#### in app.js

```
var ejsLayouts = require('express-ejs-layouts');
var methodOverride = require('method-override');
app.use(ejsLayouts);
app.use(methodOverride('_method'));
```

#### update the title on the index controller
- add `views/layout.ejs`, and DRY up the views/index.ejs

#### in layout.ejs

```
<!DOCTYPE html>
<html>
  <head>
    <title>My Hobbies App</title>
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

#### in package.json
- add `"start:dev": "nodemon ./bin/www"`

#### in another tab
- start the server- npm run start:dev
- go to localhost:3000 to make sure that you didn't break anything

#### in first tab
- git init...
- git commit

### 3. set up hobbies_controller.js
`touch routes/hobbies_controller.js`

#### in my_hobbies_controller.js

```
const express = require('express');
const router = express.Router();

module.exports = router;
```

#### in app.js

```
var hobbiesController = require('./routes/hobbies_controller');
app.use('/hobbies', hobbiesController);
```

```
mkdir db
touch db/hobbies_data.js
```

#### in db/hobbies_data.js

##### set up seeds:

```
module.exports = {
  seededHobbies: [
    {
      name: "playing guitar",
      description: "6 stringed instrument- nylon vs steel strings, classical vs flamenco, acoustic vs electric",
      difficulty: 3,
      levelOfProfficiency: 3,
      hoursPracticed: 5
    }, 
    {
      name: "drawing",
      description: "pencil to paper",
      difficulty: 2,
      levelOfProfficiency: 2,
      hoursPracticed: 1
    }
  ]
};
```

##### require the seeds file in your controller

```
var data = require('../db/hobbies_data').seededHobbies;
```

- chcek in postman/browser
- git commit...

### 4. set up index route

```
// index route
router.get('/', (req, res) => {
  res.send('we did it');
});
```

- chcek in postman/browser
- git commit...

### 5. add an index view

```
mkdir views/hobbies
touch views/hobbies/index.ejs
```

- add a dummy h1- `<h1>My Hobbies</h1>`

#### in hobbies_controller.js

```
// index route
router.get('/', (req, res) => {
  // res.send('we did it');
  res.render('hobbies/index');
});
```

#### in views/hobbies/index.ejs

```
<h1>My Hobbies</h1>

<% if (hobbies && hobbies.length) { %>
  <ul>
    <% hobbies.forEach((hobby) => { %>
      <li class="margin-bottom">
        <h2><a href="/hobbies/<%= id %>"><%= hobby.name %></a></h2>
      </li>
    <% }); %>

    <a class="btn btn-outline-info margin-top" href="/hobbies/new">Add a New Hobby</a>
  </ul>
<% } else { %>
  <h3>You don't have any hobbies!</h3>
  <a class="btn btn-outline-info margin-top" href="/hobbies/new">Add a New Hobby</a>
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

### 6. add a show route

```
// show route
router.get('/:id', (req, res) => {
  res.send('we showed it');
});
```

- check in postman/browser
- git commit...

### 7. add a show view
`touch views/hobbies/show.ejs`

#### in hobbies_controller.js

```
// show route
router.get('/:id', (req, res) => {
  // res.send('we showed it');
  var hobby = data[req.params.id];
  
  res.render('hobbies/show', {
    id: req.params.id,
    hobby: hobby
  });
});
```

#### in show.ejs

```
<h1><%= hobby.name %></h1>
<h3>description: <%= hobby.description %></h3>
<br />

<p>
   <strong>difficulty:</strong> <%= hobby.difficulty %>, 
   <strong>level of profficiency:</strong> <%= hobby.levelOfProfficiency %>
</p>

<p>
   <strong>hours practiced:</strong> <%= hobby.hoursPracticed %>
</p>

<a class="btn btn-outline-info margin-top" href="/hobbies/<%= id %>/edit">edit hobby</a>
```

- check in postman/browser
- git commit...

### 8. add a new route

```
router.get('/new', (req, res) => {
  res.send('we knew it');
});
```

- check in postman/browser
- git commit...

### 9. add a new view
`touch views/hobbies/new.ejs`

#### in new view

```
<h1>Add a New Hobby!</h1>

<form>
   <div class="form-group">
      <label for="name">Hobby Name <span class="red">(required)</span></label>
      <input class="form-control" type="text" id="name" required>
   </div>

   <div class="form-group">
      <label for="description">Description</label>
      <input class="form-control" type="text" id="description">
   </div>

   <div class="form-group">
      <label for="difficulty">Difficulty</label>
      <input class="form-control" type="number" id="difficulty" min="0" max="5" step="0.05">
   </div>

   <div class="form-group">
      <label for="levelOfProfficiency">Level of Profficiency</label>
      <input class="form-control" type="number" id="levelOfProfficiency" min="0" max="5" step="0.05">
   </div>

   <div class="form-group">
      <label for="hoursPracticed">Hours Practiced</label>
      <input class="form-control" type="number" id="hoursPracticed" step="0.05">
   </div>

   <div class="form-group">
      <input class="btn btn-outline-info"" type="submit" value="Submit">
   </div>
</form>


<a class="btn btn-outline-info margin-top" href="/hobbies">back</a>
```

- check in postman/browser
- git commit...

### 10. add in a post route

```
// create route
router.post('/', (req, res) => {
  var newHobby = req.body;

  data.seededHobbies.push(newHobby);
  res.redirect('/hobbies');
});
```

#### change the form action/method
`<form action="/hobbies" method="POST">`

- check in postman/browser
- git commit...

### 11. set up an edit route

```
// edit route
router.get('/:id/edit', (req, res) => {
  res.send('we do it');
});
```

- check in postman/browser
- git commit...

### 12. set up an edit view
`touch views/hobbies/edit.ejs`

#### in controller

```
// edit route
router.get('/:id/edit', (req, res) => {
  // res.send('we do it');
  var hobbyToEdit = data[req.params.id];

  res.render('hobbies/edit', {
    id: req.params.id,
    hobby: hobbyToEdit
  });
});
```

#### in edit.ejs

```
<h1>Edit your '<%= hobby.name %>' Hobby!</h1>

<form action="/hobbies/<%= id %>?_method=PUT" method="POST">
   <div class="form-group">
      <label for="name">Hobby Name <span class="red">(required)</span></label>
      <input class="form-control" type="text" name="name" value="<%= hobby.name %>">
   </div>

   <div class="form-group">
      <label for="description">Description</label>
      <input class="form-control" type="text" name="description" value="<%= hobby.description %>">
   </div>

   <div class="form-group">
      <label for="difficulty">Difficulty</label>
      <input class="form-control" type="number" name="difficulty" value="<%= hobby.difficulty %>" min="0" max="5" step="0.05">
   </div>

   <div class="form-group">
      <label for="levelOfProfficiency">Level of Profficiency</label>
      <input class="form-control" type="number" name="levelOfProfficiency" value="<%= hobby.levelOfProfficiency %>" min="0" max="5" step="0.05">
   </div>

   <div class="form-group">
      <label for="hoursPracticed">Hours Practices</label>
      <input class="form-control" type="number" name="hoursPracticed" value="<%= hobby.hoursPracticed %> step="0.05">
   </div>

   <div class="form-group">
      <input class="btn btn-outline-info margin-top" type="submit" value="Submit">
   </div>
</form>

<a class="btn btn-outline-info margin-top" href="/hobbies/<%= id %>">back</a>
```

- check in postman/browser
- git commit...

### 13. add a PUT route

```
// update route
router.put('/:id', (req, res) => {
  var editedHobby = req.body;
  data[req.params.id] = editedHobby;

  res.redirect('/hobbies/show', {
     id: req.params.id,
     hobby: editedHobby
  });
});
```

- check in postman/browser
- git commit...

### 14. add a delete route

```
// delete route
router.delete('/:id', (req, res) => {
  // remove the item from the array
  data.splice(req.params.id, 1); 

  // redirect back to index route
  res.redirect('/hobbies'); 
});
```

#### on the show page

```
<form action="/hobbies/<%= id %>?_method=DELETE" method="POST">
  <input class="btn btn-outline-danger sm-margin-top" type="submit" value="Delete Hobby" />
</form>
```

- check in postman/browser
- git commit...

<br />

---

# Part 2: Add Sequelize

### 1. install sequelize

```
npm install --save sequelize
npm install --save pg pg-hstore
npm install --save sequelize-cli
sequelize init
```

### This will add these folders
- config
   - config.json
- models
- index.js
- seeders
- migrations

#### update config/config.json- add in postgres

```
{
  "development": {
    "database": "hobbies",
    "host": "127.0.0.1",
    "dialect": "postgres"
  },
  "test": {
    "database": "hobbies_test",
    "host": "127.0.0.1",
    "dialect": "postgres"
  },
  "production": {
    "database": "hobbies_production",
    "host": "127.0.0.1",
    "dialect": "postgres"
  }
}

```

#### create your postgres database, in a new tab

```
createdb hobbies
psql
\l
\c hobbies
\d
\q
```

- git commit...

### 2. create a new controller for sequelize (only so that we can compare the two

```
touch routes/hobbies_controller_sequelize.js
```

#### in app.js

```
// var hobbiesController = require('./routes/hobbies_controller');
var hobbiesController = require('./routes/hobbies_controller_sequelize');
```

- git commit...

### 3. create your hobby model

```
sequelize model:create --name hobby -- attributes name:string,description:string,difficulty:float,levelOfProfficiency:float,hoursPracticed:float

sequelize db:migrate
```

- git commit...

### 4. add the model to your controller
`const Hobby = require('../models').hobby;`

- git commit...

### 5. update the index route

```
// index route
router.get('/', (req, res) => {
  // res.send('we did it');

  Hobby.findAll()
    .then((hobbies) => {
      res.render('hobbies/index', {
        hobbies: hobbies
      });
    })
    .catch((err) => {
      res.status(400).render('error');
    });
});
```

#### in index.ejs

```
...
<h2><a href="/hobbies/<%= hobby.id %>"><%= hobby.name %></a></h2>
...
```

- check in postman/browser
- git commit...

### 6. update the show route

```
// show route
router.get('/:id', (req, res) => {
  // res.send('we showed it');
  Hobby.findById(req.params.id)
    .then((hobby) => {
      res.render('hobbies/show', {
        hobby: hobby
      });
    })
    .catch((err) => {
      res.status(400).render('error');
    });
});
```

- check in postman/browser
- git commit...

#### in show.ejs

```
...

<a class="btn btn-outline-info margin-top" href="/hobbies/<%= hobby.id %>/edit">edit hobby</a>

<form action="/hobbies/<%= hobby.id %>?_method=DELETE" method="POST">
  <input class="btn btn-outline-danger sm-margin-top" type="submit" value="delete hobby" />
</form>
```

- check in postman/browser
- git commit...

### 7. update the new route

```
// new route
router.get('/new', (req, res) => {
  // res.send('we knew it');

  res.render('hobbies/new', {
    // building an instance of hobby
    hobby: Hobby.build()
  })
  .catch((err) => {
    res.status(400).render('error');
  });
});
```

- check in postman/browser
- git commit...

### 8. update the post route

```
// create route
router.post('/', (req, res) => {
  // requires an object with properties that map to the properties of the object
  Hobby.create(req.body)
    .then((hobby) => {
      res.redirect('/hobbies');
    })
    .catch((err) => {
      res.status(400).render('error');
    });
});
```

- check in postman/browser
- git commit...

### 9. update the edit route

```
// edit route
router.get('/:id/edit', (req, res) => {
  // res.send('we do it');
  Hobby.findById(req.params.id)
    .then((hobby) => {
      res.render('hobbies/edit', {
        hobby: hobby
      });
    })
    .catch((err) => {
      res.status(400).render('error');
    });
});
```

#### in edit.ejs

```
<form method="POST" action="/hobbies/<%= hobby.id %>?_method=PUT">
   ...

<a class="btn btn-outline-info margin-top" href="/hobbies/<%= hobby.id %>">back</a>
```

- check in postman/browser
- git commit...

### 9. update the put route

```
// update route
router.put('/:id', (req, res) => {
  Hobby.findById(req.params.id)
    .then((hobby) => {
      return hobby.update(req.body); // update method returns a promise
    })
    .then((updatedHobby) => { // the hobby parameter is the updated hobby
      res.render('hobbies/show', {
        hobby: updatedHobby
      });
    })
    .catch((err) => {
      res.status(400).render('error');
    });
});
```

- check in postman/browser
- git commit...

### 10. update the delete route

```
// delete route
router.delete('/:id', (req, res) => {
  Hobby.findById(req.params.id)
    .then((hobby) => {
      return hobby.destroy(); // destroy method is an asynchronous call that returns a promise
    })
    .then(() => {
      // redirect back to index route
      res.redirect('/hobbies');
    })
    .catch((err) => {
      res.status(400).render('error');
    });
});
```

- check in postman/browser
- git commit...

### 11. add a seeds file 

```
sequelize seed:create --name my-seed-file
```

```
'use strict';

module.exports = {
  up: (queryInterface, Sequelize) => {
    /*
      Add altering commands here.
      Return a promise to correctly handle asynchronicity.

      Example:
      return queryInterface.bulkInsert('Person', [{
        name: 'John Doe',
        isBetaMember: false
      }], {});
    */

    return queryInterface.bulkInsert('hobbies', [{
        name: 'playing guitar',
        description: "6 stringed instrument- nylon vs steel strings, classical vs flamenco, acoustic vs electric",
        difficulty: 3,
        levelOfProfficiency: 3,
        hoursPracticed: 5
      }, 
      {
        name: "drawing",
        description: "pencil to paper",
        difficulty: 2,
        levelOfProfficiency: 2,
        hoursPracticed: 1
      }
    ], {});
  },

  down: (queryInterface, Sequelize) => {
    /*
      Add reverting commands here.
      Return a promise to correctly handle asynchronicity.

      Example:
      return queryInterface.bulkDelete('Person', null, {});
    */
    return queryInterface.bulkDelete('hobbies', null, {});
  }
};
```

### 12. sequelize db:migrate 

```
sequelize db:seed:all
sequelize db:migrate
```

- check in postman/browser

<br />

# CONGRATS!!!

![high five](https://media.giphy.com/media/OcZp0maz6ALok/giphy.gif)
