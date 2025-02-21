# Write a Java program to implement an ArrayList that stores employee details (ID, Name, and Salary). Allow users to add, update, remove, and search employees.
    import java.util.ArrayList;
    import java.util.Scanner;

    class Employee {
    int id;
    String name;
    double salary;

    public Employee(int id, String name, double salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }

    @Override
    public String toString() {
        return "ID: " + id + ", Name: " + name + ", Salary: " + salary;
    }
    }

    public class EmployeeManager {
    private static ArrayList<Employee> employees = new ArrayList<>();
    private static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        while (true) {
            System.out.println("\nEmployee Management System");
            System.out.println("1. Add Employee");
            System.out.println("2. Update Employee");
            System.out.println("3. Remove Employee");
            System.out.println("4. Search Employee");
            System.out.println("5. Display All Employees");
            System.out.println("6. Exit");
            System.out.print("Choose an option: ");

            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1:
                    addEmployee();
                    break;
                case 2:
                    updateEmployee();
                    break;
                case 3:
                    removeEmployee();
                    break;
                case 4:
                    searchEmployee();
                    break;
                case 5:
                    displayEmployees();
                    break;
                case 6:
                    System.out.println("Exiting...");
                    return;
                default:
                    System.out.println("Invalid option! Try again.");
            }
        }
    }

    private static void addEmployee() {
        System.out.print("Enter Employee ID: ");
        int id = scanner.nextInt();
        scanner.nextLine();
        System.out.print("Enter Name: ");
        String name = scanner.nextLine();
        System.out.print("Enter Salary: ");
        double salary = scanner.nextDouble();

        employees.add(new Employee(id, name, salary));
        System.out.println("Employee added successfully.");
    }

    private static void updateEmployee() {
        System.out.print("Enter Employee ID to update: ");
        int id = scanner.nextInt();
        scanner.nextLine();
        for (Employee emp : employees) {
            if (emp.id == id) {
                System.out.print("Enter New Name: ");
                emp.name = scanner.nextLine();
                System.out.print("Enter New Salary: ");
                emp.salary = scanner.nextDouble();
                System.out.println("Employee updated successfully.");
                return;
            }
        }
        System.out.println("Employee not found.");
    }

    private static void removeEmployee() {
        System.out.print("Enter Employee ID to remove: ");
        int id = scanner.nextInt();
        for (Employee emp : employees) {
            if (emp.id == id) {
                employees.remove(emp);
                System.out.println("Employee removed successfully.");
                return;
            }
        }
        System.out.println("Employee not found.");
    }

    private static void searchEmployee() {
        System.out.print("Enter Employee ID to search: ");
        int id = scanner.nextInt();
        for (Employee emp : employees) {
            if (emp.id == id) {
                System.out.println(emp);
                return;
            }
        }
        System.out.println("Employee not found.");
    }

    private static void displayEmployees() {
        if (employees.isEmpty()) {
            System.out.println("No employees to display.");
        } else {
            for (Employee emp : employees) {
                System.out.println(emp);
            }
         }
       }
    }
    
    
# Create a program to collect and store all the cards to assist the users in finding all the cards in a given symbol using Collection interface.

    import java.util.*;

    class Card {
    private String symbol;
    private String name;

    public Card(String symbol, String name) {
        this.symbol = symbol;
        this.name = name;
    }

    public String getSymbol() {
        return symbol;
    }

    @Override
    public String toString() {
        return "Card{Name='" + name + "', Symbol='" + symbol + "'}";
    }
}

    public class CardCollection {
    private Collection<Card> cards;
    private Scanner scanner;

    public CardCollection() {
        cards = new ArrayList<>();
        scanner = new Scanner(System.in);
    }

    public void addCard() {
        System.out.print("Enter Card Name: ");
        String name = scanner.nextLine();
        System.out.print("Enter Card Symbol: ");
        String symbol = scanner.nextLine();

        cards.add(new Card(symbol, name));
        System.out.println("Card added successfully.");
    }

    public void searchBySymbol() {
        System.out.print("Enter Symbol to search: ");
        String symbol = scanner.nextLine();

        boolean found = false;
        for (Card card : cards) {
            if (card.getSymbol().equalsIgnoreCase(symbol)) {
                System.out.println(card);
                found = true;
            }
        }
        if (!found) {
            System.out.println("No cards found with the given symbol.");
        }
    }

    public void displayAllCards() {
        if (cards.isEmpty()) {
            System.out.println("No cards available.");
        } else {
            for (Card card : cards) {
                System.out.println(card);
            }
        }
    }

    public void start() {
        while (true) {
            System.out.println("\nCard Collection System");
            System.out.println("1. Add Card");
            System.out.println("2. Search Cards by Symbol");
            System.out.println("3. Display All Cards");
            System.out.println("4. Exit");
            System.out.print("Choose an option: ");

            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1:
                    addCard();
                    break;
                case 2:
                    searchBySymbol();
                    break;
                case 3:
                    displayAllCards();
                    break;
                case 4:
                    System.out.println("Exiting...");
                    return;
                default:
                    System.out.println("Invalid option. Try again.");
            }
        }
    }

    public static void main(String[] args) {
        CardCollection collection = new CardCollection();
        collection.start();
    }
    }
    
# Develop a ticket booking system with synchronized threads to ensure no double booking of seats. Use thread priorities to simulate VIP bookings being processed first.

    import java.util.concurrent.locks.Lock;
    import java.util.concurrent.locks.ReentrantLock;

    class TicketBookingSystem {
    private int availableSeats;
    private final Lock lock = new ReentrantLock();

    public TicketBookingSystem(int seats) {
        this.availableSeats = seats;
    }

    public void bookTicket(String customerName) {
        lock.lock();
        try {
            if (availableSeats > 0) {
                System.out.println(customerName + " successfully booked a seat.");
                availableSeats--;
            } else {
                System.out.println(customerName + " booking failed. No seats available.");
            }
        } finally {
            lock.unlock();
        }
    }
    }

    class Customer extends Thread {
    private final TicketBookingSystem bookingSystem;
    private final String customerName;

    public Customer(TicketBookingSystem system, String name, int priority) {
        this.bookingSystem = system;
        this.customerName = name;
        setPriority(priority);
    }

    @Override
    public void run() {
        bookingSystem.bookTicket(customerName);
    }
    }

    public class TicketBookingApp {
    public static void main(String[] args) {
        TicketBookingSystem system = new TicketBookingSystem(5);

        Customer vip1 = new Customer(system, "VIP1", Thread.MAX_PRIORITY);
        Customer vip2 = new Customer(system, "VIP2", Thread.MAX_PRIORITY);
        Customer regular1 = new Customer(system, "Regular1", Thread.NORM_PRIORITY);
        Customer regular2 = new Customer(system, "Regular2", Thread.NORM_PRIORITY);
        Customer regular3 = new Customer(system, "Regular3", Thread.NORM_PRIORITY);
        Customer regular4 = new Customer(system, "Regular4", Thread.MIN_PRIORITY);

        vip1.start();
        vip2.start();
        regular1.start();
        regular2.start();
        regular3.start();
        regular4.start();
    }
    }
