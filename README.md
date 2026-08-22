#include <stdio.h>
#include <string.h>

#define MAX_MOVIES 3
#define MAX_SHOWS 3
#define MAX_SEATS 30
#define MAX_BOOKINGS 100

/* ---------- STRUCTURES ---------- */

struct Movie
{
    int id;
    char name[50];
    float price;
};

struct Show
{
    int id;
    char time[20];
};

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

/* ---------- GLOBAL DATA ---------- */

struct Movie movies[MAX_MOVIES] =
{
    {1, "Interstellar", 250.00},
    {2, "Avengers: Endgame", 220.00},
    {3, "Leo", 200.00}
};

struct Show shows[MAX_SHOWS] =
{
    {1, "10:00 AM"},
    {2, "02:00 PM"},
    {3, "07:00 PM"}
};

struct Booking bookings[MAX_BOOKINGS];

int bookingCount = 0;

/* ---------- FUNCTIONS ---------- */

void displayHeader()
{
    printf("\n");
    printf("====================================================\n");
    printf("              MOVIE TICKET BOOKING SYSTEM           \n");
    printf("====================================================\n");
}

void displayMovies()
{
    int i;

    printf("\n---------------- AVAILABLE MOVIES ------------------\n");

    printf("%-5s %-30s %-10s\n",
           "ID", "MOVIE", "PRICE");

    printf("----------------------------------------------------\n");

    for(i = 0; i < MAX_MOVIES; i++)
    {
        printf("%-5d %-30s Rs. %.2f\n",
               movies[i].id,
               movies[i].name,
               movies[i].price);
    }
}

void displayShows()
{
    int i;

    printf("\n---------------- SHOW TIMINGS ----------------------\n");

    printf("%-5s %-20s\n",
           "ID", "TIMING");

    printf("--------------------------------\n");

    for(i = 0; i < MAX_SHOWS; i++)
    {
        printf("%-5d %-20s\n",
               shows[i].id,
               shows[i].time);
    }
}

void displaySeats(int movieID, int showID)
{
    int i, j;
    int booked;

    printf("\n---------------- SEAT LAYOUT -----------------------\n");

    for(i = 1; i <= MAX_SEATS; i++)
    {
        booked = 0;

        for(j = 0; j < bookingCount; j++)
        {
            if(bookings[j].active &&
               bookings[j].movieID == movieID &&
               bookings[j].showID == showID)
            {
                int k;

                for(k = 0; k < bookings[j].ticketCount; k++)
                {
                    if(bookings[j].seatNo[k] == i)
                    {
                        booked = 1;
                        break;
                    }
                }
            }

            if(booked)
                break;
        }

        if(booked)
            printf("[ X ] ");
        else
            printf("[%02d] ", i);

        if(i % 6 == 0)
            printf("\n");
    }

    printf("\n\n[ X ] = BOOKED\n");
}

int isSeatAvailable(int movieID, int showID, int seatNo)
{
    int i, j;

    for(i = 0; i < bookingCount; i++)
    {
        if(bookings[i].active &&
           bookings[i].movieID == movieID &&
           bookings[i].showID == showID)
        {
            for(j = 0; j < bookings[i].ticketCount; j++)
            {
                if(bookings[i].seatNo[j] == seatNo)
                    return 0;
            }
        }
    }

    return 1;
}

int getMovieIndex(int movieID)
{
    int i;

    for(i = 0; i < MAX_MOVIES; i++)
    {
        if(movies[i].id == movieID)
            return i;
    }

    return -1;
}

int getShowIndex(int showID)
{
    int i;

    for(i = 0; i < MAX_SHOWS; i++)
    {
        if(shows[i].id == showID)
            return i;
    }

    return -1;
}

/* ---------- BOOK TICKET ---------- */

