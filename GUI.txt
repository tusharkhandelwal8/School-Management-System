public class school extends javax.swing.JFrame {

    /**
     * Creates new form school
     */
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
        msg_area.setText(msg_area.getText().trim()+"Welcome to " + name);
    }
}

abstract class user
{
    String name;
    String email;
    String phone;
    
    user()
    {
       msg_area.setText(msg_area.getText().trim()+"New User:");
    }
    
    public void basic_details(String name, String email,String phone)
    {
        this.name = name;
        this.email = email;
        this.phone = phone;
    }   
    
    public void get_details()
    {
        msg_area.setText(msg_area.getText().trim()+ "email : " + email);
        msg_area.setText(msg_area.getText().trim()+"phone : " + phone);
    }
}

class admin extends user
{
    admin(String name, String email,String phone)
    {
        super.basic_details(name,email,phone);
        msg_area.setText(msg_area.getText().trim()+"Admin is added");
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
        msg_area.setText(msg_area.getText().trim()+"Details of Admin - " + name);
        super.get_details();
    }
    
    public void modify_student_details(student s,String student_name, String email, String phone, String ID,int attendance)
    {
        msg_area.setText(msg_area.getText().trim()+"Details of Student - " + s.name + " are modified by admin");
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
         msg_area.setText(msg_area.getText().trim()+"Details of Teacher - " + t.name + " are modified by admin");
        t.name = teacher_name;
        t.email = email;
        t.phone = phone;
    }
    
    public void modify_course_details(course c,String name, String course_id, String teacher)
    {
        msg_area.setText(msg_area.getText().trim()+ "Details of Course - " + c.name + " are modified by admin");
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
        msg_area.setText(msg_area.getText().trim()+"Course - " + c.name + " is deleted");
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
        msg_area.setText(msg_area.getText().trim()+"Student - " + s.name + " is deleted");
        s = null;
    }
    
    public void delete_student_records(course c, student s)
    {
        c.delete_student_records(s);
    }
    
    public void delete_teacher(teacher t)
    {   
        msg_area.setText(msg_area.getText().trim()+ "Teacher - " + t.name + " is deleted");
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
        msg_area.setText(msg_area.getText().trim()+"Teacher is added : " + name);
    }
    
