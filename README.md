# CodeAlpha_Student-Grade-Tracker
The Student Grade Tracker is a Java-based desktop application developed using Java Swing. The purpose of this project is to manage and analyze student academic performance efficiently. It allows users to input, store, and process student grades through a graphical user interface.
import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import java.awt.*;
import java.awt.event.*;
import java.util.ArrayList;

class Student {
    String name;
    double marks;

    Student(String name, double marks) {
        this.name = name;
        this.marks = marks;
    }
}

public class StudentGradeTrackerGUI extends JFrame {

    private JTextField nameField, marksField;
    private JTable table;
    private DefaultTableModel model;
    private ArrayList<Student> students;

    private JLabel avgLabel, highLabel, lowLabel;

    public StudentGradeTrackerGUI() {
        students = new ArrayList<>();

        setTitle("Student Grade Tracker");
        setSize(600, 500);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        
        JPanel inputPanel = new JPanel(new GridLayout(3, 2, 10, 10));

        inputPanel.add(new JLabel("Student Name:"));
        nameField = new JTextField();
        inputPanel.add(nameField);

        inputPanel.add(new JLabel("Marks:"));
        marksField = new JTextField();
        inputPanel.add(marksField);

        JButton addButton = new JButton("Add Student");
        inputPanel.add(addButton);

        JButton calculateButton = new JButton("Calculate");
        inputPanel.add(calculateButton);

        add(inputPanel, BorderLayout.NORTH);

        
        model = new DefaultTableModel(new String[]{"Name", "Marks"}, 0);
        table = new JTable(model);
        add(new JScrollPane(table), BorderLayout.CENTER);

       
        JPanel resultPanel = new JPanel(new GridLayout(3, 1));

        avgLabel = new JLabel("Average: ");
        highLabel = new JLabel("Highest: ");
        lowLabel = new JLabel("Lowest: ");

        resultPanel.add(avgLabel);
        resultPanel.add(highLabel);
        resultPanel.add(lowLabel);

        add(resultPanel, BorderLayout.SOUTH);


        addButton.addActionListener(e -> addStudent());
        calculateButton.addActionListener(e -> calculateStats());

        setVisible(true);
    }

    private void addStudent() {
        try {
            String name = nameField.getText();
            double marks = Double.parseDouble(marksField.getText());

            students.add(new Student(name, marks));
            model.addRow(new Object[]{name, marks});

            nameField.setText("");
            marksField.setText("");

        } catch (Exception e) {
            JOptionPane.showMessageDialog(this, "Enter valid data!");
        }
    }

    private void calculateStats() {
        if (students.isEmpty()) {
            JOptionPane.showMessageDialog(this, "No data available!");
            return;
        }

        double sum = 0;
        double highest = students.get(0).marks;
        double lowest = students.get(0).marks;

        for (Student s : students) {
            sum += s.marks;
            if (s.marks > highest) highest = s.marks;
            if (s.marks < lowest) lowest = s.marks;
        }

        double average = sum / students.size();

        avgLabel.setText("Average: " + average);
        highLabel.setText("Highest: " + highest);
        lowLabel.setText("Lowest: " + lowest);
    }

    public static void main(String[] args) {
        new StudentGradeTrackerGUI();
    }
}
