# Employee-Management-System
This program sorts the employee's name, salary and role. Allows the user to search employee's data.
public class Employee implements Comparable<Employee> {
    private String role; //Employee is a Java class
                            // that complies with the Comparable interface.

    private String name;
    private double salary; //Three private instance variables are present in the Employee class:
                              //  role (a String), name (a String), and salary (a double).

    public Employee(String role, String name, double salary) {
        this.role = role; //The Employee class's function Object()
                          // { [native code] } accepts three arguments that are used to
                            //  initialise the instance variables.
        this.name = name;
        this.salary = salary;
    }

    public String getRole() {
        return role;
    }

    public String getName() {
        return name;
    }

    public double getSalary() {
        return salary;
    }

    @Override
    public int compareTo(Employee other) { //The class complies with the Comparable interface's
                                             // requirement that it implement the compareTo() method.
                                            //Start by comparing salaries.
        int salaryCmp = Double.compare(this.salary, other.salary);
        if (salaryCmp != 0) { //If the salaries are equal, the method compares the names of the two Employee
                              // objects using the compareTo() method of the String class.
            return salaryCmp;
        }  //
        // If salaries are equal, compare by name
        int nameCmp = this.name.compareTo(other.name);
        if (nameCmp != 0) {
            return nameCmp;
        }
        // If names are also equal, compare by role
        return this.role.compareTo(other.role);
    }
    @Override
    public String toString() {
        return this.role + " " + this.name + " " + this.salary;
    }
}

import java.util.ArrayList;
import java.util.Collections;
import java.util.Scanner;

public class EmployeeInput {
    public static void main(String[] args) { //The programme generates a Scanner object and an
                                                // ArrayList to hold Employee instances in the main method:
        Scanner scanner = new Scanner(System.in);
        ArrayList<Employee> employees = new ArrayList<>();

        while (true) { //The user is continually prompted to submit employment information by the code until the user enters "quit":
            System.out.print("Enter employee details (name role salary), or 'quit' to exit: ");
            String input = scanner.nextLine();
            if (input.equals("quit")) { // Detect if the user enters "quit"
                Collections.sort(employees); // Sort the list of employees
                System.out.println("Sorted list of employees:");
                for (Employee employee : employees) { //
                    System.out.println(employee);
                }
                break; // Exit the loop
            }
            String[] parts = input.split("\\s+"); //Within the loop, the code separates the user's input into an array of strings
                                                        // and checks that the array includes the three values (name, position, and salary):
                                                     //Check to see if there are more than three numbers or fewer.

            if (parts.length != 3) {
                System.out.println("Invalid input: please enter three values");
                continue;
            }
            String name = parts[0]; //Inside of the loop, the code separates the user's input into an array of strings
                                     // and checks that the array includes the three components (name, position, and salary):
            String role = parts[1];
            double salary;
            try {
                salary = Double.parseDouble(parts[2]); // Detect if the user enters a non-numerical value for salary
            } catch (NumberFormatException e) {
                System.out.println("Invalid salary: " + parts[2]);
                continue;
            }
            Employee employee = new Employee(role, name, salary); ///The code below prints a message confirming the employee's
                                                                   // addition and adds the Employee object to the array's list of workers:
            employees.add(employee);
            System.out.println("Added employee: " + employee);
        }
    }
}
import java.awt.event.*;
import java.util.HashMap;
import java.util.Map;
import javax.swing.*;

public class MyComponent extends JComponent { // This software employs a GUI. The "MyComponent" class implements the component as a
    // subclass of the "JComponent" class

    private Map<Integer, Integer> quadrantCounts; // A map to store the number of times an event occurs in each quadrant

    public MyComponent() {
        quadrantCounts = new HashMap<>(); // HashMap" to record the frequency
        quadrantCounts.put(1, 0);  //with which the user engages with each quadrant.
        quadrantCounts.put(2, 0); // Set the numbers for each quadrant to 0 at the beginning.
        quadrantCounts.put(3, 0);
        quadrantCounts.put(4, 0);

        addMouseListener(new MouseListener() { //
            @Override
            public void mouseClicked(MouseEvent e) {
                int quadrant = getQuadrant(e.getX(), e.getY());
                incrementCount(quadrant);
                printCounts();
            }
            @Override
            public void mouseEntered(MouseEvent e) {}
            @Override
            public void mouseExited(MouseEvent e) {}
            @Override
            public void mousePressed(MouseEvent e) {}
            @Override
            public void mouseReleased(MouseEvent e) {}
        });

        addKeyListener(new KeyListener() { //Another anonymous class that uses the
                                              // KeyListener interface is added by the addKeyListener() method.
            @Override
            public void keyPressed(KeyEvent e) {
                // Ignore key presses that don't produce a visible character
                if (e.getKeyChar() == KeyEvent.CHAR_UNDEFINED) {
                    return; //The keyPressed(), keyReleased(), and keyTyped()
                             // functions are implemented by this class.
                }
                int quadrant = getQuadrant(getMousePosition().x, getMousePosition().y);
                incrementCount(quadrant);
                printCounts();
            }
            @Override
            public void keyReleased(KeyEvent e) {}
            @Override
            public void keyTyped(KeyEvent e) {}
        });

        setFocusable(true); //the setFocusable(true) method is
        // called to ensure that the component can receive input events.




    }

    private int getQuadrant(int x, int y) { // "getQuadrant" method takes x and y coordinates
        int width = getWidth(); // of a point the component and returns an integer indicating which quadrant
        int height = getHeight(); // the point is in.
        int halfWidth = width / 2;
        int halfHeight = height / 2;
        if (x < halfWidth && y < halfHeight) { // Quadrant 1
            return 1;
        } else if (x >= halfWidth && y < halfHeight) { // Quadrant 2
            return 2;
        } else if (x >= halfWidth && y >= halfHeight) { // Quadrant 3
            return 3;
        } else { // Quadrant 4
            return 4;
        }
    }

    private void incrementCount(int quadrant) { // increments the count for the specified quadrant
        int count = quadrantCounts.get(quadrant); // quadrant "quadrantCounts" map.
        quadrantCounts.put(quadrant, count + 1);
    }

    private void printCounts() { //The printCounts() method prints the
                                   // current counts for each quadrant in the quadrantCounts map.
        System.out.println("Quadrant counts:");
        for (int quadrant : quadrantCounts.keySet()) {
            int count = quadrantCounts.get(quadrant);
            System.out.println("Quadrant " + quadrant + ": " + count);
        }
        System.out.println();
    }

    public static void main(String[] args) { //In addition to creating a new MyComponent object,
                                             // the main() function creates a JFrame object and specifies its title, size, and visibility.

        JFrame frame = new JFrame("My Component");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(400, 400);
        frame.setVisible(true);

        MyComponent myComponent = new MyComponent();
        frame.add(myComponent); //When the programme is started, the JFrame will display the MyComponent.
    }
}
