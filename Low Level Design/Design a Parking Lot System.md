
## Requirements

1. The parking lot should have multiple levels, each level with a certain number of parking spots.
2. The parking lot should support different types of vehicles, such as cars, motorcycles, and trucks.
3. Each parking spot should be able to accommodate a specific type of vehicle.
4. The system should assign a parking spot to a vehicle upon entry and release it when the vehicle exits.
5. The system should track the availability of parking spots and provide real-time information to customers.
6. The system should handle multiple entry and exit points and support concurrent access.

## Classes, Interfaces and Enumerations

1. The **ParkingLot** class follows the Singleton pattern to ensure only one instance of the parking lot exists. It maintains a list of levels and provides methods to park and unpark vehicles.

```
public class ParkingLot {
	private static ParkingLot parkingLot;
	private List<Level> levels;
	
	private ParkingLot() {
		this.levels = new ArrayList<>();
	}

	public static synchronized ParkingLot getInstance() {
		if(instance == null) {
			instance = new ParkingLot();
		}
		return instance;
	}

	public void addLevel(Level level) {
		levels.add(level);
	}

	public boolean parkVehicle(Vehicle vehicle) {
		for(Level level : levels) {
			if(level.parkVehicle(vehicle)) {
				System.out.println("Vehicle parked successfully");
				return true;
			}
		}
		System.out.println("Could not park vehicle.");
		return false;
	}

	public boolean unparkVehicle(Vehicle vehicle) {
		for(Level level : levels) {
			if(level.unparkVehicle(vehicle)) {
				return true;
			}
		}
		return false;
	}

	public void displayAvailability() {
		for(Level level : levels) {
			level.displayAvailability();
		}
	}
}
```

2. The **Level** class represents a level in the parking lot and contains a list of parking spots. It handles parking and unparking of vehicles within the level.

```
public class Level {
	private final int floor;
	private final List<ParkingSpot> spots;

	public Level(int floor, int numSpots) {
		this.floor = floor;
	    parkingSpots = new ArrayList<>(numSpots);
	    
		// Assign spots in ration of 50:40:10 for bikes, cars and trucks
        double spotsForBikes = 0.50;
        double spotsForCars = 0.40;

        int numBikes = (int) (numSpots * spotsForBikes);
        int numCars = (int) (numSpots * spotsForCars);

        for (int i = 1; i <= numBikes; i++) {
            parkingSpots.add(new ParkingSpot(i,VehicleType.MOTORCYCLE));
        }
        for (int i = numBikes + 1; i <= numBikes + numCars; i++) {
            parkingSpots.add(new ParkingSpot(i,VehicleType.CAR));
        }
        for (int i = numBikes + numCars + 1; i <= numSpots; i++) {
            parkingSpots.add(new ParkingSpot(i,VehicleType.TRUCK));
        }
	}

	public void displayAvailability() {
		System.out.println("Level " + floor + " Availability:");
        for (ParkingSpot spot : parkingSpots) {
            System.out.println("Spot " + spot.getSpotNumber() + ": " + (spot.isAvailable() ? "Available For"  : "Occupied By ")+" "+spot.getVehicleType());
        }
	}

	public synchronized boolean parkVehicle(Vehicle vehicle) {
		 for (ParkingSpot spot : parkingSpots) {
            if (spot.isAvailable() && spot.getVehicleType() == vehicle.getType()) {
                spot.parkVehicle(vehicle);
                return true;
            }
        }
        return false;
	}

	public synchronized boolean unparkVehicle(Vehicle vehicle) {
		for (ParkingSpot spot : parkingSpots) {
            if (!spot.isAvailable() && spot.getParkedVehicle().equals(vehicle)) {
                spot.unparkVehicle();
                return true;
            }
        }
        return false;
	}

}
```


3. The **ParkingSpot** class represents an individual parking spot and tracks the availability and the parked vehicle

```
public class ParkingSpot {
   private VehicleType vehicleType;
   private int spotNumber;
   private Vehicle parkedVehicle;

   public ParkingSpot(int spotNumber, VehicleType vehicleType) {
        this.spotNumber = spotNumber;
        this.vehicleType = vehicleType;
   }

	public synchronized boolean isAvailable() {
        return parkedVehicle == null;
    }

    public synchronized void parkVehicle(Vehicle vehicle) {
        if (isAvailable() && vehicle.getType() == vehicleType) {
            parkedVehicle = vehicle;
        } else {
            throw new IllegalArgumentException("Invalid vehicle type or spot already occupied.");
        }
    }

    public synchronized void unparkVehicle() {
        parkedVehicle = null;
    }

    public VehicleType getVehicleType() {
        return vehicleType;
    }

    public Vehicle getParkedVehicle() {
        return parkedVehicle;
    }

    public int getSpotNumber() {
        return spotNumber;
    }
}
```

4. The **VehicleType** enum defines the different types of vehicles supported by the parking lot.

```
public enum VehicleType {
    CAR,
    MOTORCYCLE,
    TRUCK
}
```

5. The **Vehicle** class is an abstract base class for different types of vehicles. It is extended by Car, Motorcycle, and Truck classes.

```
public abstract class Vehicle {
    protected String licensePlate;
    protected VehicleType type;

    public Vehicle(String licensePlate, VehicleType type) {
        this.licensePlate = licensePlate;
        this.type = type;
    }

    public VehicleType getType() {
        return type;
    }
}
```

Car

```
public class Car extends Vehicle {
    public Car(String licensePlate) {
        super(licensePlate, VehicleType.CAR);
    }
}
```

Motorcycle

```
public class Motorcycle extends Vehicle {
    public Motorcycle(String licensePlate) {
        super(licensePlate, VehicleType.MOTORCYCLE);
    }
}
```


Truck

```
public class Truck extends Vehicle {
    public Truck(String licensePlate) {
        super(licensePlate, VehicleType.TRUCK);
    }
}
```


6. The **Main** class demonstrates the usage of the parking lot system.

```
public class ParkingLotDemo {
    public static void run() {
        ParkingLot parkingLot = ParkingLot.getInstance();
        parkingLot.addLevel(new Level(1, 100));
        parkingLot.addLevel(new Level(2, 80));

        Vehicle car = new Car("ABC123");
        Vehicle truck = new Truck("XYZ789");
        Vehicle motorcycle = new Motorcycle("M1234");

        // Park vehicles
        parkingLot.parkVehicle(car);
        parkingLot.parkVehicle(truck);
        parkingLot.parkVehicle(motorcycle);

        // Display availability
        parkingLot.displayAvailability();

        // Unpark vehicle
        parkingLot.unparkVehicle(motorcycle);

        // Display updated availability
        parkingLot.displayAvailability();
    }
}
```


