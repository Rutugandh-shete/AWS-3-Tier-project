# Student Registration Form on AWS

## Project Description
This project involves developing a student registration form that captures and stores student information using AWS services. The backend is powered by **AWS RDS** for secure data management, and the frontend is hosted on an **Apache Tomcat** server for smooth accessibility. This project demonstrates my ability to integrate AWS services for database management and web hosting, creating a seamless full-stack solution.

## Architecture
- **Frontend:** Deployed on an **Apache Tomcat** server.
- **Backend:** Uses **AWS RDS (MySQL)** for database management.
- **Security:** Security group with proper inbound rules.

## Steps to Set Up the Project

### 1. Create an AWS RDS Instance
- Create an **RDS instance** with MySQL.
- Attach the **same security group** to both the RDS and EC2 instance.
- Select the **same availability zone** for both.
- Add an inbound rule to allow **port 8080** for Tomcat.

### 2. Create an EC2 Instance
- Select an **Amazon Machine Image (AMI).**
- Choose the **same subnet and security group** as the RDS instance.
- Add the **user data script** during launch.
- Once launched, set up an **EC2 connection to RDS**:
  ```sh
  mysql -h <DB_ENDPOINT> -u admin -p
  ```

### 3. Install Required Files
- Download necessary files:
  ```sh
  curl -O https://s3.amazonaws.com/student.com.in/mysql-connector.jar
  curl -O https://s3.amazonaws.com/student.com.in/student.war
  curl -O https://s3.amazonaws.com/student.com.in/tomcat-rdsdb01+-+Copy.txt
  ```

### 4. Install Apache Tomcat
- Download and extract Tomcat:
  ```sh
  wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.93/bin/apache-tomcat-9.0.93.zip
  unzip apache-tomcat-9.0.93.zip
  ```
- Deploy the **WAR file**:
  ```sh
  mv student.war apache-tomcat-9.0.93/webapps/
  ```

### 5. Configure the Database
- Connect to MySQL and create the database:
  ```sh
  mysql -h <DB_ENDPOINT> -u admin -p
  CREATE DATABASE studentapp;
  USE studentapp;
  CREATE TABLE students(
      student_id INT NOT NULL AUTO_INCREMENT,
      student_name VARCHAR(100) NOT NULL,
      student_addr VARCHAR(100) NOT NULL,
      student_age VARCHAR(3) NOT NULL,
      student_qual VARCHAR(20) NOT NULL,
      student_percent VARCHAR(10) NOT NULL,
      student_year_passed VARCHAR(10) NOT NULL,
      PRIMARY KEY (student_id)
  );
  ```

### 6. Configure Tomcat
- Edit **context.xml** file (`apache-tomcat-9.0.93/conf/context.xml`):
  ```xml
  <Resource name="jdbc/TestDB" auth="Container"
            type="javax.sql.DataSource"
            maxTotal="100" maxIdle="30" maxWaitMillis="10000"
            username="admin" password="password"
            driverClassName="com.mysql.jdbc.Driver"
            url="jdbc:mysql://<DB_ENDPOINT>:3306/studentapp"/>
  ```

### 7. Start Tomcat
- Provide execution permission:
  ```sh
  chmod 777 apache-tomcat-9.0.93/bin/catalina.sh
  ```
- Install Java and start Tomcat:
  ```sh
  sudo apt install default-jdk -y
  ./apache-tomcat-9.0.93/bin/catalina.sh start
  ```

### 8. Access the Application
- Open a browser and visit:
  ```sh
  http://<EC2_PUBLIC_IP>:8080/student/
  ```

## Technologies Used
- **AWS RDS (MySQL)** – Database storage
- **AWS EC2** – Hosting the application
- **Apache Tomcat** – Application server
- **Java & MySQL Connector** – Backend connectivity

## Future Enhancements
- Implement **CI/CD pipeline** for automated deployments.
- Integrate **AWS Lambda** for additional processing.
- Add **user authentication** using AWS Cognito.

## Author
**Rutugandh Shete**  
[GitHub](https://github.com/Rutugandh-shete) | [LinkedIn](https://linkedin.com/in/rutugandh-shete)

---
This project showcases my expertise in AWS, Linux, and full-stack development. 🚀
