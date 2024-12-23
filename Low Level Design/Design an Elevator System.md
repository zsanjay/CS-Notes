
### Requirements

1. The elevator system should consist of multiple elevators serving multiple floors.
2. Each elevator should have a capacity limit and should not exceed it.
3. Users should be able to request an elevator from any floor and select a destination floor.
4. The elevator system should efficiently handle user requests and optimize the movement of elevators to minimize waiting time.
5. The system should prioritize requests based on the direction of travel and the proximity of the elevators to the requested floor.
6. The elevators should be able to handle multiple requests concurrently and process them in an optimal order.
7. The system should ensure thread safety and prevent race conditions when multiple threads interact with the elevators.

## Classes, Interfaces and Enumerations

1. The **Direction** enum represents the possible directions of elevator movement (UP or DOWN).

```
public enum Direction {
	UP, DOWN
}
```

2. The **Request** class represents a user request for an elevator, containing the source floor and destination floor.

```
class Request {
	private final int sourceFloor;
	private final int destinationFloor;

	public Request(int sourceFloor , int destinationFloor) {
		this.sourceFloor = sourceFloor;
		this.destinationFloor = destinationFloor;
	}

	public int getSourceFloor() {
		return sourceFloor;
	}

	public int getDestinationFloor() {
		return destinationFloor;
	}
}
```

3. The **Elevator** class represents an individual elevator in the system. It has a capacity limit and maintains a list of 4. requests. The elevator processes requests concurrently and moves between floors based on the requests.

```
class Elevator {
	private final int id;
	private final int capacity;
	private int currentFloor;
	private Direction currentDirection;
	private final List<Request> requests;

	public Elevator(int id, int capacity) {
		this.id = id;
		this.capacity = capacity;
		this.currentFloor = 1;
		this.currentDirection = Direction.UP;
		this.requests = new ArrayList<>();
	}

	public synchronized void addRequest(Request request) {
		if(requests.size() < capacity) {
			requests.add(request);
			System.out.println("Elevator " + id + " added request: " + request);
			notifyAll();
		}
	}

	public synchronized Request getNextRequest() {
		while(request.isEmpty()) {
			try {
				wait();
			} catch(InterruptedException e) {
				e.printStackTrace();
			}
		}
		return requests.remove(0);
	}

	public synchronized void processRequests() {
		while(true) {
			while(!requests.isEmpty()) {
				Request request = getNextRequest();
				processRequest(request);
			}
			try {
				wait();
			} catch(InterruptedException e) {
				e.printStackTrace();
			}
		}
	}

	private void processRequest(Request request) {
		int startFloor = currentFloor;
		int endFloor = request.getDestinationFloor();

		if(startFloor < endFloor) {
			currentDirection  = Direction.UP;
			for(int i = startFloor; i <= endFloor; i++) {
				currentFloor = i;
				System.out.println("Elevator " + id + " reached floor " + currentFloor);
				try {
					Thread.sleep(1000); // Simulating elevator movement
				} catch(InterruptedException e) {
					e.printStackTrace();
				}
			}
		} else if (startFloor > endFloor) {
			currentDirection  = Direction.DOWN;
			for(int i = startFloor; i >= endFloor; i--) {
				currentFloor = i;
				System.out.println("Elevator " + id + " reached floor " + currentFloor);
				try {
					Thread.sleep(1000); // Simulating elevator movement 
				} catch(InterruptedException e) {
					e.printStackTrace();
				}
			}
		}
	}

	public void run() {
		processRequests();
	}

	public int getCurrentFloor() {
		return currentFloor;
	}
}
```

4. The **ElevatorController** class manages multiple elevators and handles user requests. It finds the optimal elevator to serve a request based on the proximity of the elevators to the requested floor.

```
import java.util.List;
import java.util.ArrayList;

public class ElevatorController {
	private final List<Elevator> elevators;

	public ElevatorController(int numElevators, int capacity) {
		this.elevators = new ArrayList<>();
		for(int i = 0; i < numElevators; i++) {
			Elevator elevator = new Elevator(i + 1, capacity);
			elevators.add(elevator);
			new Thread(elevator::run).start();
		}
	}

	public void requestElevator(int sourceFloor, int destinationFloor) {
		Elevator optimalElevator = findOptimalElevator(sourceFloor, destinationFloor);
		optimalElevator.addRequest(new Request(sourceFloor, destinationFloor));
	}

	private Elevator findOptimalElevator(int sourceFloor, int destinationFloor) {
		Elevator optimalElevator = null;
		int minDistance = Integer.MAX_VALUE;
		
		for(Elevator elevator : elevators) {
			int distance = Math.abs(sourceFloor - elevator.getCurrentFloor());
			if(minDistance > distance) {
				minDistance = distance;
				optimalElevator = elevator;
			}
		}
		return optimalElevator;
	}
}
```

5. The **ElevatorSystem** class is the entry point of the application and demonstrates the usage of the elevator system.

```
public class ElevatorSystemDemo {
	public static void main(String[] args) {
		ElevatorController controller = new ElevatorController(3, 5);

		controller.requestElevator(5, 10);
		controller.requestElevator(3, 7);
		controller.requestElevator(8, 2);
		controller.requestElevator(1, 9);
	}
}
```


