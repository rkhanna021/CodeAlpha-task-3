import java.util.ArrayList;
import java.util.Scanner;

class Room {
    private int roomNumber;
    private String category;
    private boolean isAvailable;

    public Room(int roomNumber, String category) {
        this.roomNumber = roomNumber;
        this.category = category;
        this.isAvailable = true;
    }
    public int getRoomNumber() {
        return roomNumber;
    }
    public String getCategory() {
        return category;
    }
    public boolean isAvailable() {
        return isAvailable;
    }
    public void setAvailable(boolean available) {
        isAvailable = available;
    }
}

class Reservation {
    private String guestName;
    private Room room;
    private int numberOfNights;

    public Reservation(String guestName, Room room, int numberOfNights) {
        this.guestName = guestName;
        this.room = room;
        this.numberOfNights = numberOfNights;
    }

    public String getGuestName() {
        return guestName;
    }

    public Room getRoom() {
        return room;
    }

    public int getNumberOfNights() {
        return numberOfNights;
    }

    public double calculateTotalPrice() {
        double ratePerNight;
        switch (room.getCategory().toLowerCase()) {
            case "standard":
                ratePerNight = 100;
                break;
            case "deluxe":
                ratePerNight = 150;
                break;
            case "suite":
                ratePerNight = 250;
                break;
            default:
                ratePerNight = 0;
        }
        return ratePerNight * numberOfNights;
    }

    @Override
    public String toString() {
        return "Reservation Details:\n" +
                "Guest Name: " + guestName + "\n" +
                "Room Number: " + room.getRoomNumber() + " (" + room.getCategory() + ")\n" +
                "Number of Nights: " + numberOfNights + "\n" +
                "Total Price: $" + calculateTotalPrice();
    }
}

class Hotel {
    private ArrayList<Room> rooms;
    private ArrayList<Reservation> reservations;

    public Hotel() {
        rooms = new ArrayList<>();
        reservations = new ArrayList<>();
        // Sample Rooms
        rooms.add(new Room(101, "Standard"));
        rooms.add(new Room(102, "Standard"));
        rooms.add(new Room(201, "Deluxe"));
        rooms.add(new Room(202, "Deluxe"));
        rooms.add(new Room(301, "Suite"));
        rooms.add(new Room(302, "Suite"));
    }

    public ArrayList<Room> searchAvailableRooms(String category) {
        ArrayList<Room> availableRooms = new ArrayList<>();
        for (Room room : rooms) {
            if (room.getCategory().equalsIgnoreCase(category) && room.isAvailable()) {
                availableRooms.add(room);
            }
        }
        return availableRooms;
    }

    public boolean makeReservation(String guestName, String category, int numberOfNights) {
        ArrayList<Room> availableRooms = searchAvailableRooms(category);
        if (!availableRooms.isEmpty()) {
            Room room = availableRooms.get(0);
            room.setAvailable(false);
            Reservation reservation = new Reservation(guestName, room, numberOfNights);
            reservations.add(reservation);
            System.out.println("Reservation Successful!");
            System.out.println(reservation);
            return true;
        } else {
            System.out.println("No available rooms in the " + category + " category.");
            return false;
        }
    }

    public void viewAllReservations() {
        if (reservations.isEmpty()) {
            System.out.println("No reservations found.");
        } else {
            for (Reservation reservation : reservations) {
                System.out.println(reservation);
                System.out.println("");
            }
        }
    }
}

public class HotelReservationSystem {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Hotel hotel = new Hotel();

        while (true) {
            System.out.println("Welcome to the Hotel Reservation System");
            System.out.println("1. Search Available Rooms");
            System.out.println("2. Make a Reservation");
            System.out.println("3. View All Reservations");
            System.out.println("4. Exit");
            System.out.print("Choose an option: ");
            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1:
                    System.out.print("Enter room category (Standard/Deluxe/Suite): ");
                    String category = scanner.nextLine();
                    ArrayList<Room> availableRooms = hotel.searchAvailableRooms(category);
                    if (availableRooms.isEmpty()) {
                        System.out.println("No available rooms in the " + category + " category.");
                    } else {
                        System.out.println("Available Rooms:");
                        for (Room room : availableRooms) {
                            System.out.println("Room Number: " + room.getRoomNumber());
                        }
                    }
                    break;
                case 2:
                    System.out.print("Enter your name: ");
                    String guestName = scanner.nextLine();
                    System.out.print("Enter room category (Standard/Deluxe/Suite): ");
                    category = scanner.nextLine();
                    System.out.print("Enter number of nights: ");
                    int numberOfNights = scanner.nextInt();
                    hotel.makeReservation(guestName, category, numberOfNights);
                    break;
                case 3:
                    hotel.viewAllReservations();
                    break;
                case 4:
                    System.out.println("Thank you for using the Hotel Reservation System!");
                    scanner.close();
                    return;
                default:
                    System.out.println("Invalid option. Please try again.");
            }
            System.out.println();
        }
    }
}
\
