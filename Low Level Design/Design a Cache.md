
### Requirements

We have to do low level design for a Cache system. Cache that we will design will have to support following operations:

- **Put**: This will allow user to put a value against a key in the cache.
- **Get**: This will allow user to get the previously saved value using key.
- **Eviction**: Cache should also support removal of some key in case cache is full, and we try to add new key value.

### Classes , Interfaces and Enum

1. Create a Storage Interface, which will define the operations.

```java
public interface Storage<Key, Value> {
	void add(Key key, Value value) throws StorageFullException;

	void remove(Key key) throws NotFoundException;

	Value get(Key key) throws NotFoundException;
}
```

2. Create a HashMapBasedStorage class, which is the implementation of Storage Interface.

```java
public class HashMapBasedStorage<Key, Value> implements Storage<Key, Value> {

	private Map<Key, Value> storage;
	private final Integer capacity;

	public HashMapBasedStorage(Integer capacity) {
		this.capacity = capacity;
		storage = new HashMap<>();
	}

	@Override
	public void add(Key key, Value value) throws StorageFullException {
		if(isStorageFull()) throw new StorageFullException("Capacity Full....");
		storage.put(key, value);
	}

	@Override
	public void remove(Key key) throws NotFoundException {
		if(!storage.containsKey(key)) throw new NotFoundException(key + "doesn't exist in cache");
		storage.remove(key);
	}

	@Override
	public Value get(Key key) throws NotFoundException {
		if(!storage.containsKey(key)) throw new NotFoundException(key + "doesn't exist in cache");
		return storage.get(key);
	}

	private boolean isStorageFull() {
		return storage.size() == capacity;
	}
}
```


3. Create exception classes NotFoundException and StorageFullException

```java
public class NotFoundException extends RuntimeException {

	public NotFoundException(final String message) {
		super(message);
	}
}
```

```java
public class StorageFullException extends RuntimeException {

	public StorageFullException(String message) {
		super(message);
	}
}
```

4. Create EvictionPolicy interface 

```java
public interface EvictionPolicy<Key> {
	void keyAccessed(Key key);

	Key evictKey();
}
```

5. Create an implementation class LRUEvictionPolicy.

```java
public class LRUEvictionPolicy<Key> implements EvictionPolicy<Key> {

	private DoublyLinkedList<Key> dll;
	private Map<Key, DoublyLinkedListNode<Key>> mapper;

	public LRUEvictionPolicy() {
		this.dll = new DoublyLinkedList<>();
		this.mapper = new HashMap<>();
	}

	@Override
	public void keyAccessed(Key key) {
		if(mapper.containsKey(key)) {
			ddl.detachNode(mapper.get(key));
			ddl.addNodeAtLast(mapper.get(key));
		} else {
			DoublyLinkedListNode<Key> newNode = ddl.addElementAtLast(key);
			mapper.put(key, newNode);
		}
	}


	@Override
	public Key evictKey() {
		DoublyLinkedListNode<Key> first = dll.getFirstNode();
		if(first == null) {
			return null;
		}

		dll.detachNode(first);
		return first.getElement();
	}
}
```


6. DoublyLinkedList and DoublyLinkedListNode Implementation

```java
public class DoublyLinkedList<E> {

	DoublyLinkedListNode<E> dummyHead;
	DoublyLinkedListNode<E> dummyTail;

	public DoublyLinkedList() {
		dummyHead = new DoublyLinkedListNode<>(null);
		dummyTail = new DoublyLinkedListNode<>(null);

		dummyHead.next = dummyTail;
		dummyTail.prev = dummyHead;
	}


	public void detachNode(DoublyLinkedListNode<E> node) {
		if(node != null) {
			node.prev.next = node.next;
			node.next.prev = node.prev;
		}
	}

	public void addNodeAtLast(DoublyLinkedListNode<E> node) {
		DoublyLinkedListNode tailPrev = dummyTail.prev;
		tailPrev.next = node;
		node.next = dummyTail;
		dummyTail.prev = node;
		node.prev = tailPrev;
	}

	public DoublyLinkedListNode<E> addElementAtLast(E element) {
		if(element == null) {
			throw new InvalidElementException();
		}
		DoublyLinkedListNode<E> newNode = new DoublyLinkedListNode<>(element);
		addNodeAtLast(newNode);
		return newNode;
	}

	public boolean isItemPresent() {
		return dummyHead.next != dummyTail;
	}

	public DoublyLinkedListNode getFirstNode() throws NoSuchElementException {
		if(!isItemPresent()) {
			return null;
		}
		return dummyHead.next;
	}

	public DoublyLinkedListNode getLastNode() throws NoSuchElementException {
		if(!isItemPresent()) {
			return null;
		}
		return dummyTail.prev;
	}
}
```

DoublyLinkedListNode class

```java
@Getter
public class DoublyLinkedListNode<E> {

	DoublyLinkedListNode<E> next;
	DoublyLinkedListNode<E> prev;
	E element;

	public DoublyLinkedListNode(E element) {
		this.element = element;
		this.next = null;
		this.prev = null;
	}
}
```

7. InvalidNodeException and InvalidElementException class  

```java
public class InvalidNodeException extends RuntimeException {}
```

```java
public class InvalidElementException extends RuntimeException {}
```

8.  Cache class Implementation

```java
public class Cache<Key, Value> {
	private final EvictionPolicy<Key> evictionPolicy;
	private final Storage<Key, Value> storage;

	public Cache(EvictionPolicy<Key> evictionPolicy, Storage<Key, Value> storage) {
		this.evictionPolicy = evictionPolicy;
		this.storage = storage;
	}

	public void put(Key key, Value value) {
		try {
			this.storage.add(key,value);
			this.evictionPolicy.keyAccessed(key);
		} catch(StorageFullException exception) {
			System.out.println("Got storage full. Will try to evict.");
			Key keyToRemove = this.evictionPolicy.evictKey();
			if(keyToRemove == null) {
				throw new RuntimeException("Unexpected State. Storage full and no key to evict.")
			}
			this.storage.remove(keyToRemove);
			System.out.println("Creating space by evicting item..." + keyToRemove);
			put(key,value);
		}
	}

	public Value get(Key key) {
		try {
			Value value = this.storage.get(key);
			this.evictionPolicy.keyAccessed(key);
			return value;
		} catch(NotFoundException notFoundException) {
			System.out.println("Tried to access non-existing key.");
			return null;
		}
	}
}
```

9. Cache Factory Class Implementation

```java
public class CacheFactory<Key, Value> {
	public Cache<Key, Value> defaultCache(final int capacity) {
		return new Cache<Key, Value>(new LRUEvictionPolicy<Key>(),
				new HashMapBasedStorage<Key, Value>(capacity));
	}
}
```


### References

https://github.com/anomaly2104/cache-low-level-system-design/tree/master?tab=readme-ov-file

