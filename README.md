# Task 5

# Student Management System


## Code
```
import java.io.*;
import java.util.*;

class Student {
    String name, grade;
    int rollNo;

    public Student(String name, int rollNo, String grade) {
        this.name = name; this.rollNo = rollNo; this.grade = grade;
    }

    public String toString() { return rollNo + "," + name + "," + grade; }
    public static Student fromString(String data) {
        String[] p = data.split(",");
        return new Student(p[1], Integer.parseInt(p[0]), p[2]);
    }
}

class StudentManagement {
    private static final String FILE = "students.csv";
    private List<Student> students = loadStudents();

    private List<Student> loadStudents() {
        List<Student> list = new ArrayList<>();
        try (Scanner sc = new Scanner(new File(FILE))) {
            while (sc.hasNextLine()) list.add(Student.fromString(sc.nextLine()));
        } catch (Exception e) { System.out.println(" No previous data found."); }
        return list;
    }

    private void saveStudents() {
        try (PrintWriter out = new PrintWriter(FILE)) {
            students.forEach(out::println);
        } catch (Exception e) { System.out.println(" Error saving data."); }
    }

    public void addStudent(Student s) {
        if (students.stream().anyMatch(st -> st.rollNo == s.rollNo)) System.out.println(" Roll No exists!");
        else { students.add(s); saveStudents(); System.out.println("Student added!"); }
    }

    public void removeStudent(int rollNo) {
        if (students.removeIf(s -> s.rollNo == rollNo)) { saveStudents(); System.out.println(" Removed!"); }
        else System.out.println(" Not found!");
    }

    public void searchStudent(int rollNo) {
        students.stream().filter(s -> s.rollNo == rollNo).findFirst()
                .ifPresentOrElse(System.out::println, () -> System.out.println(" Not found!"));
    }

    public void displayAll() {
        if (students.isEmpty()) System.out.println(" No students found!");
        else students.forEach(System.out::println);
    }
}

public class StudentManagement1 {
    private static final Scanner sc = new Scanner(System.in);
    private static final StudentManagement sm = new StudentManagement();

    public static void main(String[] args) {
        while (true) {
            System.out.print("\n1️ Add 2️Remove 3️ Search 4️ Display 5️ Exit: ");
            switch (getValidInt()) {
                case 1 -> sm.addStudent(new Student(getInput("Name"), getValidInt("Roll No"), getInput("Grade")));
                case 2 -> sm.removeStudent(getValidInt("Roll No"));
                case 3 -> sm.searchStudent(getValidInt("Roll No"));
                case 4 -> sm.displayAll();
                case 5 -> { System.out.println(" Exiting..."); return; }
                default -> System.out.println("Invalid choice!");
            }
        }
    }

    private static String getInput(String msg) {
        System.out.print(msg + ": ");
        return sc.nextLine().trim();
    }

    private static int getValidInt() {
        while (true) {
            try { return Integer.parseInt(sc.nextLine().trim()); }
            catch (Exception e) { System.out.print(" Enter a valid number: "); }
        }
    }

    private static int getValidInt(String msg) {
        System.out.print(msg + ": ");
        return getValidInt();
    }
}
```

## Output

![image](https://github.com/user-attachments/assets/20807646-20d0-4832-b585-038c6ba97120)
