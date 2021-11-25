import java.io.*;
import java.util.*;
import java.util.Scanner;
class school
{
    String name;
    ArrayList<admin> admins;
    ArrayList<student> students;
    ArrayList<teacher> teachers;
    ArrayList<course> courses;
    
    school(String name)
    {
        this.name = name;
        students = new ArrayList<student>();
        teachers = new ArrayList<teacher>();
        courses = new ArrayList<course>();
        admins = new ArrayList<admin>();
        System.out.println("Welcome to " + name);
    }
}

abstract class user
{
    String name;
    String email;
    String phone;
    
    user()
    {
        System.out.println("New User:");
    }
    
    public void basic_details(String name, String email,String phone)
    {
        this.name = name;
        this.email = email;
        this.phone = phone;
    }   
    
    public void get_details()
    {
        System.out.println("email : " + email);
        System.out.println("phone : " + phone);
    }
}

class admin extends user
{
    admin(String name, String email,String phone)
    {
        super.basic_details(name,email,phone);
        System.out.println("Admin is added");
    } 
    
    public student add_new_student(String name, String email,String phone, int year_of_admission, int class_enrolled)
    {
        student st = new student(name, email, phone, year_of_admission, class_enrolled); 
        return st;
    }
    
    public teacher add_new_teacher(String name, String email,String phone)
    {
        teacher t = new teacher(name, email, phone); 
        return t;
    }
    
    public course add_new_course(String name, String course_id,String t)
    {
        course c = new course(name, course_id, t); 
        return c;
    }
    
    public void get_details()
    {   
        System.out.println("Details of Admin - " + name);
        super.get_details();
    }
    
    public void modify_student_details(student s,String student_name, String email, String phone, String ID,int attendance)
    {
        System.out.println("Details of Student - " + s.name + " are modified by admin");
        s.name = student_name;
        s.email = email;
        s.phone = phone;
        s.ID = ID;
        s.attendance = attendance;
    }
    
    public void add_student_marks(course c, student s, int attendance, int marks, String grade)
    {
        c.add_student_marks(s, attendance, marks, grade);
    }
    
    public void modify_student_marks(course c, student st, int attendance, int marks, String grade)
    {
        c.modify_student_marks(st, attendance, marks, grade);
    }
    
    public void modify_teacher_details(teacher t,String teacher_name, String email, String phone)
    {
         System.out.println("Details of Teacher - " + t.name + " are modified by admin");
        t.name = teacher_name;
        t.email = email;
        t.phone = phone;
    }
    
    public void modify_course_details(course c,String name, String course_id, String teacher)
    {
        System.out.println("Details of Course - " + c.name + " are modified by admin");
        c.name = name;
        c.course_id = course_id;
        c.teacher = teacher;
        
    }
    
    public void delete_course(course c)
    {
        c.t.courses.remove(c);
        for(student s: c.students)
        {
            s.courses.remove(c);
        }
        System.out.println("Course - " + c.name + " is deleted");
        c = null;
    }
    
    public void delete_student(student s)
    {
        s.total_students = s.total_students - 1;
        for(course c: s.courses)
        {
            c.students.remove(s);
            c.attendance_of_students.remove(s);
            c.marks_of_students.remove(s);
            c.grades_of_students.remove(s);
        }
        System.out.println("Student - " + s.name + " is deleted");
        s = null;
    }
    
    public void delete_student_records(course c, student s)
    {
        c.delete_student_records(s);
    }
    
    public void delete_teacher(teacher t)
    {   
        System.out.println("Teacher - " + t.name + " is deleted");
        for(course c : t.courses)
        {
            c.teacher = "NONE";
            c.t = null;
        }
        t = null;
    }
    
    
}

class teacher extends user
{
    ArrayList<course> courses;
    
    teacher(String name, String email,String phone)
    {   
        super.basic_details(name,email,phone);
        courses = new ArrayList<course>();
        System.out.println("Teacher is added : " + name);
    }
    
    public void get_details()
    {
        System.out.println("Details of Teacher - " + name);
        System.out.println("email : " + email);
        System.out.println("phone : " + phone);
        System.out.print("Courses : ");
        
        if(courses.size() == 0)
          System.out.println("None"); 
        else
          System.out.println(courses.size());  
          
        for(int i = 0; i < courses.size(); i++ )
        {
            course curr = courses.get(i);
            System.out.println(i+1 + ". " + curr.name); 
        }
        
    }
    
