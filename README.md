import java.io.*;
import java.util.*;

class Expense {
    String category;
    double amount;
    String date;

    Expense(String category, double amount, String date) {
        this.category = category;
        this.amount = amount;
        this.date = date;
    }

    @Override
    public String toString() {
        return date + " | " + category + " | ₹" + amount;
    }
}

public class ExpenseTracker {
    static final String FILE_NAME = "expenses.txt";
    static Scanner scanner = new Scanner(System.in);
    static List<Expense> expenses = new ArrayList<>();

    public static void main(String[] args) {
        loadExpenses();

        while (true) {
            System.out.println("\n--- Expense Tracker ---");
            System.out.println("1. Add Expense");
            System.out.println("2. View Expenses");
            System.out.println("3. View Total Expense");
            System.out.println("4. Exit");
            System.out.print("Enter choice: ");
            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1 -> addExpense();
                case 2 -> viewExpenses();
                case 3 -> viewTotal();
                case 4 -> {
                    saveExpenses();
                    System.out.println("Goodbye!");
                    return;
                }
                default -> System.out.println("Invalid choice.");
            }
        }
    }

    static void addExpense() {
        System.out.print("Enter category: ");
        String category = scanner.nextLine();
        System.out.print("Enter amount: ₹");
        double amount = scanner.nextDouble();
        scanner.nextLine(); // Consume newline
        System.out.print("Enter date (YYYY-MM-DD): ");
        String date = scanner.nextLine();

        expenses.add(new Expense(category, amount, date));
        System.out.println("Expense added!");
    }

    static void viewExpenses() {
        if (expenses.isEmpty()) {
            System.out.println("No expenses recorded.");
            return;
        }

        System.out.println("\n--- Expense List ---");
        for (Expense e : expenses) {
            System.out.println(e);
        }
    }

    static void viewTotal() {
        double total = 0;
        for (Expense e : expenses) {
            total += e.amount;
        }
        System.out.println("Total Expenses: ₹" + total);
    }

    static void loadExpenses() {
        try (BufferedReader br = new BufferedReader(new FileReader(FILE_NAME))) {
            String line;
            while ((line = br.readLine()) != null) {
                String[] parts = line.split(",");
                expenses.add(new Expense(parts[0], Double.parseDouble(parts[1]), parts[2]));
            }
        } catch (IOException e) {
            // No previous data, first run
        }
    }

    static void saveExpenses() {
        try (BufferedWriter bw = new BufferedWriter(new FileWriter(FILE_NAME))) {
            for (Expense e : expenses) {
                bw.write(e.category + "," + e.amount + "," + e.date);
                bw.newLine();
            }
        } catch (IOException e) {
            System.out.println("Error saving expenses.");
        }
    }
}
