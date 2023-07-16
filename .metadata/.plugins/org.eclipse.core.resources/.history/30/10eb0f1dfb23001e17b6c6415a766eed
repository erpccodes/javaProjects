package satApplication;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Scanner;

public class SatApplication {

	private static final String DB_URL = "jdbc:mysql://localhost:3306/sat_results";
    private static final String DB_USER = "root";
    private static final String DB_PASSWORD = "root";

    public static void main(String[] args) {
        try (Connection connection = DriverManager.getConnection(DB_URL, DB_USER, DB_PASSWORD)) {
            createTableIfNotExists(connection);

            Scanner scanner = new Scanner(System.in);
            int choice;

            do {
                System.out.println("1. Insert data");
                System.out.println("2. View all data");
                System.out.println("3. Get rank");
                System.out.println("4. Update score");
                System.out.println("5. Delete one record");
                System.out.println("0. Exit");
                System.out.print("Enter your choice: ");
                choice = scanner.nextInt();
                scanner.nextLine(); // Consume the newline character

                switch (choice) {
                    case 1:
                        insertData(connection, scanner);
                        break;
                    case 2:
                        viewAllData(connection);
                        break;
                    case 3:
                        getRank(connection, scanner);
                        break;
                    case 4:
                        updateScore(connection, scanner);
                        break;
                    case 5:
                        deleteRecord(connection, scanner);
                        break;
                    case 0:
                        System.out.println("Exiting the application.");
                        break;
                    default:
                        System.out.println("Invalid choice. Please try again.");
                        break;
                }
                System.out.println();
            } while (choice != 0);

            scanner.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    private static void createTableIfNotExists(Connection connection) throws SQLException {
        String query = "CREATE TABLE IF NOT EXISTS sat_results (" +
                "id INT AUTO_INCREMENT PRIMARY KEY," +
                "name VARCHAR(255) NOT NULL UNIQUE," +
                "address VARCHAR(255) NOT NULL," +
                "city VARCHAR(255) NOT NULL," +
                "country VARCHAR(255) NOT NULL," +
                "pincode VARCHAR(255) NOT NULL," +
                "sat_score INT NOT NULL," +
                "passed BOOLEAN NOT NULL" +
                ")";

        try (Statement statement = connection.createStatement()) {
            statement.executeUpdate(query);
        }
    }

    private static void insertData(Connection connection, Scanner scanner) throws SQLException {
        System.out.print("Enter Name: ");
        String name = scanner.nextLine();
        System.out.print("Enter Address: ");
        String address = scanner.nextLine();
        System.out.print("Enter City: ");
        String city = scanner.nextLine();
        System.out.print("Enter Country: ");
        String country = scanner.nextLine();
        System.out.print("Enter Pincode: ");
        String pincode = scanner.nextLine();
        System.out.print("Enter SAT Score: ");
        int satScore = scanner.nextInt();
        scanner.nextLine(); // Consume the newline character

        boolean passed = satScore > 30;
        String query = "INSERT INTO sat_results (name, address, city, country, pincode, sat_score, passed) " +
                "VALUES (?, ?, ?, ?, ?, ?, ?)";

        try (PreparedStatement statement = connection.prepareStatement(query)) {
            statement.setString(1, name);
            statement.setString(2, address);
            statement.setString(3, city);
            statement.setString(4, country);
            statement.setString(5, pincode);
            statement.setInt(6, satScore);
            statement.setBoolean(7, passed);

            int rowsInserted = statement.executeUpdate();
            if (rowsInserted > 0) {
                System.out.println("Data inserted successfully.");
            } else {
                System.out.println("Data insertion failed.");
            }
        }
    }

    private static void viewAllData(Connection connection) throws SQLException {
        String query = "SELECT * FROM sat_results";

        try (Statement statement = connection.createStatement();
             ResultSet resultSet = statement.executeQuery(query)) {
            while (resultSet.next()) {
                String name = resultSet.getString("name");
                String address = resultSet.getString("address");
                String city = resultSet.getString("city");
                String country = resultSet.getString("country");
                String pincode = resultSet.getString("pincode");
                int satScore = resultSet.getInt("sat_score");
                boolean passed = resultSet.getBoolean("passed");

                SATResult result = new SATResult(name, address, city, country, pincode, satScore, passed);
                System.out.println(result.toJSON());
            }
        }
    }

    private static void getRank(Connection connection, Scanner scanner) throws SQLException {
        System.out.print("Enter Name: ");
        String name = scanner.nextLine();

        String query = "SELECT name, sat_score, FIND_IN_SET(sat_score, " +
                "(SELECT GROUP_CONCAT(DISTINCT sat_score ORDER BY sat_score DESC) FROM sat_results)) AS `rank` " +
                "FROM sat_results " +
                "WHERE name = ?";

        try (PreparedStatement statement = connection.prepareStatement(query)) {
            statement.setString(1, name);

            try (ResultSet resultSet = statement.executeQuery()) {
                if (resultSet.next()) {
                    int rank = resultSet.getInt("rank");
                    System.out.println(name + " has a rank of " + rank);
                } else {
                    System.out.println("No data found for " + name);
                }
            }
        }
    }


    private static void updateScore(Connection connection, Scanner scanner) throws SQLException {
        System.out.print("Enter Name: ");
        String name = scanner.nextLine();
        System.out.print("Enter New SAT Score: ");
        int newScore = scanner.nextInt();
        scanner.nextLine(); // Consume the newline character

        boolean passed = newScore > 30;
        String query = "UPDATE sat_results SET sat_score = ?, passed = ? WHERE name = ?";

        try (PreparedStatement statement = connection.prepareStatement(query)) {
            statement.setInt(1, newScore);
            statement.setBoolean(2, passed);
            statement.setString(3, name);

            int rowsUpdated = statement.executeUpdate();
            if (rowsUpdated > 0) {
                System.out.println("Score updated successfully.");
            } else {
                System.out.println("No data found for " + name);
            }
        }
    }

    private static void deleteRecord(Connection connection, Scanner scanner) throws SQLException {
        System.out.print("Enter Name: ");
        String name = scanner.nextLine();

        String query = "DELETE FROM sat_results WHERE name = ?";

        try (PreparedStatement statement = connection.prepareStatement(query)) {
            statement.setString(1, name);

            int rowsDeleted = statement.executeUpdate();
            if (rowsDeleted > 0) {
                System.out.println("Record deleted successfully.");
            } else {
                System.out.println("No data found for " + name);
            }
        }
    }

    private static class SATResult {
        private String name;
        private String address;
        private String city;
        private String country;
        private String pincode;
        private int satScore;
        private boolean passed;

        public SATResult(String name, String address, String city, String country, String pincode, int satScore, boolean passed) {
            this.name = name;
            this.address = address;
            this.city = city;
            this.country = country;
            this.pincode = pincode;
            this.satScore = satScore;
            this.passed = passed;
        }

        public String getName() {
            return name;
        }

        public String getAddress() {
            return address;
        }

        public String getCity() {
            return city;
        }

        public String getCountry() {
            return country;
        }

        public String getPincode() {
            return pincode;
        }

        public int getSatScore() {
            return satScore;
        }

        public boolean isPassed() {
            return passed;
        }

        public String toJSON() {
            return "{"
                    + "\"name\":\"" + name + "\","
                    + "\"address\":\"" + address + "\","
                    + "\"city\":\"" + city + "\","
                    + "\"country\":\"" + country + "\","
                    + "\"pincode\":\"" + pincode + "\","
                    + "\"sat_score\":" + satScore + ","
                    + "\"passed\":" + passed
                    + "}";
        }
    }
}
