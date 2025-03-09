
#### server.js

```js
const express = require('express');
const app = express();
const path = require('path');
const cors = require('cors');
const { logger } = require('./middleware/logEvents.js');
const errorHandler = require('./middleware/errorHandler.js');
const PORT = process.env.PORT || 3500;


// custom middleware logger
app.use(logger)

  
// Cross Origin Resource Sharing
const whitelist = ['https://www.yourwebsite.com', 'http://127.0.0.1:5500', 'http://localhost:3500'];

const corsOptions = {

origin : (origin, callback) => {

if(whitelist.indexOf(origin) !== -1 || !origin) {

callback(null, true)

} else {

callback(new Error('Not allowed by CORS'));

}

},

optionsSuccessStatus: 200

}

app.use(cors(corsOptions));

  

// built-in middleware to handle urlencoded data
// in other words, form data:
// 'content-type: application/x-www-form-urlencoded'
app.use(express.urlencoded({ extended : false}));

  

// built-in middleware for json
app.use(express.json());

  

// serve static files
app.use('/', express.static(path.join(__dirname , '/public')));
app.use('/subdir', express.static(path.join(__dirname , '/public')));


// routes
app.use('/' , require('./routes/root'));
app.use('/subdir' , require('./routes/subdir'));
app.use('/employees' , require('./routes/api/employees'));

  
app.all('*' , (req, res) => {
	res.status(404);
	if(req.accepts('html')) {
		res.sendFile(path.join(__dirname, 'views', '404.html'));
	} else if (req.accepts('json')) {
		res.json({ error : "404 Not Found"});
	} else {
		res.type('txt').send("404 Not Found");
	}
});


app.use(errorHandler)

app.listen(PORT , () => console.log(`Server running on port ${PORT}`));
```

#### employees.js

```js
const express = require('express');
const router = express.Router();
const data = {}

data.employees = require('../../data/employees.json');

router.route('/').get((req, res) => {
	res.json(data.employees);
})

.post((req, res) => {
	res.json({
		"firstname" : req.body.firstname,
		"lastname" : req.body.lastname
	});
}).put((req , res) => {
	res.json({
		"firstname" : req.body.firstname,
		"lastname" : req.body.lastname
	});
}).delete((req, res) => {
	res.json({ "id" : req.body.id })
});

router.route('/:id').get((req, res) => {
	res.json({ "id" : req.params.id });
})
  
module.exports = router;
```

#### root.js

```js
const express = require('express');
const router = express.Router();
const path = require('path');


router.get('^/$|/index(.html)?' , (req, res) => {
	//res.sendFile('./views/index.html' , { root : __dirname});
	res.sendFile(path.join(__dirname, '..', 'views', 'index.html'));
});


router.get('/new-page(.html)?' , (req, res) => {
	//res.sendFile('./views/index.html' , { root : __dirname});
	res.sendFile(path.join(__dirname, '..', 'views', 'new-page.html'));
});


router.get('/old-page(.html)?' , (req, res) => {
	//res.sendFile('./views/index.html' , { root : __dirname});
	res.redirect(301, '/new-page.html'); // 302 by default
});
  
module.exports = router;
```


### subdir.js

```js
const express = require('express');
const router = express.Router();
const path = require('path');

router.get('^/$|/index(.html)?' , (req, res) => {
	res.sendFile(path.join(__dirname, '..', 'views', 'subdir', 'index.html'));
});

router.get('/test(.html)?' , (req, res) => {
	res.sendFile(path.join(__dirname, '..', 'views', 'subdir', 'test.html'));
});

module.exports = router;
```


#### Chaining Routes

```js
// Route handlers
app.get('/hello(.html)?', (req, res, next) => {
	console.log('attempted to load hello.html');
	next()
}, (req, res) => {
	res.send('Hello World!');
})

  

// Chaining route handlers
const one = (req, res, next) => {
	console.log('one');
	next();
}

const two = (req, res, next) => {
	console.log('two');
	next();
}


const three = (req, res, next) => {
	console.log('three');
	res.send('Finished!');
}

app.get('/chain(.html)?', [one, two, three]);
```

