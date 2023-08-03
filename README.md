import java.util.Scanner;

public class TrainSeatBooking {
    private static final int TOTAL_SEATS = 80;
    private static final int ROWS = 11;
    private static final int SEATS_PER_ROW = 7;

    private static char[][] coach;

    public static void main(String[] args) {
        coach = createCoach(ROWS, SEATS_PER_ROW);

        Scanner scanner = new Scanner(System.in);

        while (true) {
            displayCoach(coach);
            System.out.print("Enter the number of seats you want to book (or 0 to exit): ");
            int requiredSeats = scanner.nextInt();

            if (requiredSeats == 0) {
                System.out.println("Thank you for using the booking system.");
                break;
            }

            if (requiredSeats > 7 || requiredSeats < 1) {
                System.out.println("Invalid input. You can only book 1 to 7 seats at a time.");
                continue;
            }

            int[] bookedSeats = bookSeats(requiredSeats);
            if (bookedSeats != null) {
                System.out.print("Seats ");
                for (int seat : bookedSeats) {
                    System.out.print(seat + " ");
                }
                System.out.println("have been booked for you.");
            } else {
                System.out.println("Sorry, there are no consecutive seats available. Please try again with a smaller number.");
            }

            int bookedCount = countBookedSeats();
            if (bookedCount == TOTAL_SEATS) {
                System.out.println("All seats are booked. The coach is full.");
                break;
            }
        }

        scanner.close();
    }

    private static char[][] createCoach(int rows, int seatsPerRow) {
        char[][] coach = new char[rows][seatsPerRow];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < seatsPerRow; j++) {
                coach[i][j] = 'O';
            }
        }
        return coach;
    }

    private static void displayCoach(char[][] coach) {
        for (char[] row : coach) {
            for (char seat : row) {
                System.out.print(seat + " ");
            }
            System.out.println();
        }
    }

    private static int[] bookSeats(int requiredSeats) {
        int row, startSeat;

        for (row = 0; row < ROWS; row++) {
            int consecutiveSeats = 0;
            for (startSeat = 0; startSeat < SEATS_PER_ROW; startSeat++) {
                if (coach[row][startSeat] == 'O') {
                    consecutiveSeats++;
                    if (consecutiveSeats == requiredSeats) {
                        return markSeatsAsBooked(row, startSeat - requiredSeats + 1, requiredSeats);
                    }
                } else {
                    consecutiveSeats = 0;
                }
            }
        }
        return null;
    }

    private static int[] markSeatsAsBooked(int row, int startSeat, int requiredSeats) {
        int[] bookedSeats = new int[requiredSeats];
        for (int i = 0; i < requiredSeats; i++) {
            coach[row][startSeat + i] = 'X';
            bookedSeats[i] = startSeat + i + 1;
        }
        return bookedSeats;
    }

    private static int countBookedSeats() {
        int count = 0;
        for (char[] row : coach) {
            for (char seat : row) {
                if (seat == 'X') {
                    count++;
                }
            }
        }
        return count;
    }
}