void bookTicket()
{
    int movieID;
    int showID;

    int movieIndex;
    int showIndex;

    int ticketCount;
    int seatNo;

    int i, j;
    int duplicate;

    char name[50];
    char phone[15];

    if(bookingCount >= MAX_BOOKINGS)
    {
        printf("\nBooking capacity reached!\n");
        return;
    }

    /* Select Movie */

    displayMovies();

    printf("\nEnter Movie ID: ");
    scanf("%d", &movieID);

    movieIndex = getMovieIndex(movieID);

    if(movieIndex == -1)
    {
        printf("Invalid Movie ID!\n");
        return;
    }

    /* Select Show */

    displayShows();

    printf("\nEnter Show ID: ");
    scanf("%d", &showID);

    showIndex = getShowIndex(showID);

    if(showIndex == -1)
    {
        printf("Invalid Show ID!\n");
        return;
    }

    /* Number of tickets */

    printf("\nEnter number of tickets: ");
    scanf("%d", &ticketCount);

    if(ticketCount < 1 || ticketCount > MAX_SEATS)
    {
        printf("Invalid number of tickets!\n");
        return;
    }

    displaySeats(movieID, showID);

    /* Select multiple seats */

    for(i = 0; i < ticketCount; i++)
    {
        printf("\nEnter seat number %d: ", i + 1);
        scanf("%d", &seatNo);

        /* Check seat range */

        if(seatNo < 1 || seatNo > MAX_SEATS)
        {
            printf("Invalid seat number!\n");
            i--;
            continue;
        }

        /* Check whether seat is already booked */

        if(!isSeatAvailable(movieID, showID, seatNo))
        {
            printf("Seat %d is already booked!\n", seatNo);
            i--;
            continue;
        }

        /* Check duplicate seats in current booking */

        duplicate = 0;

        for(j = 0; j < i; j++)
        {
            if(bookings[bookingCount].seatNo[j] == seatNo)
            {
                duplicate = 1;
                break;
            }
        }

        if(duplicate)
        {
            printf("You have already selected seat %d!\n",
                   seatNo);

            i--;
            continue;
        }

        bookings[bookingCount].seatNo[i] = seatNo;
    }

    /* Customer Details */

    printf("\nEnter Customer Name: ");
    scanf(" %[^\n]", name);

    printf("Enter Phone Number: ");
    scanf("%s", phone);

    /* Store Booking */

    bookings[bookingCount].bookingID =
        1001 + bookingCount;

    strcpy(bookings[bookingCount].customerName, name);

    strcpy(bookings[bookingCount].phone, phone);

    bookings[bookingCount].movieID = movieID;

    bookings[bookingCount].showID = showID;

    bookings[bookingCount].ticketCount =
        ticketCount;

    bookings[bookingCount].amount =
        movies[movieIndex].price * ticketCount;

    bookings[bookingCount].active = 1;

    /* Booking Confirmation */

    printf("\n");
    printf("====================================================\n");
    printf("              BOOKING CONFIRMED                    \n");
    printf("====================================================\n");

    printf("Booking ID   : %d\n",
           bookings[bookingCount].bookingID);

    printf("Customer     : %s\n",
           bookings[bookingCount].customerName);

    printf("Phone        : %s\n",
           bookings[bookingCount].phone);

    printf("Movie        : %s\n",
           movies[movieIndex].name);

    printf("Show         : %s\n",
           shows[showIndex].time);

    printf("Tickets      : %d\n",
           ticketCount);

    printf("Seats        : ");

    for(i = 0; i < ticketCount; i++)
    {
        printf("%d ", bookings[bookingCount].seatNo[i]);
    }

    printf("\n");

    printf("Price/Ticket : Rs. %.2f\n",
           movies[movieIndex].price);

    printf("Total Amount : Rs. %.2f\n",
           bookings[bookingCount].amount);

    printf("====================================================\n");

    bookingCount++;
}

/* ---------- VIEW BOOKING ---------- */

