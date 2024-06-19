# Quiz Application

This document provides an overview of the deployment and configuration of a Java Spring Boot application for a quiz system on AWS. The focus is on the AWS setup, including EC2 and RDS, to deploy and manage the application.

**Note:** This is not the full project but rather an explanation of the deployment and configuration of the project in AWS.

- The full project can be found [here](https://github.com/Gigi-Pons/QuizApplication)

## Running Application

You can access the running application using the following link: [Quiz Application Running on AWS](http://ec2-44-232-53-0.us-west-2.compute.amazonaws.com:8080/queries/allQueries)

The URL is as follows: http://ec2-44-232-53-0.us-west-2.compute.amazonaws.com:8080/queries/allQueries

## AWS Setup

### EC2 Instance

1. **Creation**:
   - An EC2 instance was created using the AWS Management Console. The instance type used is `t3.micro`.

2. **Security Groups**:
   - Security groups were configured to allow inbound traffic on specific ports:
     - **SSH (Port 22)**: For secure shell access.
     - **HTTP (Port 8080)**: For accessing the application.
     - **PostgreSQL (Port 5432)**: For database connectivity.

### RDS Instance

1. **Creation**:
   - An RDS instance running PostgreSQL was created. The instance identifier is `database-1` and it is accessible at `database-1.clsc0miqknho.us-west-2.rds.amazonaws.com`.

2. **Database Setup**:
   - A database named `quizappdb` was created within the RDS instance.
   - Necessary tables were created and populated with data.

3. **Configuration**:
   - The application properties were configured to connect to the RDS instance:
     ```properties
     spring.datasource.driver-class-name=org.postgresql.Driver
     spring.datasource.url=jdbc:postgresql://database-1.clsc0miqknho.us-west-2.rds.amazonaws.com:5432/quizappdb
     spring.datasource.username=postgres
     spring.datasource.password=your-password
     spring.jpa.hibernate.ddl-auto=update
     spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
     spring.jackson.serialization.indent-output=true

     logging.level.root=DEBUG
     logging.level.org.springframework=DEBUG
     logging.level.org.hibernate=DEBUG
     ```

## Deployment and Running the Application

### Uploading the .jar File

1. **Build the .jar file**:
   - Use Maven to build the application and create a `.jar` file.

2. **Upload the .jar file to EC2**:
   - Use SCP to upload the `.jar` file to the EC2 instance:
     ```sh
     scp -i your-key-pair.pem /path/to/quizApplication-0.0.1-SNAPSHOT.jar ec2-user@your-ec2-ip:/home/ec2-user/
     ```

3. **Run the Application**:
   - SSH into the EC2 instance and run the application:
     ```sh
     ssh -i your-key-pair.pem ec2-user@your-ec2-ip
     cd /home/ec2-user/
     nohup java -jar quizApplication-0.0.1-SNAPSHOT.jar > application.log 2>&1 &
     ```

## Testing with Postman

1. **Set Up Postman**:
   - Open Postman and create a new GET request.
   - Use the following URL to test the application's endpoint:
     ```
     http://your-ec2-public-ip:8080/queries/allQueries
     ```
   - Replace `your-ec2-public-ip` with the public IP address of your EC2 instance.

2. **Send the Request**:
   - Click on "Send" to send the GET request.
   - You should receive a response from the server indicating that the application is running correctly and connected to the RDS database.
