 # Java Application Deployment with Reverse Proxy on AWS
🎯 Objective

* Deploy a Java-based Student Registration Web Application on AWS with:

* Proper MySQL database integration

* Secure reverse proxy access

* Backend isolation using AWS Security Groups

## Architecture
User → Nginx Reverse Proxy (EC2) → Backend EC2 (Apache + PHP) → AWS RDS (MySQL)




#  Technologies Used

* Cloud Provider: AWS

* Compute: EC2 (Linux)

* Web Server / Reverse Proxy: Nginx / Apache HTTPD

* Application Server: Apache Tomcat

* Language: Java (Servlet-based)

* Database: Amazon RDS (MySQL-compatible)

* Database Connector: MySQL Connector/J

* Build Artifact: .war file


#  Requirements Implemented
## 1️. Infrastructure Setup

* Launched two Linux EC2 instances:

* Backend EC2: Java Application Server

* Proxy EC2: Reverse Proxy Server

* Configured Security Groups:

* Proxy EC2: Allow HTTP (80) & SSH (22)

* Backend EC2: Allow port 8080 only from Proxy EC2
* RDS: Allow MySQL (3306) only from    Backend EC2

## 2️. Application Deployment 

* 1.Installed Apache Tomcat on Backend EC2

* Deployed the student registration application:

  student.war

* Application runs internally on:

  http://<Backend-Pub-IP>:8080/student

## 3️. Database Setup (Amazon RDS)

Launched Amazon RDS (MySQL-compatible)

Created a database schema and table

Database Schema
```
CREATE DATABASE studentapp;
USE studentapp;
create table if not exists students(student_id int not null auto_increment,student_name VARCHAR(100) NOT NULL,
    ->  student_addr VARCHAR(100) NOT NULL,
    ->  student_age VARCHAR(3) NOT NULL,
    ->  student_qual VARCHAR(20) NOT NULL,
    ->  student_percent VARCHAR(10) NOT NULL,
    ->  student_year_passed VARCHAR(10) NOT NULL,
    -> PRIMARY KEY (student_id)
    ->  );
```
![alt text](<screenshots\Screenshot 2026-01-05 161548.png>)

##  4️. Database Connector Configuration

* Downloaded MySQL Connector

* mysql-connector.jar

* Placed it inside Tomcat library directory:

/opt/apache/lib/

![alt text](<screenshots\Screenshot 2026-01-04 195033.png>)

Enabled JDBC connectivity between Java application and RDS:
```
cd /opt/conf/context.xml
```
![alt text](<screenshots\Screenshot 2026-01-03 182117.png>)

## 5️. Reverse Proxy Configuration

Installed Nginx on Proxy EC2

Configured reverse proxy to forward requests to backend Tomcat server

Sample Nginx Configuration
```
server {
    listen 80;
    server_name <Proxy-Public-IP>;

    location /{
        proxy_pass http://<Backend-Private-IP>:8080/student;
    }
}    
```   

## 6️. Access Control & Security

* Backend EC2 not publicly accessible

* Port 8080 blocked from public internet

* Only Proxy EC2 can communicate with Backend EC2

* Application accessible only via reverse proxy

* Direct backend access blocked:
```
http://<Backend-Public-IP>:8080  →
 Restricted
  ```

 Final Access URL:

``` 
http:         //<Proxy-Instance-Public-IP>/student
```
![alt text](<screenshots\Screenshot (1).png>)

entered the data it will store the data in database

![alt text](<screenshots\Screenshot 2026-01-08 235830.png>)

tests the data had stored or no not so chek in database

```
select * from students;
```
![alt text](<screenshots\Screenshot 2026-01-09 002329.png>) 