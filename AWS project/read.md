# AWS Student Registration Form Project

## Project Description
This project involves creating a **Student Registration Form** to capture and store student information. The backend is managed using **AWS RDS** (Relational Database Service) for secure data storage. The website is deployed on an **Apache Tomcat** server, providing a reliable hosting solution. This project highlights the integration of AWS services with database management and web hosting for a full-stack application.

---

## Steps to Implement

### 1. Create RDS Instance
- Refer to the RDS documentation to create an RDS instance.
- Attach the same security group to both RDS and EC2 instance.
- Ensure both are in the same availability zone.
- Add the following inbound rule to the security group:
  - **Port 8080**: Default port for Apache Tomcat to listen for HTTP requests.

### 2. Set Up EC2 Instance
- Create an EC2 instance.
- Select an Amazon Machine Image (AMI).
- Choose the subnet of the same availability zone.
- Attach the same security group used for RDS.
- Add the following script in **User Data**:

### 3. Connect EC2 with RDS
- Launch the EC2 instance.
- Go to the RDS service, click on the database, and set up an EC2 connection.
- Access the EC2 terminal and check the connection using:
  ```bash
  mysql -h database-1.c9qec24e6olk.us-east-1.rds.amazonaws.com -u admin -p
  ```

### 4. Install Required Files
Run the following commands on the EC2 instance:
- Download MySQL Connector:
  ```bash
  curl -O https://s3.amazonaws.com/student.com.in/mysql-connector.jar
  ```
- Download `student.war` file:
  ```bash
  curl -O https://s3.amazonaws.com/student.com.in/student.war
  ```
- Download configuration file:
  ```bash
  curl -O https://s3.amazonaws.com/student.com.in/tomcat-rdsdb01+-+Copy.txt
  ```

### 5. Install Apache Tomcat
- Download the latest version of Apache Tomcat:
  ```bash
  curl -O https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.93/bin/apache-tomcat-9.0.93.zip
  ```
- Unzip the downloaded file.

### 6. Configure Database
- Add a schema to the database:
  ```sql
  CREATE DATABASE studentapp;
  USE studentapp;
  CREATE TABLE IF NOT EXISTS students(
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

### 7. Deploy Web Application
- Copy `student.war` to the `webapps` directory in Apache Tomcat:
  ```bash
  cp student.war /path-to-apache-tomcat/webapps/
  ```
- Update the `context.xml` file in `apache-tomcat-9.0.93/conf/`:
  ```xml
  <Resource name="jdbc/TestDB" auth="Container" type="javax.sql.DataSource"
            maxTotal="100" maxIdle="30" maxWaitMillis="10000"
            username="USERNAME" password="PASSWORD"
            driverClassName="com.mysql.jdbc.Driver"
            url="jdbc:mysql://DB-ENDPOINT:3306/DATABASE" />
  ```

### 8. Configure and Run Tomcat
- Navigate to the `bin` directory of Apache Tomcat.
- Make `catalina.sh` executable:
  ```bash
  chmod 777 catalina.sh
  ```
- Install Java if not already installed.
- Start Tomcat:
  ```bash
  ./catalina.sh start
  ```

### 9. Access Application
- Access the application via the public IP of the instance:
  ```
  http://<PUBLIC-IP>:8080/student/
  ```

---

## Directories in Apache Tomcat

### Key Directories
- **bin**: Scripts to start/stop Tomcat.
- **conf**: Configuration files.
- **lib**: Java libraries required by Tomcat.
- **logs**: Logs for troubleshooting.
- **webapps**: Location to deploy WAR files.

---

## Notes
- Ensure proper permissions are set for all files.
- Test connectivity between EC2 and RDS before deploying the application.
- Monitor logs in the `logs` directory for troubleshooting issues.

---

## Technologies Used
- **AWS RDS** for database management.
- **Apache Tomcat** for web hosting.
- **MySQL** for database.
- **Java** for application backend.