void viewBooking()
{
    int bookingID;
    int i, j;
    int found = 0;

    printf("\nEnter Booking ID: ");
    scanf("%d", &bookingID);

    for(i = 0; i < bookingCount; i++)
    {
        if(bookings[i].bookingID == bookingID &&
           bookings[i].active)
        {
            int movieIndex =
                getMovieIndex(bookings[i].movieID);

            int showIndex =
                getShowIndex(bookings[i].showID);

            printf("\n---------------- BOOKING DETAILS ------------------\n");

            printf("Booking ID  : %d\n",
                   bookings[i].bookingID);

            printf("Customer    : %s\n",
                   bookings[i].customerName);

            printf("Phone       : %s\n",
                   bookings[i].phone);

            printf("Movie       : %s\n",
                   movies[movieIndex].name);

            printf("Show        : %s\n",
                   shows[showIndex].time);

            printf("Tickets     : %d\n",
                   bookings[i].ticketCount);

            printf("Seats       : ");

            for(j = 0; j < bookings[i].ticketCount; j++)
            {
                printf("%d ", bookings[i].seatNo[j]);
            }

            printf("\n");

            printf("Amount      : Rs. %.2f\n",
                   bookings[i].amount);

            printf("----------------------------------------------------\n");

            found = 1;
            break;
        }
    }

    if(!found)
        printf("\nBooking not found!\n");
}

/* ---------- CANCEL BOOKING ---------- */

void cancelBooking()
{
    int bookingID;
    int i;

    printf("\nEnter Booking ID to cancel: ");
    scanf("%d", &bookingID);

    for(i = 0; i < bookingCount; i++)
    {
        if(bookings[i].bookingID == bookingID &&
           bookings[i].active)
        {
            bookings[i].active = 0;

            printf("\nBooking cancelled successfully!\n");

            return;
        }
    }

    printf("\nBooking ID not found!\n");
}

/* ---------- VIEW ALL BOOKINGS ---------- */

void viewAllBookings()
{
    int i, j;
    int found = 0;

    printf("\n---------------- ALL BOOKINGS ----------------------\n");

    printf("%-8s %-18s %-20s %-8s %-10s\n",
           "ID",
           "CUSTOMER",
           "MOVIE",
           "SEATS",
           "AMOUNT");

    printf("----------------------------------------------------\n");

    for(i = 0; i < bookingCount; i++)
    {
        if(bookings[i].active)
        {
            int movieIndex =
                getMovieIndex(bookings[i].movieID);

            printf("%-8d %-18s %-20s ",
                   bookings[i].bookingID,
                   bookings[i].customerName,
                   movies[movieIndex].name);

            for(j = 0; j < bookings[i].ticketCount; j++)
            {
                printf("%d ", bookings[i].seatNo[j]);
            }

            printf("     Rs. %.2f\n",
                   bookings[i].amount);

            found = 1;
        }
    }

    if(!found)
        printf("No active bookings.\n");
}

/* ---------- MAIN FUNCTION ---------- */

int main()
{
    int choice;

    do
    {
        displayHeader();

        printf("\n1. View Movies\n");
        printf("2. View Show Timings\n");
        printf("3. Book Ticket\n");
        printf("4. View Booking\n");
        printf("5. Cancel Booking\n");
        printf("6. View All Bookings\n");
        printf("7. Exit\n");

        printf("\nEnter your choice: ");
        scanf("%d", &choice);

        switch(choice)
        {
            case 1:
                displayMovies();
                break;

            case 2:
                displayShows();
                break;

            case 3:
                bookTicket();
                break;

            case 4:
                viewBooking();
                break;

            case 5:
                cancelBooking();
                break;

            case 6:
                viewAllBookings();
                break;

            case 7:
                printf("\nThank you for using the system!\n");
                break;

            default:
                printf("\nInvalid choice! Please try again.\n");
        }

    } while(choice != 7);

    return 0;
}
