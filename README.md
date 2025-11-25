import java.util.*;

class InvalidMarksException extends Exception {
    public InvalidMarksException(String msg) {
        super(msg);
    }
}

class Student {
    int roll;
    String name;
    int[] marks = new int[3];

    public Student(int roll, String name, int[] marks) throws InvalidMarksException {
        this.roll = roll;
        this.name = name;
        this.marks = marks;
        validate();
    }

    void validate() throws InvalidMarksException {
        for (int m : marks) {
            if (m < 0 || m > 100) {
                throw new InvalidMarksException("Invalid marks: " + m);
            }
        }
    }

    double avg() {
        int sum = 0;
        for (int m : marks) sum += m;
        return sum / 3.0;
    }

    void print() {
        double a = avg();
        System.out.println("Roll Number: " + roll);
        System.out.println("Student Name: " + name);
        System.out.println("Marks: " + marks[0] + " " + marks[1] + " " + marks[2]);
        System.out.println("Average: " + a);
        System.out.println("Result: " + (a >= 40 ? "Pass" : "Fail"));
    }
}

public class ResultManager {

    static Student[] data = new Student[100];
    static int count = 0;
    static Scanner sc = new Scanner(System.in);

    static void add() {
        try {
            System.out.print("Enter Roll Number: ");
            int r = sc.nextInt();
            sc.nextLine();

            System.out.print("Enter Student Name: ");
            String n = sc.nextLine();

            int[] m = new int[3];
            for (int i = 0; i < 3; i++) {
                System.out.print("Enter marks for subject " + (i + 1) + ": ");
                m[i] = sc.nextInt();
            }

            data[count++] = new Student(r, n, m);
            System.out.println("Student added successfully.");

        } catch (InputMismatchException e) {
            System.out.println("Invalid input type.");
            sc.nextLine();
        } catch (InvalidMarksException e) {
            System.out.println(e.getMessage());
        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
        }
    }

    static void search() {
        try {
            System.out.print("Enter Roll Number to search: ");
            int r = sc.nextInt();

            boolean ok = false;
            for (int i = 0; i < count; i++) {
                if (data[i].roll == r) {
                    data[i].print();
                    ok = true;
                    break;
                }
            }

            if (!ok) {
                System.out.println("Student not found.");
            }

        } catch (InputMismatchException e) {
            System.out.println("Invalid input type.");
            sc.nextLine();
        }
    }

    static void menu() {
        try {
            while (true) {
                System.out.println("===== Student Result Management System =====");
                System.out.println("1. Add Student");
                System.out.println("2. Show Student Details");
                System.out.println("3. Exit");
                System.out.print("Enter your choice: ");

                int ch = sc.nextInt();

                if (ch == 1) add();
                else if (ch == 2) search();
                else if (ch == 3) break;
                else System.out.println("Invalid choice.");
            }
        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
        } finally {
            System.out.println("Closing application...");
            sc.close();
        }
    }

    public static void main(String[] args) {
        menu();
    }
}
