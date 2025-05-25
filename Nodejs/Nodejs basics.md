
### How NodeJS differs from Vanilla JS

1. Node runs on a server - not in a browser (backend not frontend)
2. The console is the terminal window
3. global object instead of window object.
4. Has Common Core modules that we will explore.
5. CommonJS modules instead of ES6 modules.
6. Missing some JS APIs like fetch

```node
console.log('Hello World');

console.log(global) // global object

const os = require('os');
const path = require('path');

console.log(os.type())
console.log(os.version())
console.log(os.homedir())

console.log(__dirname);
console.log(__filename);

console.log(path.dirname(__filename));
console.log(path.basename(__filename));
console.log(path.extname(__filename));

console.log(path.parse(__filename));
```

```node
Output

Hello World

<ref *1> Object [global] {
  global: [Circular *1],
  clearInterval: [Function: clearInterval],
  clearTimeout: [Function: clearTimeout],
  setInterval: [Function: setInterval],
  setTimeout: [Function: setTimeout] {
    [Symbol(nodejs.util.promisify.custom)]: [Getter]
  },
  queueMicrotask: [Function: queueMicrotask],
  performance: Performance {
    nodeTiming: PerformanceNodeTiming {
      name: 'node',
      entryType: 'node',
      startTime: 0,
      duration: 40.442875027656555,
      nodeStart: 2.4117079973220825,
      v8Start: 5.819875001907349,
      bootstrapComplete: 30.549833059310913,
      environment: 16.28166699409485,
      loopStart: -1,
      loopExit: -1,
      idleTime: 0
    },
    timeOrigin: 1740760641104.567
  },
  clearImmediate: [Function: clearImmediate],
  setImmediate: [Function: setImmediate] {
    [Symbol(nodejs.util.promisify.custom)]: [Getter]
  }
}

Darwin
Darwin Kernel Version 23.0.0: Fri Sep 15 14:43:05 PDT 2023; root:xnu-10002.1.13~1/RELEASE_ARM64_T6020
/Users/sanjay

/Users/sanjay/Documents/Sanjay/Nodejs/Beginner Course Dave Gray
/Users/sanjay/Documents/Sanjay/Nodejs/Beginner Course Dave Gray/server.js

/Users/sanjay/Documents/Sanjay/Nodejs/Beginner Course Dave Gray
server.js
.js

{
  root: '/',
  dir: '/Users/sanjay/Documents/Sanjay/Nodejs/Beginner Course Dave Gray',
  base: 'server.js',
  ext: '.js',
  name: 'server'
}
```

### Create your own module

math.js

```node
const add = a => b => a + b;
const sub = a => b => a - b;
const mul = a => b => a * b;
const divide = a => b => a / b;

module.exports = { add , sub, mul, divide }
```

```node
const {add , sub, mul, divide} = require('./math');

console.log(add(2)(5));
console.log(sub(5)(2));
console.log(mul(5)(2));
console.log(divide(5)(2));

Output

7
3
10
2.5
```

Math.js can be rewritten like this as well.

```node
exports.add = a => b => a + b;
exports.sub = a => b => a - b;
exports.mul = a => b => a * b;
exports.divide = a => b => a / b;

//module.exports = { add , sub, mul, divide }
```

#### fs module

```node
const fs = require('fs');
const path = require('path');

fs.readFile('files/starter.txt', (err, data) => {
	if(err) throw err;
	console.log(data.toString());
});

  

fs.readFile('files/starter.txt', 'utf8', (err, data) => {
	if(err) throw err;
	console.log(data);
});

console.log(__dirname);

fs.readFile(path.join(__dirname, 'files', 'starter.txt'), (err, data) => {
	if(err) throw err;
	console.log(data.toString());
});


fs.writeFile(path.join(__dirname, 'files', 'reply.txt'), 'Nice to meet you.', (err) => {
	if (err) throw err;
	console.log('Write complete');

	fs.appendFile(path.join(__dirname, 'files', 'reply.txt'), '\n\nYes it is.', (err) => {
	
	if (err) throw err;
	console.log('Append complete');

	fs.rename(path.join(__dirname, 'files', 'reply.txt'), path.join(__dirname, 'files', 'newReply.txt'), (err) => {
	
		if (err) throw err;
		console.log('Rename complete');

	})

})

})


process.on('uncaughtException', err => {
	console.error(`There was an uncaught error: ${err}`);
	process.exit(1);
})
```

