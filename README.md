//java_crud_operation

package jdbc;
import java.sql.*;
import java.util.*;

public class Demo_jdbc {
	   static void insert(int no,String name,String dob,String doj) {
		    String dbURL = "jdbc:mysql://localhost:3306/anytimedeveloper";
			String username = "root";
			String password = " "; 
		   try (Connection conn = DriverManager.getConnection(dbURL, username, password)) {
				
				String sql = "INSERT INTO STUDENT (STUDENT_NO, STUDENT_NAME, STUDENT_DOB, STUDENT_DOJ) VALUES (?, ?, ?, ?)";
				
				PreparedStatement statement = conn.prepareStatement(sql);
				statement.setInt(1, no);
				statement.setString(2, name);
				statement.setString(3, dob);
				statement.setString(4, doj);
				
				int rowsInserted = statement.executeUpdate();
				if (rowsInserted > 0) {
					System.out.println("A new user was inserted successfully!");
				}

				
			} catch (SQLException ex) {
				ex.printStackTrace();
			}		
 
	   }
	static void delete(String sname) {
		String dbURL = "jdbc:mysql://localhost:3306/anytimedeveloper";
		String username = "root";
		String password = " ";
		try (Connection conn = DriverManager.getConnection(dbURL, username, password)) {
			
			String sql = "DELETE FROM STUDENT WHERE STUDENT_NAME=?";
			
			PreparedStatement statement = conn.prepareStatement(sql);
			statement.setString(1, sname);
			
			int rowsDeleted = statement.executeUpdate();
			if (rowsDeleted > 0) {
				System.out.println("A user was deleted successfully!"+rowsDeleted);
			}
			
		} catch (SQLException ex) {
			ex.printStackTrace();
		}
	}
	static void update(String name,String dob,String doj,int no) {
		String dbURL = "jdbc:mysql://localhost:3306/anytimedeveloper";
		String username = "root";
		String password = " ";
		try (Connection conn = DriverManager.getConnection(dbURL, username, password)) {

			String sql = "UPDATE STUDENT SET STUDENT_NAME=?, STUDENT_DOB=?, STUDENT_DOJ=? WHERE STUDENT_NO=?";

			PreparedStatement statement = conn.prepareStatement(sql);
			statement.setString(1, name);
			statement.setString(2, dob);
			statement.setString(3, doj);
			statement.setInt(4, no);

			int rowsUpdated = statement.executeUpdate();
			if (rowsUpdated > 0) {
				System.out.println("An existing user was updated successfully!");
			}
			else {
				System.out.println("No Student found with id : "+no);
			}


		} catch (SQLException ex) {
			ex.printStackTrace();
		}
	}
	static void showuser() {
		String dbURL = "jdbc:mysql://localhost:3306/anytimedeveloper";
		String username = "root";
		String password = " ";
		try (Connection conn = DriverManager.getConnection(dbURL, username, password)) {
			
			String sql = "SELECT * FROM STUDENT";
			
			Statement statement = conn.createStatement();
			ResultSet result = statement.executeQuery(sql);
			
			int count = 0;
			
			while (result.next()){
				int student_no = result.getInt(1);
				String student_name = result.getString(2);
				
				String student_dob = result.getString("STUDENT_DOB");
				String student_doj = result.getString("STUDENT_DOJ");
				
				String output = "User #%d: %d - %s - %s - %s";
				System.out.println(String.format(output, ++count, student_no, student_name, student_dob, student_doj));
			}
			
		} catch (SQLException ex) {
			ex.printStackTrace();
		}
	}
	static void student_information() {
		String dbURL = "jdbc:mysql://localhost:3306/anytimedeveloper";
		String username = "root";
		String password = " ";
      try (Connection conn = DriverManager.getConnection(dbURL, username, password)) {
			
			String sql = "SELECT STUDENT_NAME FROM STUDENT WHERE STUDENT_NO=002";
			
			Statement statement = conn.createStatement();
			ResultSet result = statement.executeQuery(sql);
			 result.next(); //take your pointer to the next record because by default it is pointing to previous record
	        String name=result.getString("STUDENT_NAME");
	        	        System.out.println(name);

		} catch (SQLException ex) {
			ex.printStackTrace();
		}
	}
	public static void main(String[] args) {
	Scanner sc = new Scanner(System.in);
	int ch;
	do {
		System.out.println("Enter your choice");
		System.out.println("1.Insert the record");
		System.out.println("2.Update the record");
		System.out.println("3.Delete the record");
		System.out.println("4.Get the list of all student");
		System.out.println("5.Get one student information depending on the student id filter");
		System.out.println("0.Exit");
		ch=sc.nextInt();
		
		switch(ch) {
		case 1: System.out.println("Enter Student_no");
		        int no=sc.nextInt();
		        System.out.println("Enter Student Name");
		        String name=sc.next();
		        System.out.println("Enter student dob");
		        String dob=sc.next();
		        System.out.println("Enter Student doj");
		        String doj=sc.next();
		        
			    insert(no,name,dob,doj);
		         break;
		case 2:System.out.println("Enter Student_no");
               int num=sc.nextInt();
               System.out.println("Enter Student Name");
               String stuname=sc.next();
               System.out.println("Enter student dob");
               String stu_dob=sc.next();
               System.out.println("Enter Student doj");
               String stu_doj=sc.next();
        
			   update(stuname,stu_dob, stu_doj, num);
		         break;
		case 3: System.out.println("Enter student name to delete");
		        String sname=sc.next();
		        delete(sname);
		        break;
		case 4: showuser();
		        break;
		case 5: student_information();
		         break;
		case 0:System.out.println("Exit successful");
			    System.exit(0);
		          break;
		 default:
			 System.out.println("Enter valid choice");
			 break;
		}
	}while(ch != 0);
 }
}
