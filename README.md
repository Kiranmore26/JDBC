# JDBC

package MySQL;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Scanner;

public class pro1 
{
	Scanner sc=new Scanner(System.in);
	public static void main(String[] args)
	{
		pro1 obj=new pro1();
		//obj.insert_records();
		//obj.view_All_Data();
		obj.Book_Operation();

	}
	public void Book_Operation()
	{
		
		try 
		{
			Class.forName("com.mysql.cj.jdbc.Driver");
			Connection conn=DriverManager.getConnection(
					"jdbc:mysql://localhost:3306/school","root","root");
			
			while(true)
			{
				System.out.println("Enter Your Choice");
				System.out.println("1:Create table");
				System.out.println("2:Insert Records");
				System.out.println("3:View Record");
				System.out.println("4:Delete Record");
				System.out.println("5:Update Record");
				System.out.println("6:Exit");
				int ch=sc.nextInt();
				switch(ch)
				{
				case 1:
					create_table(conn);
					break;
				case 2:
					insert_records(conn);
					break;
				case 3:
					view_All_Data(conn);
					break;
				case 4:
					delte_rec(conn);
					break;
				case 5:
					update_red(conn);
					break;
				case 6:
					System.exit(0);
					break;
				default:
					System.out.println("Invalid Choice");
					break;
				}
			}
			
			//conn.close();
		} 
		catch (Exception e) 
		{
			
			System.err.println(e.getMessage());
		} 
		
		
	}
	public void create_table(Connection conn) throws SQLException
	{
		
			String qur="create table book (b_id int primary key,b_name varchar(30),"
					+ "b_price double,auth_name varchar(40))";
			Statement st=conn.createStatement();//3
			st.execute(qur);
			System.out.println("Table created Succesfully");

	}

	public void insert_records(Connection conn) throws SQLException

	{
			String qur="insert into book value(?,?,?,?)";
			PreparedStatement ps=conn.prepareStatement(qur);
			int b_id;
			double b_pr;
			String b_name,a_name;
			System.out.println("Enter book id");
			b_id=sc.nextInt();
			System.out.println("Enter Book name");
			b_name=sc.next();
			System.out.println("Enter Book price");
			b_pr=sc.nextDouble();
			System.out.println("Enter Author name");
			a_name=sc.next();
			
			ps.setInt(1, b_id);
			ps.setString(2, b_name);
			ps.setDouble(3, b_pr);
			ps.setString(4, a_name);
			
			int count=ps.executeUpdate();
			if(count>0)
			{
				System.out.println("Record added :)");
			}
			
	}

	public void view_All_Data(Connection conn) throws SQLException 
	{
			String qur="select * from book";
			Statement st=conn.createStatement();
			ResultSet rs= st.executeQuery(qur);
			System.out.println("Book ID\tBook Name\tBook Price\tBook AUthor");
			while(rs.next())
			{
				System.out.println(rs.getInt(1)+"\t"+rs.getString(2)+"\t\t"+rs.getDouble(3)+"\t\t"+rs.getString(4));
			}
		
	}
	
	public void delte_rec(Connection conn) throws SQLException
	{
		int b_id;
		System.out.println("Enter Book id");
		b_id=sc.nextInt();
		String qur="delete from book where b_id=?";
		PreparedStatement ps=conn.prepareStatement(qur);
		
		ps.setInt(1, b_id);
		
		ps.executeUpdate();
		System.out.println("Deleted");
	}

	public void update_red(Connection conn) throws SQLException
	{
		int b_id;
		double b_price;
		System.out.println("Enter Book id");
		b_id=sc.nextInt();
		System.out.println("Enter Book Price");
		b_price=sc.nextDouble();
		String qur="update book set b_price=? where b_id=?";
		PreparedStatement ps=conn.prepareStatement(qur);
		ps.setDouble(1, b_price);
		ps.setInt(2, b_id);
		
		ps.executeUpdate();
		System.out.println("Updated");
	}
}