    public void add_courses(course c)
    {
        courses.add(c);
        c.teacher = name;
        c.t = this;
        System.out.println("Course " + c.name + " is now under Professor " + c.teacher );
        
    }
    
    public void modify_student_marks(course c, student s, int attendance, int marks, String grade)
    {
        c.modify_student_marks(s, attendance, marks, grade);
    }
    
    public void add_student_marks(course c, student s, int attendance, int marks, String grade)
    {
        c.add_student_marks(s, attendance, marks, grade);
    }
    
    public void delete_student_records(course c, student s)
    {
        c.delete_student_records(s);
    }
    
}

class student extends user
{
    static int total_students = 0;
    static int total_admissions = 0;
    String ID;
    int attendance;
    ArrayList<course> courses;
    
    student(String name, String email,String phone, int year_of_admission, int class_enrolled)
    {
        super.basic_details(name,email,phone);
        total_students = total_students + 1;
        total_admissions = total_admissions + 1;
        courses = new ArrayList<course>();
        attendance = 85;
        ID = Integer.toString(year_of_admission) + "C" + Integer.toString(class_enrolled) + "TS" + Integer.toString(total_admissions);
        System.out.println("Student is added : " + name);
    }
    
    public void get_details()
    {
        System.out.println("Details of Student - " + name);
        System.out.println("ID : " + ID);
        System.out.println("Attendance : " + attendance );
        System.out.println("email : " + email);
        System.out.println("phone : " + phone);
        System.out.print("Courses enrolled : ");
        
        if(courses.size() == 0)
            System.out.println("None");  
        else
            System.out.println(courses.size());
          
        for(int i = 0; i < courses.size(); i++ )
        {   
           course curr = courses.get(i);
           System.out.println(i+1 + ". " + curr.name);
           System.out.println( "Grade : " + curr.grades_of_students.get(this));
           System.out.println( "Marks : " + curr.marks_of_students.get(this));
           
        }
        
    }
    
    public void add_courses(course c)
    {
        courses.add(c);
        c.students.add(this);
        System.out.println("Student " + name + " is now enrolled in course " + c.name);
        
    }
}

class course
{
    String name;
    String course_id;
    String  teacher;
    HashSet<student> students;
    HashMap<student, Integer> attendance_of_students;
    HashMap<student, Integer> marks_of_students;
    HashMap<student, String> grades_of_students;
    teacher t;
    
    course(String name, String course_id, String teacher)
    {
        this.name = name;
        this.teacher = teacher;
        this.course_id = course_id;
        students = new HashSet<student>();
        attendance_of_students = new HashMap<>();
        marks_of_students = new HashMap<>();
        grades_of_students = new HashMap<>();
        System.out.println("New course " + name + " is added under Professor " + teacher);
        
    }
    
    public void get_details()
    {
        System.out.println("Course : " + name);
        System.out.println("Course_id : " + course_id);
        System.out.println("Professor : " + teacher);
        System.out.print("Students enrolled : ");
        
        if(students.size() == 0)
            System.out.println("None");
        else    
            System.out.println(students.size());
            
        int count = 0;
        for( student s : students)
        {   
            
            count = count + 1;
            System.out.println(count + ") " + s.name );
            System.out.println("Attendace " + attendance_of_students.get(s) );
            System.out.println("Marks " + marks_of_students.get(s) );
            System.out.println("Grade " + grades_of_students.get(s) );
            
        }
    }
    
    public void add_student_marks(student s, int attendance, int marks, String grade)
    {
        students.add(s);
        attendance_of_students.put(s , attendance);
        marks_of_students.put(s , marks);
        grades_of_students.put(s , grade);
        System.out.println("Student " + s.name + " marks are added in course " + name);
    }
    
    public void modify_student_marks(student s, int attendance, int marks, String grade)
    {
        attendance_of_students.put(s , attendance);
        marks_of_students.put(s , marks);
        grades_of_students.put(s, grade);
        System.out.println("Student " + s.name + " marks are modified in course " + name);
    }
    
    public void delete_student_records(student s)
    {
        students.remove(s);
        attendance_of_students.remove(s);
        marks_of_students.remove(s);
        grades_of_students.remove(s);
        System.out.println("Student " + s.name + " is removed from course " + name);
    } 
}

