
```js
const extension = '.js';

// Switch statement
let contentType;

switch(extension) {

	case '.css':
		contentType = 'text/css';
		break;
	
	case '.js':
		contentType = 'text/javascript';
		break;
	
	case '.json':
		contentType = 'application/json';
		break;
	
	case '.jpg':
		contentType = 'image/jpeg';
		break;
	
	case '.png':
		contentType = 'image/png';
		break;
	
	case '.txt':
		contentType = 'text/plain';
		break;
	
	default:
		contentType = "text/html";
}

  

// For only key value pair we can use a better solution
// 1. Use object to map key value pairs

const extensionObj = {
	".css" : "text/css",
	".js" : "text/javascript",
	".json" : "application/json",
	".jpg" : "image/jpeg",
	".png" : "image/png",
	".txt" : "text/plain"
}

console.log(extensionObj[extension] || 'text/html');

  

// 2. Use Map to store key value pairs

const myMap = new Map();
myMap.set('.css' , 'text/css');
myMap.set('.js', 'text/javascript');

console.log(myMap.get(extension) || 'text/html');
```

### References

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map