import java.sql.*;


public class ConnectionMySQL {
	public static void main(String args[])throws SQLException
	{
		Connection connection=null;
		try {
			 connection = DriverManager.getConnection("jdbc:mysql://localhost/emp","root", "");
			System.out.println("Connection Success");
	} catch (Exception e) {

		System.err.println(e);
	}
		float count=0;//counting numbers of smokers
		float count1=0;//for non smokers
		 float count3=0;//counting for family and smoking (fh=yes and smoking =yes)
	        float count4=0;//counting for family and smoking (fh=yes and smoking =No)

	        float count5=0;//counting for gender and smoking (gender=yes and smoking =yes)
	       float count6=0;//counting for gender and smoking (gender=yes and smoking =No)
			 float count7=0;
			 float count8=0;
			 
			 float count9=0;
			 float count10=0;
			 float count11=0;
			 float count12=0;
		        
		Statement st=connection.createStatement();
		 //ResultSet totalrows= st.executeQuery("SELECT COUNT(*) FROM employee");
        ResultSet rs=st.executeQuery("select * from employee");
        
        while(rs.next())
        {
            //System.out.println(rs.getString(1));
            //System.out.println(rs.getString(5));
        	
        	if(rs.getString(4).equals("Yes"))
        	{
        	count++;
        	}
        	if(rs.getString(4).equals("No"))
        	{
        	count1++;
        	}
        
        }
        System.out.println("Total numbers of yes:"+count);
        System.out.println("Total number of No:"+count1);
      
       
        float prob=count/30;//probability for smokers
        float prob1=count1/30;//probablity for non smokers
        System.out.println("Probability of yes the person smokes:"+prob);
        System.out.println("Probability of the person who do not smokes:"+prob1);
        
        
      ResultSet ob =st.executeQuery("select count(gender) from employee");
        //int val =  ((Number) ob.getObject(1)).intValue();
        //System.out.println(val);
        int id;
        if(ob.next()) {
			 id = ob.getInt(1);

		}
        
      //  System.out.println("No. of rows"+id);
      
            //System.out.println(rs.getString(1));
            //System.out.println(rs.getString(5));
        ResultSet rs1=st.executeQuery("select * from employee");
        
        while(rs1.next())
        {
        	if(rs1.getString(4).equals("Yes")&&rs1.getString(3).equals("Yes"))

        	{
        		
        	count3++;
        	}
        	if(rs1.getString(4).equals("No")&&rs1.getString(3).equals("Yes"))
        	{
        	count4++;
        	}
           }
        float probfamilyhist=count3/count;
        float probfamilyhistnosmk=count4/count1;
        System.out.println("Probability of familyhistory and who smokes:"+probfamilyhist);
        System.out.println("Probability of familyhistory and who not smokes:"+probfamilyhistnosmk);
        
        
        ResultSet rs2=st.executeQuery("select * from employee");
        while(rs2.next())
        {
        if(rs2.getString(1).equals("M")&&rs2.getString(4).equals("Yes"))//checking gender with smoking 
    	{
    		count5++;
    		}
    	if(rs2.getString(1).equals("M")&&rs2.getString(4).equals("No"))
    	{
    		count6++;
    	}
    	if(rs2.getString(5).equals("Yes")&&rs2.getString(4).equals("Yes"))//checking smoking with cough blood with smoking ='yes'
    	{
    		count7++;
    	}
    	if(rs2.getString(5).equals("Yes")&&rs2.getString(4).equals("No"))///checking smoking with cough blood with smoking ='No'
    	{
    		count8++;
    	}
       

    
    	if(rs2.getString(8).equals("Yes")&&rs2.getString(4).equals("Yes"))///checking smoking with environment with smoking ='Yes'
    	{
    		count9++;
    	}
    	if(rs2.getString(8).equals("Yes")&&rs2.getString(4).equals("No"))///checking smoking with environment with smoking ='No'
    	{
    		count10++;
    	}
        }
    	
    	 ResultSet rs6=st.executeQuery("select * from employee");
    	 while(rs6.next())
    	 {
    	
    	if(rs6.getString(4).equals("Yes")&&rs6.getString(9).equals("Yes"))///checking chest pain with smoking ='yes'
    	{
    		count11++;
    	}
    	if(rs6.getString(4).equals("No")&&rs6.getString(9).equals("Yes"))///checking chest pain  with smoking ='No'
    	{
    		count12++;
    	}
    	
	}
        
        
        float probcough=count7/count;
        float probcoughnosm=count8/count1;
        System.out.println("Probability having cough with blood and who smokes:"+probcough);
      System.out.println("Probability having cough with blood and who do not smokes:"+probcoughnosm);
        
        float probenv=count9/count;
        float probenvwithnosm=count10/count1;
        System.out.println("Probability who is working in environment(asbestos) and who smokes:"+probenv);
        System.out.println("Probability who is working in environment(asbestos) and who not smokes:"+probenvwithnosm);
        
        
        float probchest=count11/count;
        float probchestnosmk=count12/count1;
       // System.out.println(count11);
        //System.out.println(count12);
        
        
        System.out.println("Probability who is havig chest pain and who smokes:"+probchest);
        System.out.println("Probability who is having chest pain and who do not smokes:"+probchestnosmk);
        
    
        
     
        
        
       float probXforsmoking=probfamilyhist*probcough*probenv*probchest;
        System.out.println("Probability with given training dataset and who smokes:"+probXforsmoking);
        float probXfornosmkoing=probfamilyhistnosmk*probcoughnosm*probenvwithnosm*probchestnosmk;
        System.out.println("Probability with given training dataset and who do not smokes:"+ probXfornosmkoing);
        
        float finalop=probXforsmoking*prob;
        float finalop2=probXfornosmkoing*prob1;
        System.out.println("Final Probability of smoking"+finalop);
        System.out.println("Final Probability of Not smoking"+finalop2);
        
        if(finalop>finalop2)
        {
        	System.out.println("Yes the people according to given data have higher probability of Lungs cancer ");
        	
        }
        else
        {
        	
        	System.out.println("The  people are safe and have less probabity of having Lungs cancer let us scan their image  ");
        	
        }
       
        
        
        
	}

	}


