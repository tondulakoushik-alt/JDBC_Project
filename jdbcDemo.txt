import java.sql.*;
import java.util.Scanner;

public class H2JDBCDemo {

    private static final String JDBC_URL = "jdbc:h2:./candy";
    private static final String USER = "sa";
    private static final String PASSWORD = "";

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        sc.nextInt();
        try (Connection connection = DriverManager.getConnection(jdbc:mysql://@localhost:3306/candy","root","root)) {
            System.out.println(" Connected to H2 Database!");

            createTable(connection);
            insertStudent(connection, "Rahul", 23);
            insertStudent(connection, "Anita", 21);
            insertStudent(connection, "Kiran", 25);

            System.out.println("\n All Students:");
            fetchStudents(connection);

//            updateStudentAge(connection, 2, 22);
//            System.out.println("\n After Updating Anita's Age:");
//            fetchStudents(connection);
//
//            deleteStudent(connection, 1);
//            System.out.println("\n After Deleting Rahul:");
//            fetchStudents(connection);

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    private static void createTable(Connection connection) throws SQLException {
        String createTableSQL = "CREATE TABLE IF NOT EXISTS STUDENTS (" +
                "ID INT PRIMARY KEY AUTO_INCREMENT," +
                "NAME VARCHAR(100)," +
                "AGE INT)";
        try (Statement stmt = connection.createStatement()) {
            stmt.execute(createTableSQL);
            System.out.println(" Table created.");
        }
    }

    private static void insertStudent(Connection connection, String name, int age) throws SQLException {
        String insertSQL = "INSERT INTO STUDENTS (NAME, AGE) VALUES (?, ?)";
        try (PreparedStatement pstmt = connection.prepareStatement(insertSQL)) {
            pstmt.setString(1, name);
            pstmt.setInt(2, age);
            pstmt.executeUpdate();
            System.out.println(" Inserted: " + name);
        }
    }

    private static void fetchStudents(Connection connection) throws SQLException {
        String selectSQL = "SELECT * FROM STUDENTS";
        try (Statement stmt = connection.createStatement();
             ResultSet rs = stmt.executeQuery(selectSQL)) {
            while (rs.next()) {
                int id = rs.getInt("ID");
                String name = rs.getString("NAME");
                int age = rs.getInt("AGE");
                System.out.println(id + " | " + name + " | " + age);
            }
        }
    }

    private static void updateStudentAge(Connection connection, int id, int newAge) throws SQLException {
        String updateSQL = "UPDATE STUDENTS SET AGE=? WHERE ID=?";
        try (PreparedStatement pstmt = connection.prepareStatement(updateSQL)) {
            pstmt.setInt(1, newAge);
            pstmt.setInt(2, id);
            int rows = pstmt.executeUpdate();
            if (rows > 0) {
                System.out.println(" Updated student ID " + id + " age to " + newAge);
            }
        }
    }

    private static void deleteStudent(Connection connection, int id) throws SQLException {
        String deleteSQL = "DELETE FROM STUDENTS WHERE ID=?";
        try (PreparedStatement pstmt = connection.prepareStatement(deleteSQL)) {
            pstmt.setInt(1, id);
            int rows = pstmt.executeUpdate();
            if (rows > 0) {
                System.out.println(" Deleted student ID " + id);
            }
        }
    }
}