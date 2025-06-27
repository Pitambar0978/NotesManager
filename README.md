# NotesManager

NotesManager

import java.io.*;
import java.util.*;

public class NotesManager {
    private static final String FILE = "notes.txt";

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        while (true) {
            System.out.println("\n📒 Notes Manager");
            System.out.println("1. View notes");
            System.out.println("2. Add a note");
            System.out.println("3. Delete a note");
            System.out.println("4. Exit");
            System.out.print("Choose an option: ");
            
            switch (sc.nextLine()) {
                case "1": viewNotes(); break;
                case "2": addNote(sc); break;
                case "3": deleteNote(sc); break;
                case "4":
                    System.out.println("Bye!");
                    sc.close();
                    return;
                default: System.out.println("Invalid option.");
            }
        }
    }

    private static void viewNotes() {
        File file = new File(FILE);
        if (!file.exists()) {
            System.out.println("No notes found.");
            return;
        }
        try (BufferedReader reader = new BufferedReader(new FileReader(file))) {
            System.out.println("\n--- Your Notes ---");
            String line;
            int i = 1;
            while ((line = reader.readLine()) != null) {
                System.out.printf("%d. %s%n", i++, line);
            }
        } catch (IOException e) {
            System.err.println("Error reading notes: " + e.getMessage());
        }
    }

    private static void addNote(Scanner sc) {
        System.out.print("Enter your note: ");
        String note = sc.nextLine();
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(FILE, true))) {
            writer.write(note);
            writer.newLine();
            System.out.println("Note added!");
        } catch (IOException e) {
            System.err.println("Error adding note: " + e.getMessage());
        }
    }

    private static void deleteNote(Scanner sc) {
        List<String> notes = new ArrayList<>();
        File file = new File(FILE);
        if (!file.exists()) {
            System.out.println("No notes to delete.");
            return;
        }
        try (BufferedReader reader = new BufferedReader(new FileReader(file))) {
            String line;
            while ((line = reader.readLine()) != null) notes.add(line);
        } catch (IOException e) {
            System.err.println("Error reading notes: " + e.getMessage());
            return;
        }

        if (notes.isEmpty()) {
            System.out.println("No notes to delete.");
            return;
        }

        for (int i = 0; i < notes.size(); i++) {
            System.out.printf("%d. %s%n", i + 1, notes.get(i));
        }
        System.out.print("Enter note number to delete: ");
        try {
            int idx = Integer.parseInt(sc.nextLine()) - 1;
            if (idx >= 0 && idx < notes.size()) {
                notes.remove(idx);
                try (BufferedWriter writer = new BufferedWriter(new FileWriter(file))) {
                    for (String n : notes) writer.write(n + System.lineSeparator());
                }
                System.out.println("Note deleted!");
            } else {
                System.out.println("Invalid number.");
            }
        } catch (NumberFormatException | IOException e) {
            System.err.println("Error deleting note: " + e.getMessage());
        }
    }
}
