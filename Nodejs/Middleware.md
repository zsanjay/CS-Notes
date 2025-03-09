#### server.js file

```node
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

app.use(express.static(path.join(__dirname , '/public')));

  

app.get('/hello-world' , (req, res) => {

res.send("Hello World!");

});

  

app.get('^/$|/index(.html)?' , (req, res) => {

//res.sendFile('./views/index.html' , { root : __dirname});

res.sendFile(path.join(__dirname, 'views', 'index.html'));

});

  

app.get('/new-page(.html)?' , (req, res) => {

//res.sendFile('./views/index.html' , { root : __dirname});

res.sendFile(path.join(__dirname, 'views', 'new-page.html'));

});

  

app.get('/old-page(.html)?' , (req, res) => {

//res.sendFile('./views/index.html' , { root : __dirname});

res.redirect(301, '/new-page.html'); // 302 by default

});

  
  
  

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

  

app.all('*' , (req, res) => {

res.status(404);

if(req.accept('html')) {

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

#### package.json

```node
{

"name": "beginner-course-dave-gray",

"version": "1.0.0",

"description": "Express Demo",

"main": "server.js",

"scripts": {

"dev": "nodemon server",

"start": "node server"

},

"author": "Sanjay Mehta",

"license": "ISC",

"dependencies": {

"cors": "^2.8.5",

"date-fns": "^4.1.0",

"express": "^4.21.2",

"uuid": "^11.1.0"

},

"devDependencies": {

"nodemon": "^3.1.9"

}

}
```

#### logEvents.js

```js
const { format } = require('date-fns');
const { v4: uuid } = require('uuid');
const fs = require('fs');
const fsPromises = require('fs/promises');
const path = require('path');


const logEvents = async (message, logName) => {

	const dateTime = `${format(new Date(), 'yyyyMMdd\tHH:mm:ss')}`;
	const logItem = `${dateTime}\t${uuid()}\t${message}\n`;
	console.log(logItem);
	
	try {
	
		if(!fs.existsSync(path.join(__dirname , '..', 'logs'))) {
			await fsPromises.mkdir(path.join(__dirname , '..', 'logs'));
		}
	
		await fsPromises.appendFile(path.join(__dirname, '..', 'logs', logName), logItem);
	} catch(err) {
		console.error(err);
	}

}

const logger = (req, res, next) => {
	logEvents(`${req.method}\t${req.headers.origin}\t${req.url}` , 'reqLog.txt');
	console.log(`${req.method} ${req.path}`);
	next();
}


module.exports = { logger, logEvents};
```

#### errorHandler.js

```js
const { logEvents } = require('./logEvents');

const errorHandler = (err, req, res, next) => {
	logEvents(`${err.name} : ${err.message}`, 'errLog.txt');
	console.error(err.stack);
	res.status(500).send(err.message);
}

module.exports = errorHandler;
```

