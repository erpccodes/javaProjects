# SatApplication

The SatApplication is a Java program that interacts with a MySQL database using JDBC. It provides functionality to insert data, view data, get ranks, update scores, and delete records in the `sat_results` table.

## Prerequisites

To run the SatApplication, you need to have the following installed on your machine:
- Java Development Kit (JDK)
- MySQL database server
- MySQL Connector/J driver

## Getting Started

1. Clone the repository or download the source code files.

2. Ensure that you have the MySQL Connector/J driver JAR file available. If not, download the driver from the official MySQL website.

3. Set up the MySQL database:
   - Create a database named `sat_results`.
   - Create the `sat_results` table with the required structure:
     ```sql
     CREATE TABLE IF NOT EXISTS sat_results (
         id INT AUTO_INCREMENT PRIMARY KEY,
         name VARCHAR(255) NOT NULL UNIQUE,
         address VARCHAR(255) NOT NULL,
         city VARCHAR(255) NOT NULL,
         country VARCHAR(255) NOT NULL,
         pincode VARCHAR(255) NOT NULL,
         sat_score INT NOT NULL,
         passed BOOLEAN NOT NULL
     );
     ```

4. Add the MySQL Connector/J driver to the project:
   - Download the MySQL Connector/J driver from the official MySQL website.
   - Copy the downloaded JAR file to the project's "lib" directory.
   - Add the JAR file to the project's classpath.

5. Open the project in your preferred Java IDE.

6. Update the database connection details:
   - Open the `SatApplication.java` file.
   - Update the `DB_URL`, `DB_USER`, and `DB_PASSWORD` constants with your MySQL database connection details.

7. Build and run the application:
   - Compile and run the `SatApplication.java` file.

8. Use the menu-driven interface to interact with the application.

## Troubleshooting

- If you encounter any issues related to database connectivity, ensure that your MySQL database server is running and accessible with the correct connection details.
- Double-check that you have correctly added the MySQL Connector/J driver to the project's classpath.
- Verify that the `sat_results` table is created in the database with the correct structure.