    public void get_details()
    {
        msg_area.setText(msg_area.getText().trim()+"Details of Teacher - " + name );
        msg_area.setText(msg_area.getText().trim()+ "email : " + email);
        msg_area.setText(msg_area.getText().trim()+ "phone : " + phone);
        msg_area.setText(msg_area.getText().trim()+ "Courses : ");
        
        if(courses.size() == 0)
          {
              msg_area.setText(msg_area.getText().trim()+ "None");
          }
        else
          {
              msg_area.setText(msg_area.getText().trim()+courses.size());
          }
          
        for(int i = 0; i < courses.size(); i++ )
        {
            course curr = courses.get(i);
            msg_area.setText(msg_area.getText().trim()+ (i+1 + ". " + curr.name);
        }
        
    }
    
    public void add_courses(course c)
    {
        courses.add(c);
        c.teacher = name;
        c.t = this;
        msg_area.setText(msg_area.getText().trim()+ "Course " + c.name + " is now under Professor " + c.teacher);
        
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
        msg_area.setText(msg_area.getText().trim()+ "Student is added : " + name);
    }
    
    public void get_details()
    {
       
        msg_area.setText(msg_area.getText().trim()+ "Details of Student - " + name);
        msg_area.setText(msg_area.getText().trim()+ "ID : " + ID);
        msg_area.setText(msg_area.getText().trim()+ "Attendance : " + attendance);
        msg_area.setText(msg_area.getText().trim()+"email : " + email);
        msg_area.setText(msg_area.getText().trim()+ "phone : " + phone);
        msg_area.setText(msg_area.getText().trim()+"Courses enrolled : ");
        
        if(courses.size() == 0)
           {
               msg_area.setText(msg_area.getText().trim()+"None");
           }
        else
           {
               msg_area.setText(msg_area.getText().trim()+courses.size());
           } 
          
        for(int i = 0; i < courses.size(); i++ )
        {   
           course curr = courses.get(i);
           msg_area.setText(msg_area.getText().trim()+i+1 + ". " + curr.name);
           msg_area.setText(msg_area.getText().trim()+ "Grade : " + curr.grades_of_students.get(this));
           msg_area.setText(msg_area.getText().trim()+ "Marks : " + curr.marks_of_students.get(this));
           
        }
        
    }
    
    public void add_courses(course c)
    {
        courses.add(c);
        c.students.add(this);
        msg_area.setText(msg_area.getText().trim()+"Student " + name + " is now enrolled in course " + c.name);
        
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
        msg_area.setText(msg_area.getText().trim()+ ("New course " + name + " is added under Professor " + teacher);
        
    }
    
    public void get_details()
    {
        msg_area.setText(msg_area.getText().trim()+"Course : " + name);
        msg_area.setText(msg_area.getText().trim()+ "Course_id : " + course_id);
        msg_area.setText(msg_area.getText().trim()+"Professor : " + teacher);
        msg_area.setText(msg_area.getText().trim()+ "Students enrolled : ");
        
        if(students.size() == 0)
          {
              msg_area.setText(msg_area.getText().trim()+ "None");
          }  
        else    
         {
             msg_area.setText(msg_area.getText().trim()+students.size());
         }   
            
        int count = 0;
        for( student s : students)
        {   
            
            count = count + 1;
            msg_area.setText(msg_area.getText().trim()+count + ") " + s.name);
            msg_area.setText(msg_area.getText().trim()+"Attendace " + attendance_of_students.get(s));
            msg_area.setText(msg_area.getText().trim()+"Marks " + marks_of_students.get(s));
            msg_area.setText(msg_area.getText().trim()+ "Grade " + grades_of_students.get(s) );
            
        }
    }
    
    public void add_student_marks(student s, int attendance, int marks, String grade)
    {
        students.add(s);
        attendance_of_students.put(s , attendance);
        marks_of_students.put(s , marks);
        grades_of_students.put(s , grade);
        msg_area.setText(msg_area.getText().trim()+"Student " + s.name + " marks are added in course " + name);
    }
    
    public void modify_student_marks(student s, int attendance, int marks, String grade)
    {
        attendance_of_students.put(s , attendance);
        marks_of_students.put(s , marks);
        grades_of_students.put(s, grade);
        msg_area.setText(msg_area.getText().trim()+"Student " + s.name + " marks are modified in course " + name);
    }
    
    public void delete_student_records(student s)
    {
        students.remove(s);
        attendance_of_students.remove(s);
        marks_of_students.remove(s);
        grades_of_students.remove(s);
        msg_area.setText(msg_area.getText().trim()+"Student " + s.name + " is removed from course " + name);
    } 
}
    public school() {
        initComponents();
    }

    /**
     * This method is called from within the constructor to initialize the form.
     * WARNING: Do NOT modify this code. The content of this method is always
     * regenerated by the Form Editor.
     */
    @SuppressWarnings("unchecked")
    // <editor-fold defaultstate="collapsed" desc="Generated Code">                          
    private void initComponents() {

        jScrollPane1 = new javax.swing.JScrollPane();
        msg_text = new javax.swing.JTextArea();
        send = new javax.swing.JButton();
        msg_area = new javax.swing.JTextField();

        setDefaultCloseOperation(javax.swing.WindowConstants.EXIT_ON_CLOSE);

        msg_text.setColumns(20);
        msg_text.setRows(5);
        jScrollPane1.setViewportView(msg_text);

        send.setText("jButton1");
        send.addActionListener(new java.awt.event.ActionListener() {
            public void actionPerformed(java.awt.event.ActionEvent evt) {
                sendActionPerformed(evt);
            }
        });

        msg_area.setText("jTextField1");

        javax.swing.GroupLayout layout = new javax.swing.GroupLayout(getContentPane());
        getContentPane().setLayout(layout);
        layout.setHorizontalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(layout.createSequentialGroup()
                .addContainerGap()
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.TRAILING, false)
                    .addComponent(msg_area)
                    .addGroup(layout.createSequentialGroup()
                        .addComponent(jScrollPane1, javax.swing.GroupLayout.PREFERRED_SIZE, 258, javax.swing.GroupLayout.PREFERRED_SIZE)
                        .addPreferredGap(javax.swing.LayoutStyle.ComponentPlacement.RELATED)
                        .addComponent(send, javax.swing.GroupLayout.PREFERRED_SIZE, 104, javax.swing.GroupLayout.PREFERRED_SIZE)))
                .addContainerGap(22, Short.MAX_VALUE))
        );
        layout.setVerticalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addGroup(javax.swing.GroupLayout.Alignment.TRAILING, layout.createSequentialGroup()
                .addContainerGap()
                .addComponent(msg_area, javax.swing.GroupLayout.DEFAULT_SIZE, 200, Short.MAX_VALUE)
                .addGap(18, 18, 18)
                .addGroup(layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING, false)
                    .addComponent(jScrollPane1, javax.swing.GroupLayout.PREFERRED_SIZE, 0, Short.MAX_VALUE)
                    .addComponent(send, javax.swing.GroupLayout.DEFAULT_SIZE, 48, Short.MAX_VALUE))
                .addGap(23, 23, 23))
        );

        pack();
    }// </editor-fold>                        

    private void sendActionPerformed(java.awt.event.ActionEvent evt) {                                    
        // TODO add your handling code here:
        String s1 = "";
        s1 = msg_text.getText().trim();
       
        while(true)
{      
   msg_area.setText(msg_area.getText().trim()+"Choose either - New, Add, Delete, View, Modify details/marks, Delete marks END ");
                    if(s1.equals("END"))
   {
       break;
   }
   else if(s1.equals("New"))
                    {
                        msg_area.setText(msg_area.getText().trim()+"\"Please enter: new_type - student teacher course\"");
                   
     
       s1 = msg_text.getText().trim();
       if(s1.equals("student"))
       {  
           msg_area.setText(msg_area.getText().trim()+"Please enter: name email phone year_of_admission class_enrolled");
           String name = msg_text.getText().trim();
           String email = msg_text.getText().trim();
           String phone = msg_text.getText().trim();
           int year_of_admission =  Integer.parseInt(msg_text.getText().trim());
           int class_enrolled = Integer.parseInt(msg_text.getText().trim());
           
           student new_student = Admin.add_new_student(name, email, phone, year_of_admission, class_enrolled );
           s.students.add(new_student);
       }
       
       else if(s1.equals("teacher"))
       {  
         
             msg_area.setText(msg_area.getText().trim()+"Please enter: name email phone ");
           String name = msg_text.getText().trim();
           String email = msg_text.getText().trim();
           String phone = msg_text.getText().trim();
           
           teacher new_teacher = Admin.add_new_teacher(name, email, phone);
           s.teachers.add(new_teacher);
         
       }
       
       else if(s1.equals("course"))
       {  
           
            msg_area.setText(msg_area.getText().trim()+"Please enter: name course_id teacher ");
           String name = msg_text.getText().trim();
           String course_id = msg_text.getText().trim();
           String t = msg_text.getText().trim();
           
           course new_course = Admin.add_new_course(name, course_id, t);
           s.courses.add(new_course);
           
       }
       
       else
       {  

           msg_area.setText(msg_area.getText().trim()+"Invalid new_type");
       }
       
   }
   
   else if(s1.equals("Add"))
   {
       
      msg_area.setText(msg_area.getText().trim()+"Please enter: add_type - marks course");
       String add_type = msg_text.getText().trim();
       
       if(add_type.equals("marks"))
       {  
                            msg_area.setText(msg_area.getText().trim()+"Please enter: course_name student_id attendance marks grade changed_by name");
           String course_name = msg_text.getText().trim();
           String student_id = msg_text.getText().trim();
           int attendance = Integer.parseInt(msg_text.getText().trim());
           int marks =  Integer.parseInt(msg_text.getText().trim());
           String grade = msg_text.getText().trim();
           String changed_by = msg_text.getText().trim();
           String by = msg_text.getText().trim();
           
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
               msg_area.setText(msg_area.getText().trim()+ "Wrong query!");
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
                {
                    msg_area.setText(msg_area.getText().trim()+"Wrong query!");
                }
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
               msg_area.setText(msg_area.getText().trim()+"Wrong query!");
           }  
           sc.nextLine();
           
       }
       
       else if(add_type.equals("course"))
       {  
           msg_area.setText(msg_area.getText().trim()+"Wrong query!");
           System.out.println("Please enter: course_name student/teacher id/name ");
           String course_name = msg_text.getText().trim();
           String user_tp = msg_text.getText().trim();
           String user_id = msg_text.getText().trim();
           
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
                       {
                           msg_area.setText(msg_area.getText().trim()+"Wrong query!");
                       }
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
                       msg_area.setText(msg_area.getText().trim()+"Wrong query!");
                     else  
                     {
                       std.add_courses(cur);
                     }
               }  
          else
               {
                   msg_area.setText(msg_area.getText().trim()+"Wrong query!");
               }
       }
       
       
       else
       {  
           msg_area.setText(msg_area.getText().trim()+"Invalid add_type");
       }
       
   }
   
   else if(s1.equals("Delete"))
   {
        msg_area.setText(msg_area.getText().trim()+"Please enter: course/teacher/student name/name/id ");
        String user_tp = msg_text.getText().trim();
        String user_id = msg_text.getText().trim();
       
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
                     {
                        msg_area.setText(msg_area.getText().trim()+"This teacher does not exist");
                     }
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
                       {
                           msg_area.setText(msg_area.getText().trim()+"This student does not exist");
                       }
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
                     {
                         msg_area.setText(msg_area.getText().trim()+"This course does not exist");
                     } 
                     else
                     {
                       s.courses.remove(cur);
                       Admin.delete_course(cur);
                     }
               }
           else
               {
                   msg_area.setText(msg_area.getText().trim()+"Wrong query!");
               }
               sc.nextLine();
   }
   
   else if(s1.equals("View"))
   {
        msg_area.setText(msg_area.getText().trim()+"Please enter: course/teacher/student name/name/id ");
        String user_tp = msg_text.getText().trim();
        String user_id = msg_text.getText().trim();
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
                          {
                              msg_area.setText(msg_area.getText().trim()+"This teacher does not exist");
                          }    
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
                      {
                          msg_area.setText(msg_area.getText().trim()+"This student does not exist");
                      }
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
                      {
                          msg_area.setText(msg_area.getText().trim()+"This course does not exist");
                      } 
                     else
                     {
                     cur.get_details();
                     }
               }
           else
               {
                   msg_area.setText(msg_area.getText().trim()+"Wrong query!");
               }
               
   }
   
   else if(s1.equals("Modify details"))
   {
       msg_area.setText(msg_area.getText().trim()+"Please enter: course/teacher/student modified_by  ");
       String user_tp = msg_text.getText().trim();
       String modified_by = msg_text.getText().trim();
       
       
        if(user_tp.equals("teacher"))
               {
                     msg_area.setText(msg_area.getText().trim()+"name email phone ");
                     String name = msg_text.getText().trim();
                     String email = msg_text.getText().trim();
                     String phone = msg_text.getText().trim();

                     teacher tch = null ;
                     for(int j = 0; j < s.teachers.size(); j++)
                     {
                       teacher chk = s.teachers.get(j);
                       if(chk.name.equals(phone))
                           tch = chk ;
                     }
                     
                     if(tch == null || (!modified_by.equals("admin")))
                        {
                            msg_area.setText(msg_area.getText().trim()+"Invalid Modification!");
                        }   
                         else
                         {
                       Admin.modify_teacher_details(tch, name, email, phone);
                         }  
                         sc.nextLine();
               }
           else if(user_tp.equals("student"))
               {
            
                     msg_area.setText(msg_area.getText().trim()+"name email phone ID attendance ");
                     String name = msg_text.getText().trim();
                     String email = msg_text.getText().trim();
                     String phone = msg_text.getText().trim();
                     String ID = msg_text.getText().trim();
                     int attendance = Integer.parseInt(msg_text.getText().trim());
                     
                     
                     student std = null ;
                     for(int j = 0; j < s.students.size(); j++)
                     {
                       student chk = s.students.get(j);
                       if(chk.ID.equals(ID))
                           std = chk ;
                      }
                     
                     if(std == null || (!modified_by.equals("admin")))
                      {
                          msg_area.setText(msg_area.getText().trim()+"Invalid Modification!");
                      }
                     else
                     {
                       Admin.modify_student_details(std, name, email, phone, ID, attendance);
                     }
                
               }  
           else if(user_tp.equals("course"))
               {    
                     msg_area.setText(msg_area.getText().trim()+"name course_id teacher ");
                     String name = msg_text.getText().trim();
                     String course_id = msg_text.getText().trim();
                     String teacher = msg_text.getText().trim();
                     
                     course cur = null;
                 for(int j = 0; j < s.courses.size(); j++)
                 {
                  course chk = s.courses.get(j);
                  if(chk.name.equals(name))
                      cur = chk ;
                 }
                     if(cur == null || (!modified_by.equals("admin")))
                      {
                          msg_area.setText(msg_area.getText().trim()+"Invalid Modification!");
                      }
                     else
                     {
                       Admin.modify_course_details(cur, name, course_id, teacher);
                     }
                     sc.nextLine();
               }
           else
               {
                  msg_area.setText(msg_area.getText().trim()+"Invalid Modification!"); 
               }
             
   }
   
   else if(s1.equals("Modify marks"))
   {
         msg_area.setText(msg_area.getText().trim()+"Please enter: course_name student_id attendance marks grade modified_by ");
         String course_name = msg_text.getText().trim();
                  String student_id = msg_text.getText().trim();
                  int attendance = Integer.parseInt(msg_text.getText().trim());
                  int marks = Integer.parseInt(msg_text.getText().trim());
                  String grade = msg_text.getText().trim();
                  String modified_by = msg_text.getText().trim();
       
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
                   {
                       msg_area.setText(msg_area.getText().trim()+"Marks cannot be modified");
                   } 
                   
            
       
   }
   
   else if(s1.equals("Delete marks"))
   {
         msg_area.setText(msg_area.getText().trim()+"Please enter: course_name student_id  modified_by");
         String course_name = msg_text.getText().trim();
                  String student_id = msg_text.getText().trim();
                  String modified_by = msg_text.getText().trim();
       
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
                   {
                       msg_area.setText(msg_area.getText().trim()+"Marks cannot be modified");
                   } 
            
       
   }
}
           
    }                                    

    /**
     * @param args the command line arguments
     */
    public static void main(String args[]) {
        /* Set the Nimbus look and feel */
        //<editor-fold defaultstate="collapsed" desc=" Look and feel setting code (optional) ">
        /* If Nimbus (introduced in Java SE 6) is not available, stay with the default look and feel.
         * For details see http://download.oracle.com/javase/tutorial/uiswing/lookandfeel/plaf.html
         */
        try {
            for (javax.swing.UIManager.LookAndFeelInfo info : javax.swing.UIManager.getInstalledLookAndFeels()) {
                if ("Nimbus".equals(info.getName())) {
                    javax.swing.UIManager.setLookAndFeel(info.getClassName());
                    break;
                }
            }
        } catch (ClassNotFoundException ex) {
            java.util.logging.Logger.getLogger(school.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (InstantiationException ex) {
            java.util.logging.Logger.getLogger(school.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (IllegalAccessException ex) {
            java.util.logging.Logger.getLogger(school.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        } catch (javax.swing.UnsupportedLookAndFeelException ex) {
            java.util.logging.Logger.getLogger(school.class.getName()).log(java.util.logging.Level.SEVERE, null, ex);
        }
        //</editor-fold>

        /* Create and display the form */
        java.awt.EventQueue.invokeLater(new Runnable() {
            public void run() {
                new school().setVisible(true);
            }
        });
    }

    // Variables declaration - do not modify                    
    private javax.swing.JScrollPane jScrollPane1;
    private static javax.swing.JTextField msg_area;
    private static javax.swing.JTextArea msg_text;
    private static javax.swing.JButton send;
    // End of variables declaration                  
}