public class Main
{
	public static void main(String[] args) {
		System.out.println("Welcome To School Management System");
		school s = new school("BITS Pilani , Pilani Campus");
		admin Admin = new admin("Admin","admin@bitspilani-pilani.ac.in","+91-1596-255220");
		s.admins.add(Admin);
		
		Scanner sc = new Scanner(System.in);
		
		while(true)
		{   
		    System.out.println("Choose either - New, Add, Delete, View, Modify details/marks, Delete marks END ");
		    String op = sc.nextLine();   
		    System.out.println(op);
		    if(op.equals("END"))
		    {
		        break;
		    }
		    else if(op.equals("New"))
		    {
		        System.out.println("Please enter: new_type - student teacher course");
		        String new_type = sc.next();
		        
		        if(new_type.equals("student"))
		        {   
		            System.out.println("Please enter: name email phone year_of_admission class_enrolled");
		            String name = sc.next();
		            String email = sc.next();
		            String phone = sc.next();
		            int year_of_admission =  sc.nextInt();
		            int class_enrolled = sc.nextInt();
		            
		            student new_student = Admin.add_new_student(name, email, phone, year_of_admission, class_enrolled );
		            s.students.add(new_student);
		            sc.nextLine();
		        }
		        
		        else if(new_type.equals("teacher"))
		        {   
		            System.out.println("Please enter: name email phone ");
		            String name = sc.next();
		            String email = sc.next();
		            String phone = sc.next();
		            
		            teacher new_teacher = Admin.add_new_teacher(name, email, phone);
		            s.teachers.add(new_teacher);
		            sc.nextLine();
		        }
		        
		        else if(new_type.equals("course"))
		        {   
		            System.out.println("Please enter: name course_id teacher ");
		            String name = sc.next();
		            String course_id = sc.next();
		            String t = sc.next();
		            
		            course new_course = Admin.add_new_course(name, course_id, t);
		            s.courses.add(new_course);
		            sc.nextLine();
		        }
		        
		        else 
		        {   
		            System.out.println("Invalid new_type");
		            sc.nextLine();
		        }
		        
		    }
		    
		    else if(op.equals("Add"))
		    {
		        System.out.println("Please enter: add_type - marks course");
		        String add_type = sc.next();
		        
		        if(add_type.equals("marks"))
		        {   
		            System.out.println("Please enter: course_name student_id attendance marks grade changed_by name");
		            String course_name = sc.next();
		            String student_id = sc.next();
		            int attendance = sc.nextInt();
		            int marks =  sc.nextInt();
		            String grade = sc.next();
		            String changed_by = sc.next();
		            String by = sc.next();
		            
		            course cur = null;
		            for(int j = 0; j < s.courses.size(); j++)
		                {
		                   course chk = s.courses.get(j);
		                   if(chk.name.equals(course_name))
		                       cur = chk ;
		                }
		            
		            student std = null ;
		            for(int j = 0; j < s.students.size(); j++)
		                {
		                   student chk = s.students.get(j);
		                   if(chk.ID.equals(student_id))
		                       std = chk ;
		                } 
		            
		            if(cur == null || std == null)
		            {
		                System.out.println("Wrong query!");
		            }
		            else if(changed_by.equals("teacher"))
		            {
		                teacher tch = null ;
		                for(int j = 0; j < s.teachers.size(); j++)
		                {
		                   teacher chk = s.teachers.get(j);
		                   if(chk.name.equals(by))
		                       tch = chk ;
		                }
		                
		                if(tch == null)
		                    System.out.println("Wrong query!");
		                else
		                {
		                    tch.add_student_marks(cur, std, attendance, marks, grade);
		                }      
		            }
		            else if(changed_by.equals("admin"))
		            {
		                Admin.add_student_marks( cur, std, attendance, marks, grade);
		            }
		            else
		            {
		                System.out.println("Wrong query!");
		            }   
		            sc.nextLine();
		            
		        }
		        
		        else if(add_type.equals("course"))
		        {   
		            System.out.println("Please enter: course_name student/teacher id/name ");
		            String course_name = sc.next();
		            String user_tp = sc.next();
		            String user_id = sc.next();
		            
		            course cur = null ;
		            for(int j = 0; j < s.courses.size(); j++)
		                {
		                   course chk = s.courses.get(j);
		                   if(chk.name.equals(course_name))
		                       cur = chk ;
		                }
		            
		            if(user_tp.equals("teacher"))
		                {
		                      teacher tch = null ;
		                      for(int j = 0; j < s.teachers.size(); j++)
		                      {
		                        teacher chk = s.teachers.get(j);
		                        if(chk.name.equals(user_id))
		                            tch = chk ;
		                      }
		                      
		                      if(cur == null || tch == null)
		                        System.out.println("Wrong query!");
		                      else
		                      {
		                        tch.add_courses(cur);
		                      }  
		                }
		            else if(user_tp.equals("student"))
		                {
		                      student std = null;
		                      for(int j = 0; j < s.students.size(); j++)
		                      {
		                        student chk = s.students.get(j);
		                        if(chk.ID.equals(user_id))
		                            std = chk ;
		                       } 
		                      
		                      if(cur == null || std == null)
		                        System.out.println("Wrong query!");
		                      else  
		                      {
		                        std.add_courses(cur);
		                      } 
		                }  
		           else
		                {
		                    System.out.println("Wrong query");
		                } 
		                sc.nextLine();
		        }
		        
		        
		        else 
		        {   
		            System.out.println("Invalid add_type");
		            sc.nextLine();
		        }
		        
		    }
		    
		    else if(op.equals("Delete"))
		    {
		         System.out.println("Please enter: course/teacher/student name/name/id ");
		         String user_tp = sc.next();
		         String user_id = sc.next();
		         
		         if(user_tp.equals("teacher"))
		                {
		                      teacher tch = null ;
		                      for(int j = 0; j < s.teachers.size(); j++)
		                      {
		                        teacher chk = s.teachers.get(j);
		                        if(chk.name.equals(user_id))
		                            tch = chk ;
		                      } 
		                      if(tch == null)
	                                System.out.println("This teacher does not exist");
	                          else
	                          {
		                        s.teachers.remove(tch);
		                        Admin.delete_teacher(tch);
	                          }
		                }
	            else if(user_tp.equals("student"))
	                {
	                      student std = null;
	                      for(int j = 0; j < s.students.size(); j++)
	                      {
	                        student chk = s.students.get(j);
	                        if(chk.ID.equals(user_id))
	                            std = chk ;
	                       }
	                       
	                      if(std == null)
	                        System.out.println("This student does not exist");
	                      else
	                      {
	                        s.students.remove(std);
	                        Admin.delete_student(std);
	                      }
	                }  
	            else if(user_tp.equals("course"))
	                {
	                      course cur = null;
		                  for(int j = 0; j < s.courses.size(); j++)
		                  {
		                   course chk = s.courses.get(j);
		                   if(chk.name.equals(user_id))
		                       cur = chk ;
		                  }
	                      if(cur == null)
	                        System.out.println("This course does not exist");
	                      else
	                      {
	                        s.courses.remove(cur);
	                        Admin.delete_course(cur);
	                      }
	                }
	            else
	                {
	                    System.out.println("Wrong query!");
	                }
	                sc.nextLine();
		    }
		    
		    else if(op.equals("View"))
		    {
		         System.out.println("Please enter: course/teacher/student name/name/id ");
		         String user_tp = sc.next();
		         String user_id = sc.next();
		         System.out.println( user_tp + " " + user_id);
		         if(user_tp.equals("teacher"))
		                {
		                      teacher tch = null ;
		                      for(int j = 0; j < s.teachers.size(); j++)
		                      {
		                        teacher chk = s.teachers.get(j);
		                        if(chk.name.equals(user_id))
		                            tch = chk ;
		                      } 
		                      
		                      if(tch == null)
	                                System.out.println("This teacher does not exist");
	                          else
	                          {
		                        tch.get_details();
	                          }  
		                }
	            else if(user_tp.equals("student"))
	                {
	                      student std = null ;
	                      for(int j = 0; j < s.students.size(); j++)
	                      {
	                        student chk = s.students.get(j);
	                        if(chk.ID.equals(user_id))
	                            std = chk ;
	                       } 
	                      
	                      if(std == null)
	                        System.out.println("This student does not exist");
	                      else
	                      {
	                        std.get_details();
	                      }     
	                }  
	            else if(user_tp.equals("course"))
	                {
	                      course cur = null;
		                  for(int j = 0; j < s.courses.size(); j++)
		                  {
		                   course chk = s.courses.get(j);
		                   if(chk.name.equals(user_id))
		                       cur = chk ;
		                  }
	                      if(cur == null)
	                        System.out.println("This course does not exist");
	                      else
	                      {
	                      cur.get_details();
	                      }
	                }
	            else
	                {
	                    System.out.println("Wrong query!");
	                }
	                sc.nextLine();
		    }
		    
		    else if(op.equals("Modify details"))
		    {
		        System.out.println("Please enter: course/teacher/student modified_by  ");
		        String user_tp = sc.next();
		        String modified_by = sc.next();
		        sc.nextLine();
		         
		         if(user_tp.equals("teacher"))
		                {
		                      System.out.println("name email phone ");
		                      String name = sc.next();
		                      String email = sc.next();
		                      String phone = sc.next();
		 
		                      teacher tch = null ;
		                      for(int j = 0; j < s.teachers.size(); j++)
		                      {
		                        teacher chk = s.teachers.get(j);
		                        if(chk.name.equals(phone))
		                            tch = chk ;
		                      } 
		                      
		                      if(tch == null || (!modified_by.equals("admin")))
	                                System.out.println("Invalid Modification!");
	                          else
	                          {
		                        Admin.modify_teacher_details(tch, name, email, phone);
	                          }  
	                          sc.nextLine();
		                }
	            else if(user_tp.equals("student"))
	                {
	                      System.out.println("name email phone ID attendance ");
	                      String name = sc.next();
	                      String email = sc.next();
	                      String phone = sc.next();
	                      String ID = sc.next();
	                      int attendance = sc.nextInt();
	                      
	                      
	                      student std = null ;
	                      for(int j = 0; j < s.students.size(); j++)
	                      {
	                        student chk = s.students.get(j);
	                        if(chk.ID.equals(ID))
	                            std = chk ;
	                       } 
	                      
	                      if(std == null || (!modified_by.equals("admin")))
	                        System.out.println("Invalid Modification!");
	                      else
	                      {
	                        Admin.modify_student_details(std, name, email, phone, ID, attendance);
	                      } 
	                      sc.nextLine();
	                }  
	            else if(user_tp.equals("course"))
	                {     
	                      System.out.println("name course_id teacher ");
	                      String name = sc.next();
	                      String course_id = sc.next();
	                      String teacher = sc.next();
	                      
	                      course cur = null;
		                  for(int j = 0; j < s.courses.size(); j++)
		                  {
		                   course chk = s.courses.get(j);
		                   if(chk.name.equals(name))
		                       cur = chk ;
		                  }
	                      if(cur == null || (!modified_by.equals("admin")))
	                        System.out.println("Invalid Modification!");
	                      else
	                      {
	                        Admin.modify_course_details(cur, name, course_id, teacher);
	                      }
	                      sc.nextLine();
	                }
	            else
	                {
	                   System.out.println("Invalid Modification!");  
	                }
	               
		    }
		    
		    else if(op.equals("Modify marks"))
		    {
    		      System.out.println("Please enter: course_name student_id attendance marks grade modified_by ");
    		      String course_name = sc.next();
                  String student_id = sc.next();
                  int attendance = sc.nextInt();
                  int marks = sc.nextInt();
                  String grade = sc.next();
                  String modified_by = sc.next();
        
                  course cur = null;
                  for(int j = 0; j < s.courses.size(); j++)
                  {
                   course chk = s.courses.get(j);
                   if(chk.name.equals(course_name))
                       cur = chk ;
                  }
                  
                  student std = null ;
                  for(int j = 0; j < s.students.size(); j++)
                  {
                    student chk = s.students.get(j);
                    if(chk.ID.equals(student_id))
                        std = chk ;
                  } 
                  
                   
                  
                  if(modified_by.equals("admin") || modified_by.equals("teacher") )
                    cur.modify_student_marks( std, attendance, marks, grade);
                  else 
                    System.out.println("Marks cannot be modified");
                    
                sc.nextLine();
		        
		    }
		    
		    else if(op.equals("Delete marks"))
		    {
    		      System.out.println("Please enter: course_name student_id  modified_by ");
    		      String course_name = sc.next();
                  String student_id = sc.next();
                  String modified_by = sc.next();
        
                  course cur = null;
                  for(int j = 0; j < s.courses.size(); j++)
                  {
                   course chk = s.courses.get(j);
                   if(chk.name.equals(course_name))
                       cur = chk ;
                  }
                  
                  student std = null ;
                  for(int j = 0; j < s.students.size(); j++)
                  {
                    student chk = s.students.get(j);
                    if(chk.ID.equals(student_id))
                        std = chk ;
                  } 
                  
                   
                  
                  if((modified_by.equals("admin") || modified_by.equals("teacher"))&&(cur != null && std != null) )
                    cur.delete_student_records(std);
                  else 
                    System.out.println("Marks cannot be modified");
                    
                sc.nextLine();
		        
		    }
		}
		
		
	}
}