### fs promises

```js
const fsPromises = require('fs/promises');
const path = require('path');

const fileOps = async () => {

	try {
		const data = await fsPromises.readFile(path.join(__dirname, 'files', 'starter.txt'), 'utf8')
		console.log(data);
		await fsPromises.unlink(path.join(__dirname, 'files', 'starter.txt'));
		await fsPromises.writeFile(path.join(__dirname, 'files', 'promiseWrite.txt'), data);
		await fsPromises.appendFile(path.join(__dirname, 'files', 'promiseWrite.txt'), '\n\nNice to meet you.');
		await fsPromises.rename(path.join(__dirname, 'files', 'promiseWrite.txt'), path.join(__dirname, 'files', 'promiseComplete.txt'));
		const newData = await fsPromises.readFile(path.join(__dirname, 'files', 'promiseComplete.txt'), 'utf8')
		console.log(newData);
	} catch(err) {
		console.error(err);
	}
}

fileOps();
```

### Read Stream and Write Stream

```js
const fs = require('fs');

const rs = fs.createReadStream(path.join(__dirname, 'files', 'promiseComplete.txt') , {encoding : 'utf8'});

const ws = fs.createWriteStream(path.join(__dirname, 'files', 'usingPipe.txt'));

// rs.on('data' , (dataChuck) => {

// ws.write(dataChuck);

// });
rs.pipe(ws);

```

### Directory  operations

```js

const fs = require('fs');

if(!fs.existsSync('./new')) {
	fs.mkdir('./new', (err) => {
	if(err) throw err;
		console.log('Directory Created');
	})
}

if(fs.existsSync('./new')) {
	fs.rmdir('./new', (err) => {
	if(err) throw err;
		console.log('Directory Removed');
	})
}
```

### nodemon module

```shell
npm i nodemon -g
```

Now, to run any file using node. You don't need to use node, instead use nodemon 'filename'

### Command for creating  package.json file

```js
npm init
```

### To install a package or dependency

```shell
npm i data-fns
```

### Install dependency as a devDependency

```node
npm i nodemon -D
```

#### Scripts in package.json

```node
"scripts": {
	"dev": "nodemon",
	"start": "node index.js"
}
```

#### To run the dev script

```node
npm run dev
```

### version number in dependencies

```js
"date-fns": "^4.1.0"

major-version.minor-version.patch-version

^4 - don't update the major version.
1 - can update the minor version.
0 - can update the patch version.
```

#### To remove the dependency

```js
npm rm nodemon -D
```

### Event Emitter

logEvents.js

```js
const { format } = require('date-fns');
const { v4: uuid } = require('uuid');


const fs = require('fs');
const fsPromises = require('fs/promises');
const path = require('path');


const logEvents = async (message) => {
const dateTime = `${format(new Date(), 'yyyyMMdd\tHH:mm:ss')}`;
const logItem = `${dateTime}\t${uuid()}\t${message}\n`;
console.log(logItem);

try {
	if(!fs.existsSync(path.join(__dirname , 'logs'))) {
		await fsPromises.mkdir(path.join(__dirname , 'logs'));
	}
	await fsPromises.appendFile(path.join(__dirname, 'logs', 'eventLog.txt'), logItem);
} catch(err) {
	console.error(err);
}
}
module.exports = logEvents;
```

index.js

```js
const logEvents = require('./logEvents.js');
const EventEmitter = require('events');

class MyEmitter extends EventEmitter {};

const myEmitter = new MyEmitter();

myEmitter.on('log', (msg) => logEvents(msg));

setTimeout(() => {
	// Emit event
	myEmitter.emit('log' , 'Log event emitted!')
}, 2000);
```

#### JSON.parse vs JSON.stringify

- **Functionality**:
    
    - `JSON.parse()` **parses** a string and converts it into an object.
    - `JSON.stringify()` **stringifies** an object and converts it into a string.
- **Input/Output**:
    
    - `JSON.parse()` takes a JSON-formatted string and outputs a JavaScript object.
    - `JSON.stringify()` takes a JavaScript object and outputs a JSON-formatted string.
### References

https://www.youtube.com/watch?v=f2EqECiTBL8&t=322s