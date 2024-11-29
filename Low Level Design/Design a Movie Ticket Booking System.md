

## Requirements

1. The system should allow users to view the list of movies playing in different theaters.
2. Users should be able to select a movie, theater, and show timing to book tickets.
3. The system should display the seating arrangement of the selected show and allow users to choose seats.
4. Users should be able to make payments and confirm their booking.
5. The system should handle concurrent bookings and ensure seat availability is updated in real-time.
6. The system should support different types of seats (e.g., normal, premium) and pricing.
7. The system should allow theater administrators to add, update, and remove movies, shows, and seating arrangements.
8. The system should be scalable to handle a large number of concurrent users and bookings.


## Classes, Interfaces and Enumerations

1. The **Movie** class represents a movie with properties such as ID, title, description, and duration.
```
public class Movie {
	private final Long id;
	private final String title;
	private final String description;
	private final int durationInMinutes;

	public Movie(Long id, String title, String description, int durationInMinutes) {
		this.id = id;
		this.title = title;
		this.description = description;
		this.durationInMinutes = durationInMinutes;
	}

	public int getDurationInMinutes() {
		return durationInMinutes;
	}

}
```

2. The **Theater** class represents a theater with properties such as ID, name, location, and a list of shows.

```
public class Theater {
	private final Long id;
	private final String name;
	private final String location;
	private final List<Show> shows;

	public Theater(Long id , String name, String location, List<Show> shows) {
		this.id = id;
		this.name = name;
		this.location = location;
		this.shows = shows;
	}

	// Getters
}
```


3. The **Show** class represents a movie show in a theater, with properties such as ID, movie, theater, start time, end time, and a map of seats.

```
public class Show {
	private final Long id;
	private final Movie movie;
	private final Theater theater;
	private final LocalDateTime startTime;
	private final LocalDateTime endTime;
	private final Map<String, Seat> seats;

	public Show(Long id , Movie movie, Theater theater, Long startTime, Long endTime, Map<String, Seat> seats) {
		this.id = id;
		this.movie = movie;
		this.theater = theater;
		this.startTime = startTime;
		this.endTime = endTime;
		this.seats = seats;
	}

	// Getters
}
```


4. The **Seat** class represents a seat in a show, with properties such as ID, row, column, type, price, and status.

```
public class Seat {
	private final Long id;
	private final int row;
	private final int column;
	private final SeatType seatType;
	private final double price;
	private SeatStatus status;

	public Seat(Long id, int row, int column, SeatType type, double price, SeatStatus status) {
		this.id = id;
		this.row = row;
		this.column = column;
		this.type = type;
		this.price = price;
		this.status = status;
	}

	public void setStatus(SeatStatus status) {
		this.status = status;
	}

	// Getters
}
```


5. The **SeatType** enum defines the different types of seats (normal or premium).

```
public enum SeatType {
	NORMAL, PREMIUM
}
```

6. The **SeatStatus** enum defines the different statuses of a seat (available or booked).

```
public enum SeatStatus {
	AVAILABLE , BOOKED
}
```


7. The **Booking** class represents a booking made by a user, with properties such as ID, user, show, selected seats, total price, and status.

```
public class Booking {
	private final Long id;
	private final User user;
	private final Show show;
	private final List<Seat> selectedSeats;
	private final double totalPrice;
	private BookingStatus status;

	public Booking(Long id, User user, Show show, List<Seat> selectedSeats, double totalPrice, BookingStatus status) {
		this.id = id;
		this.user = user;
		this.show = show;
		this.selectedSeats = selectedSeats;
		this.totalPrice = totalPrice;
		this.status = status;
	}

	public void setBookingStatus(BookingStatus status) {
		this.status = status;
	}

	// Getters
	
}
```


8. The **BookingStatus** enum defines the different statuses of a booking (pending, confirmed, or cancelled).

```
public enum BookingStatus {
	PENDING, CONFIRMED, CANCELLED
}
```

9. The **User** class represents a user of the booking system, with properties such as ID, name, and email.

```
public class User {
	private final Long id;
	private final String name;
	private final String email;

	public User(Long id, String name, String email) {
		this.id = id;
		this.name = name;
		this.email = email;
	}

	// Getters and Setters
}
```

10. The **MovieTicketBookingSystem** class is the main class that manages the movie ticket booking system. It follows the Singleton pattern to ensure only one instance of the system exists.
    The MovieTicketBookingSystem class provides methods for adding movies, theaters, and shows, as well as booking tickets, confirming bookings, and cancelling bookings.
    Multi-threading is achieved using concurrent data structures such as ConcurrentHashMap to handle concurrent access to shared resources like shows and bookings.

