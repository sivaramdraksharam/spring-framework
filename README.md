How to work with spring jdbc?

Working with Spring JDBC primarily involves using the JdbcTemplate class, which simplifies database interactions by handling boilerplate JDBC code like connection management, statement creation, and resource cleanup.

Steps to work with Spring JDBC:
•	Add Dependencies: Include the necessary Spring JDBC and database driver dependencies in your project's build file (e.g., pom.xml for Maven or build.gradle for Gradle).
Code
    <!-- Maven example -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>6.x.x</version> <!-- Use appropriate version -->
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.x.x</version> <!-- Use appropriate version for your DB -->
    </dependency>
•	Configure a DataSource: Define a DataSource bean in your Spring configuration (Java-based or XML-based). This DataSource provides the connection to your database.
    // Java-based configuration example
    @Configuration
    public class AppConfig {
        @Bean
        public DataSource dataSource() {
            DriverManagerDataSource dataSource = new DriverManagerDataSource();
            dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
            dataSource.setUrl("jdbc:mysql://localhost:3306/your_database");
            dataSource.setUsername("your_username");
            dataSource.setPassword("your_password");
            return dataSource;
        }
    }
•	Create a JdbcTemplate Bean: Create a JdbcTemplate bean and inject the configured DataSource into it.

    @Bean
    public JdbcTemplate jdbcTemplate(DataSource dataSource) {
        return new JdbcTemplate(dataSource);
    }
•	Perform Database Operations: Use the JdbcTemplate methods to execute SQL queries and updates.
o	update(): For INSERT, UPDATE, DELETE statements.

        jdbcTemplate.update("INSERT INTO users (name, email) VALUES (?, ?)", "John Doe", "john.doe@example.com");
•	queryForObject(): For retrieving a single row and mapping it to an object.

        String name = jdbcTemplate.queryForObject("SELECT name FROM users WHERE id = ?", String.class, 1);
•	query(): For retrieving multiple rows and mapping them to a list of objects using a RowMapper.

        List<User> users = jdbcTemplate.query("SELECT id, name, email FROM users", (rs, rowNum) -> {
            User user = new User();
            user.setId(rs.getInt("id"));
            user.setName(rs.getString("name"));
            user.setEmail(rs.getString("email"));
            return user;
        });
•	batchUpdate(): For executing multiple INSERT, UPDATE, or DELETE statements in a batch.

        List<Object[]> userData = new ArrayList<>();
        userData.add(new Object[]{"Jane Doe", "jane.doe@example.com"});
        userData.add(new Object[]{"Peter Pan", "peter.pan@example.com"});
        jdbcTemplate.batchUpdate("INSERT INTO users (name, email) VALUES (?, ?)", userData);
By using JdbcTemplate, you abstract away the complexities of raw JDBC, leading to cleaner and more maintainable data access code in your Spring applications.


