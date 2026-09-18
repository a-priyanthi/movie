# Movie Ticket Booking System

## Overview

The **Movie Ticket Booking System** is a console-based application developed in **C**. It allows users to view available movies and show timings, book multiple movie tickets, select seats, view booking details, cancel bookings, and view all active bookings.

The system uses **structures, arrays, functions, loops, conditional statements, and string handling** to manage movie and booking information.

## Features

* View available movies and ticket prices
* View available show timings
* Book one or multiple tickets in a single booking
* Select multiple seats
* Prevent booking of already occupied seats
* Prevent selecting the same seat twice in one booking
* Generate a unique Booking ID
* Store customer name and phone number
* Calculate the total ticket amount automatically
* View individual booking details
* Cancel an entire booking using the Booking ID
* View all active bookings
* Maintain separate seat availability for each movie and show

## Movies

The system currently contains the following movies:

| Movie             | Ticket Price |
| ----------------- | -----------: |
| Interstellar      |      Rs. 250 |
| Avengers: Endgame |      Rs. 220 |
| Leo               |      Rs. 200 |

## Show Timings

The available show timings are:

| Show ID | Timing   |
| ------- | -------- |
| 1       | 10:00 AM |
| 2       | 02:00 PM |
| 3       | 07:00 PM |

## Seat Management

The system provides **30 seats** for each movie-show combination.

* `[01]`, `[02]`, etc. indicate available seats.
* `[ X ]` indicates a booked seat.
* Seats are tracked separately for each movie and show.
* A seat booked for one movie/show can still be booked for another movie/show.

## Multiple Ticket Booking

Users can book multiple tickets in a single transaction.

For example:

```text
Enter number of tickets: 4

Enter seat number 1: 1
Enter seat number 2: 2
Enter seat number 3: 3
Enter seat number 4: 4
```

The system creates one booking containing all four seats and calculates the total amount automatically.

```text
Tickets      : 4
Seats        : 1 2 3 4
Price/Ticket : Rs. 250.00
Total Amount : Rs. 1000.00
```

## Menu Options

```text
1. View Movies
2. View Show Timings
3. Book Ticket
4. View Booking
5. Cancel Booking
6. View All Bookings
7. Exit
```

### 1. View Movies

Displays all available movies along with their IDs and ticket prices.

### 2. View Show Timings

Displays all available show timings.

### 3. Book Ticket

The user selects:

1. Movie
2. Show timing
3. Number of tickets
4. Seats
5. Customer name
6. Phone number

A Booking ID is then generated and the total amount is calculated.

### 4. View Booking

The user enters a Booking ID to view:

* Booking ID
* Customer name
* Phone number
* Movie
* Show timing
* Number of tickets
* Selected seats
* Total amount

### 5. Cancel Booking

The user enters a Booking ID. The complete booking is cancelled, making all the seats in that booking available again.

### 6. View All Bookings

Displays all currently active bookings in a tabular format.

### 7. Exit

Terminates the program.

## Data Structures Used

### Movie Structure

```c
struct Movie
{
    int id;
    char name[50];
    float price;
};
```

Stores movie ID, movie name, and ticket price.

### Show Structure

```c
struct Show
{
    int id;
    char time[20];
};
```

Stores show ID and show timing.

### Booking Structure

```c
struct Booking
{
    int bookingID;
    char customerName[50];
    char phone[15];

    int movieID;
    int showID;

    int seatNo[MAX_SEATS];
    int ticketCount;

    float amount;
    int active;
};
```

Stores complete booking information. The `seatNo` array allows multiple seats to be stored under a single booking.

## Important Functions

| Function            | Purpose                            |
| ------------------- | ---------------------------------- |
| `displayHeader()`   | Displays the system heading        |
| `displayMovies()`   | Displays available movies          |
| `displayShows()`    | Displays show timings              |
| `displaySeats()`    | Displays seat availability         |
| `isSeatAvailable()` | Checks whether a seat is available |
| `getMovieIndex()`   | Finds a movie using its ID         |
| `getShowIndex()`    | Finds a show using its ID          |
| `bookTicket()`      | Books one or multiple tickets      |
| `viewBooking()`     | Displays a particular booking      |
| `cancelBooking()`   | Cancels a booking                  |
| `viewAllBookings()` | Displays all active bookings       |


## Constants

The program uses the following limits:

```c
#define MAX_MOVIES 3
#define MAX_SHOWS 3
#define MAX_SEATS 30
#define MAX_BOOKINGS 100
```

This means the system supports:

* 3 movies
* 3 show timings
* 30 seats per movie-show combination
* Up to 100 booking records

## Technologies Used

* **Programming Language:** C
* **Compiler:** GCC / MinGW / Turbo C / any standard C compiler
* **Interface:** Command Line / Console

## How to Run

### Compile

Using GCC:

```bash
gcc movie_booking.c -o movie_booking
```

### Run

On Windows:

```bash
movie_booking.exe
```

On Linux/macOS:

```bash
./movie_booking
```

## Sample Booking

```text
Movie        : Interstellar
Show         : 07:00 PM
Customer     : Priya
Tickets      : 4
Seats        : 1 2 3 4
Price/Ticket : Rs. 250.00
Total Amount : Rs. 1000.00
```

## Limitations

* Data is stored only during program execution.
* Bookings are lost when the program is closed.
* The system does not currently include online payment.
* Movie and show information is predefined in the source code.
* There is no login or administrator module.

## Future Enhancements

The project can be extended with:

* File handling for permanent booking storage
* Login and registration
* Admin dashboard
* Online payment simulation
* Different seat categories such as Premium and Regular
* Different prices for different show timings
* Food and beverage booking
* Automatic ticket generation
* Booking history
* Date-wise show scheduling

## Author

**Movie Ticket Booking System**

Developed as a C programming project demonstrating structures, arrays, functions, and menu-driven programming.