```
public class MovieTicketBookingSystem {

	private static MovieTicketBookingSystem instance;
	private final List<Movie> movies;
	private final List<Theater> theaters;
	private final Map<String, Show> shows;
	private final Map<String, Booking> bookings;

	 private static final String BOOKING_ID_PREFIX = "BKG";
	 private static final AtomicLong bookingCounter = new AtomicLong(0);

	private MovieTicketBookingSystem() {
		movies = new ArrayList<>();
		theaters = new ArrayList<>();
		show = new ConcurrentHashMap<>();
		bookings = new ConcurrentHashMap<>();
	}

	public static synchronized MovieTicketBookingSystem getInstance() {
		if(instance == null) {
			instance = new MovieTicketBookingSystem();
		}
		return instance;
	}

	public void addMovie(Movie movie) {
		this.movies.add(movie);
	}

	public void addTheaters(Theater theater) {
		this.theaters.add(theater);
	}

	public void addShow(Show show) {
		shows.put(show.getId(), show);
	}

	 public List<Movie> getMovies() {
        return movies;
    }

    public List<Theater> getTheaters() {
        return theaters;
    }

    public Show getShow(String showId) {
        return shows.get(showId);
    }

	public synchronized Booking bookTickets(User user, Show show, List<Seat> selectedSeats) {
	    if(areSeatsAvailable(show, selectedSeats)) {
		    markSeatsAsBooked(show, selectedSeats);
		    double totalPrice = calculateTotalPrice(selectedSeats);
		    Long bookingId = generateBookingId();
		    Booking booking = new Booking(bookingId, user, show, selectedSeats, totalPrice, BookingStatus.PENDING);
		    bookings.put(bookingId, booking);
		    return booking;
	    }
		return null;
	}

	private boolean areSeatsAvailable(Show show, List<Seat> selectedSeats) {
		Map<String, Seat> showSeats = show.getSeats();
		
		for(Seat seat : selectedSeats) {
			Seat showSeat = showSeats.get(seat.getId());
			// showSeat can be null if the user pass wrong id.
				if(showSeat == null || showSeat.getStatus() != SeatStatus.AVAILABLE) {
				return false;
			}
		}
		return true;
	}

	private void markSeatsAsBooked(Show show, List<Seat> selectedSeats) {
		Map<String, Seat> showSeats = show.getSeats();
		for(Seat seat : selectedSeats) {
			Seat showSeat = showSeats.get(seat.getId());
			showSeat.setStatus(SeatStatus.BOOKED);
		}
	}

	private double calculateTotalPrice(List<Seat> selectedSeats) {
		return selectedSeats.stream().mapToDouble(Seat::getPrice).sum();
	}


	private Long generateBookingId() {
		return bookingCounter.incrementAndGet();
	}

	public synchronized void confirmBooking(String bookingId) {
		Booking booking = bookings.get(bookingId);
		if(booking != null && booking.getStatus() == BookingStatus.PENDING) {
			booking.setStatus(BookingStatus.CONFIRMED);
			// Process payment and send confirmation
			// ...
		}
	}

	public synchronized void cancelBooking(String bookingId) {
		Booking booking = bookings.get(bookingId);
		if(booking != null && booking.getStatus() != Booking.CANCELLED) {
			booking.setStatus(BookingStatus.CANCELLED);
			markSeatsAsAvailable(booking.getShow(), booking.getSelectedSeats());
			// Process refund and send cancellation notification
			// ....
		}
	}
	
	private void markSeatsAsAvailable(Show show, List<Seats> seats) {
		Map<String, Seat> showSeats = show.getSeats();
		for(Seat seat : selectedSeats) {
			Seat showSeat = showSeats.get(seat.getId());
			showSeat.setStatus(SeatStatus.AVAILABLE);
		}
	}

}
```

11. The **MovieTicketBookingDemo** class demonstrates the usage of the movie ticket booking system by adding movies, theaters, shows, booking tickets, and confirming or cancelling bookings.

```
public class MovieTicketBookingDemo {

	public static void main(String[] args) {
		MovieTicketBookingSystem bookingSystem = MovieTicketBookingSystem.getInstance();

		// Add movies
		Movie movie1 = new Movie("M1", "Movie 1", "Description 1", 120);
		Movie movie2 = new Movie("M2", "Movie 2", "Description 2", 135);
		bookingSystem.addMovie(movie1);
		bookingSystem.addMovie(movie2);

		// Add theaters
		Theater theater1 = new Theater("T1", "Theater 1", "Location 1", new ArrayList<>());
		Theater theater2 = new Theater("T2", "Theater 2", "Location 2", new ArrayList<>());
		bookingSystem.addTheater(theater1);
		bookingSystem.addTheater(theater2);

		// Add Shows
		  Show show1 = new Show("S1", movie1, theater1, LocalDateTime.now(), LocalDateTime.now().plusMinutes(movie1.getDurationInMinutes()), createSeats(10, 10));
        Show show2 = new Show("S2", movie2, theater2, LocalDateTime.now(), LocalDateTime.now().plusMinutes(movie2.getDurationInMinutes()), createSeats(8, 8));
        bookingSystem.addShow(show1);
        bookingSystem.addShow(show2);


		  // Book tickets
        User user = new User("U1", "John Doe", "john@example.com");
        List<Seat> selectedSeats = Arrays.asList(show1.getSeats().get("1-5"), show1.getSeats().get("1-6"));
        Booking booking = bookingSystem.bookTickets(user, show1, selectedSeats);
        if (booking != null) {
            System.out.println("Booking successful. Booking ID: " + booking.getId());
            bookingSystem.confirmBooking(booking.getId());
        } else {
            System.out.println("Booking failed. Seats not available.");
        }

        // Cancel booking
        bookingSystem.cancelBooking(booking.getId());
        System.out.println("Booking canceled. Booking ID: " + booking.getId());
	}

	private static Map<String, Seat> createSeats(int rows, int columns) {
		Map<String, Seat> seats = new HashMap<>();
		for(int row = 1, row <= rows; row++) {
			for(int col = 1, col <= columns; col++) {
				String seatId = row + "-" + col;
				SeatType seatType = (row <= 2) ? SeatType.PREMIUM : SeatType.NORMAL;
				double price = (seatType == SeatType.PREMIUM) ? 150.0 : 100.0;
				Seat seat = new Seat(seatId, row, col, seatType, price, SeatStatus.AVAILABLE);
				seats.put(seatId, seat);
			}
		}
		return seats;
	}
}
```

