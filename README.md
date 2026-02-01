import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

class Book {
    private String id;
    private String title;
    private String author;
    private boolean isAvailable;

    public Book(String id, String title, String author) {
        this.id = id;
        this.title = title;
        this.author = author;
        this.isAvailable = true;
    }

    public String getId() { return id; }
    public String getTitle() { return title; }
    public boolean isAvailable() { return isAvailable; }

    
    public void setAvailable(boolean available) { isAvailable = available; }

@Override
public String toString() {
    return 
           "\nBOOK ID    : " + id +
           "\nTITLE      : " + title +
           "\nAUTHOR     : " + author +
           "\nSTATUS     : " + (isAvailable ? "Available" : "Borrowed");
}
}

class LibraryManager {
    private List<Book> books = new ArrayList<>();

    public void addBook(Book book) {
        books.add(book);
        System.out.println(" Success: Book added to inventory.");
    }

    public void viewAllBooks() {
        if (books.isEmpty()) {
            System.out.println(">>> Notice: The library is currently empty.");
        } else {
            System.out.println("\n--- Current Inventory ---");
            books.forEach(System.out::println);
        }
    }

    public boolean updateStatus(String id, boolean status) {
        for (Book b : books) {
            if (b.getId().equalsIgnoreCase(id)) {
                b.setAvailable(status);
                return true;
            }
        }
        return false;
    }

    public boolean removeBook(String id) {
        return books.removeIf(b -> b.getId().equalsIgnoreCase(id));
    }
}
public class LibrarySystem {
    public static void main(String[] args) {
        LibraryManager manager = new LibraryManager();
        Scanner scanner = new Scanner(System.in);
        
        while (true) {
            
            System.out.println("  LIBRARY MANAGEMENT SYSTEM ");
            System.out.println("1. Add New Book");
            System.out.println("2. View All Books");
            System.out.println("3. Update Availability");
            System.out.println("4. Remove Book");
            System.out.println("5. Exit");
            System.out.print("Select an option: ");

            String choice = scanner.nextLine();

            switch (choice) {
                case "1":
                    System.out.print("Enter Book ID: "); String id = scanner.nextLine();
                    System.out.print("Enter Title: "); String title = scanner.nextLine();
                    System.out.print("Enter Author: "); String author = scanner.nextLine();
                    manager.addBook(new Book(id, title, author));
                    break;

                case "2":
                    manager.viewAllBooks();
                    break;

                case "3":
                    System.out.print("Enter Book ID to update: "); String uId = scanner.nextLine();
                    System.out.print("Is it available now? (yes/no): ");
                    boolean status = scanner.nextLine().equalsIgnoreCase("yes");
                    if (manager.updateStatus(uId, status)) {
                        System.out.println(">>> Success: Status updated.");
                    } else {
                        System.out.println("Error: Book ID not found.");
                    }
                    break;

                case "4":
                    System.out.print("Enter Book ID to remove: "); String rId = scanner.nextLine();
                    if (manager.removeBook(rId)) {
                        System.out.println(">>> Success: Book removed.");
                    } else {
                        System.out.println("Error: Book ID not found.");
                    }
                    break;

                case "5":
                    System.out.println("Exiting System. Goodbye!");
                    scanner.close();
                    return;

                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }
}
