# JDBC 
	Step 1 : Load jdbc driver
		Class.forName("oracle.jdbc.driver.OracleDriver");
		or 
		Driver myDriver = new oracle.jdbc.driver.OracleDriver();
		DriverManager.registerDriver( myDriver );
		
	Step 2 : Get Connection 
		String URL = "jdbc:oracle:thin:username/password@amrood:1521:EMP";
		Connection conn = DriverManager.getConnection(URL);
	------------------------------------------------
	Step 3: Create Statement
		Statement stmt = conn.createStatement( );
	
	Step 4 : Execute Statement 
		boolean execute (String SQL)
		int executeUpdate (String SQL)
		ResultSet executeQuery (String SQL)
	-------------------------------------------------
	Step 3 : PreparedStatement 
		The PreparedStatement interface extends the Statement interface, which gives you added functionality.
		String SQL = "Update Employees SET age = ? WHERE id = ?";	
		PreparedStatement pstmt = conn.prepareStatement(SQL)

		String SQL = "{call getEmpName (?, ?)}";
   		CallableStatement cstmt = conn.prepareCall (SQL);
	
	-----------------------------------------------------
		Statement stmt = conn.createStatement( );
		String sql = "SELECT id, first, last, age FROM Employees";
		ResultSet rs = stmt.executeQuery(sql);

		int id = rs.getInt(1);
		if( rs.wasNull( ) ) {
		   id = 0;
		}
	
	Step 5 : Close connection
		stmt.close();
		conn.close();

# Transactions

	try{
		//Assume a valid connection object conn
		conn.setAutoCommit(false);
		Statement stmt = conn.createStatement();
		String SQL = "INSERT INTO Employees VALUES (106, 20, 'Rita', 'Tez')";
		stmt.executeUpdate(SQL);  
		//Submit a malformed SQL statement that breaks
		String SQL = "INSERTED IN Employees VALUES (107, 22, 'Sita', 'Singh')";
		stmt.executeUpdate(SQL);
		// If there is no error.
		conn.commit();
	}catch(SQLException se){
		// If there is any error.
		conn.rollback();
	}
	
# Transactions Save Point
	try{
		//Assume a valid connection object conn
		conn.setAutoCommit(false);
		Statement stmt = conn.createStatement();

		//set a Savepoint
		Savepoint savepoint1 = conn.setSavepoint("Savepoint1");
		String SQL = "INSERT INTO Employees VALUES (106, 20, 'Rita', 'Tez')";
		stmt.executeUpdate(SQL);  
		//Submit a malformed SQL statement that breaks
		String SQL = "INSERTED IN Employees VALUES (107, 22, 'Sita', 'Tez')";
		stmt.executeUpdate(SQL);
		// If there is no error, commit the changes.
		conn.commit();
	}catch(SQLException se){
	   // If there is any error.
	   conn.rollback(savepoint1);
	}
	
# JDBC - Batch Processing
	
	// Create statement object
	Statement stmt = conn.createStatement();

	// Set auto-commit to false
	conn.setAutoCommit(false);

	// Create SQL statement
	String SQL = "INSERT INTO Employees (id, first, last, age) VALUES(200,'Zia', 'Ali', 30)";
	// Add above SQL statement in the batch.
	stmt.addBatch(SQL);

	// Create one more SQL statement
	String SQL = "INSERT INTO Employees (id, first, last, age) VALUES(201,'Raj', 'Kumar', 35)";
	// Add above SQL statement in the batch.
	stmt.addBatch(SQL);

	// Create one more SQL statement
	String SQL = "UPDATE Employees SET age = 35 WHERE id = 100";
	// Add above SQL statement in the batch.
	stmt.addBatch(SQL);

	// Create an int[] to hold returned values
	int[] count = stmt.executeBatch();

	//Explicitly commit statements to apply changes
	conn.commit();
	