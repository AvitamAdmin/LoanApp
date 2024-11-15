# Bank Loan Aplication

The purpose of this project is to build a potential backend system for bank loan application system. Registered users can create a customer account and apply to loan. The bank loan management system would evaluate their application according to custom bank loan criteria and respond customers if they are available to receive loan, and their loan limit. 

The app is **live** and accessible on: https://bank-loan-application-demo.herokuapp.com

You can play around directly using the endpoints. 

For documentation click [here](#api-documentation)

## Technologies Used


This project implementats Spring Boot framework with given dependencies:

- Lombok --> Java annotation library which helps to reduce boilerplate code.
- Spring Web --> Builds web, including RESTful, applications using Spring MVC
- Spring Security --> Highly customizable authentication and access-control framework for Spring applications.
- Spring Data JPA --> Persists data in SQL stores with Java Persistence API using Spring Data and Hibernate
- PostgreSQL Driver --> JDBC & R2DBC driver allowing Java programs to connect to PostgreSQL database using standart Java code
- Swagger --> OAS Rest Documentation
- JUnit, Mockito -> (Unit Testing)
- SLF4J --> Logging



## API Documentation

To have it your way in postman, fork it to your workspace and play around: 

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/5231798-289c2878-e5a2-41ac-9cbb-e95f614d998d?action=collection%2Ffork&collection-url=entityId%3D5231798-289c2878-e5a2-41ac-9cbb-e95f614d998d%26entityType%3Dcollection%26workspaceId%3D0c774f82-43e3-4f89-961b-2b4f9bfabddc)

To access Swagger doc, click live link below: 

https://bank-loan-application-demo.herokuapp.com/swagger-ui/index.html

You can also asccess it locally by updating baseUrl with your local server port.

> {{baseUrl}}/swagger-ui/index.html .

![image](https://user-images.githubusercontent.com/19313466/184607079-d706432f-460f-4323-873d-a95ebef97453.png)
 

## API Demonstration



**Step 1. Register Securely**

| Type | Method |
| ------ | ------ |
| POST | https://bank-loan-application-demo.herokuapp.com/api/v1/users/signup |

```json
{
    "username":"John Cloud",
    "email":"john@gmail.com",
    "password":"12345"
}
```

Response Token:

>eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJKb2huIENsb3VkIiwiYXV0aCI6W3siYXV0aG9yaXR5IjoiUk9MRV9DTElFTlQifV0sImlhdCI6MTY2MDU2MTM1NywiZXhwIjoxNjYwNjQ3NzU3fQ.bkxHd1i1jttct0HVnN8pdCICp38wEKPcKGEoVVrfwso



**Step 2. Add New Customer**


| Type | Method |
| ------ | ------ |
| POST | https://bank-loan-application-demo.herokuapp.com/api/v1/customer/add |

Following request will create a bank customer record in the database. 

```json
{
    "nationalIdentityNumber": "93111111111",
    "firstName": "{{$randomFullName}}",
    "lastName": "{{$randomLastName}}",
    "phone": "{{$randomPhoneNumber}}",
    "email": "{{$randomEmail}}",
    "monthlyIncome": "10000",
    "gender": "male",
    "age": "{{$randomInt}}",
    "loanScore":"1000"
}
```

**Step 3. Check New Customer Information**

| Type | Method |
| ------ | ------ |
| GET | https://bank-loan-application-demo.herokuapp.com/api/v1/customer/get/{nationalIdentityNumber} |


Following request will return the bank customer with specified national identity number. 


```json
{
    "nationalIdentityNumber": "93111111111",
    "firstName": "Isaac Terry",
    "lastName": "Baumbach",
    "phone": "947-893-1929",
    "email": "Leon76@hotmail.com",
    "monthlyIncome": 10000.0,
    "gender": "male",
    "age": 661,
    "loanScore": 1000,
    "loanApplications": []
}
```

**Step 4. Create New Loan Application**


| Type | Method |
| ------ | ------ |
| POST | https://bank-loan-application-demo.herokuapp.com/api/v1/loanapplication/create/{nationalIdentityNumber} |


**Step 5. Learn Loan Application Result**


| Type | Method |
| ------ | ------ |
| GET | https://bank-loan-application-demo.herokuapp.com/api/v1/loanapplication/result/{nationalIdentityNumber} |


Following request will return an active and approved loan application with loan limit. 

```json

{
    "id": 21,
    "loanType": "PERSONAL",
    "loanLimit": 10000.0,
    "loanScoreResult": "APPROVED",
    "loanStatus": "ACTIVE",
    "loanDate": "15-08-2022",
    "creditMultiplier": 4,
    "loanApplication": {
        "id": 5
    }
}

```


#### Case1: 
New customer with loan score 1000 and more, and monthly income above 5000. The application is approved and loan limit specually calculated.
 
```json
{
    "id": 17,
    "loanType": "PERSONAL",
    "loanLimit": 40000.0,
    "loanScoreResult": "APPROVED",
    "loanStatus": "ACTIVE",
    "loanDate": "15-08-2022",
    "creditMultiplier": 4,
    "loanApplication": {
        "id": 1
    }
}
```

#### Case2: 

New customer with loan score between 500 - 1000 and monthly income above 5000. The application is approved and loan limit 20.000 

```json

{
    "id": 22,
    "loanType": "PERSONAL",
    "loanLimit": 20000.0,
    "loanScoreResult": "APPROVED",
    "loanStatus": "ACTIVE",
    "loanDate": "15-08-2022",
    "creditMultiplier": 4,
    "loanApplication": {
        "id": 6
    }
}

```


#### Case3: 

New customer with loan score between 500 - 1000 and monthly income below 5000. The application is approved and loan limit 10.000 set.

```json

{
    "id": 20,
    "loanType": "PERSONAL",
    "loanLimit": 10000.0,
    "loanScoreResult": "APPROVED",
    "loanStatus": "ACTIVE",
    "loanDate": "15-08-2022",
    "creditMultiplier": 4,
    "loanApplication": {
        "id": 4
    }
}

```

#### Case4: 

New customer with loan score below 500. The application is rejected.

```json

{
    "id": 20,
    "loanType": "PERSONAL",
    "loanLimit": 0.0,
    "loanScoreResult": "REJECTED",
    "loanStatus": "INACTIVE",
    "loanDate": "15-08-2022",
    "creditMultiplier": 4,
    "loanApplication": {
        "id": 12
    }
}

```




## Functional Requirements & Analysis



| **USER STORY ID** | **AS A** | **I WANT TO**                                | **SO THAT**                                                          |
|-------------------|----------|----------------------------------------------|----------------------------------------------------------------------|
| 1                 | customer | register to bank loan system                 | I can create a customer account                                      |
| 2                 | customer | add/update/delete loan application           | I can manage my loan application                                     | 
| 3                 | customer | browse my loan application(s)                | I can see my list of confirmed and rejected loan application         | 
| 4                 | customer | login / logout                               | I can securely enter and leave the system                            | 
| 5                 | admin    | register to bank loan system                 | I can perform as a root user such as employee or bank manager        |  
| 6                 | admin    | view a loan application request              | I can review the application information                             |
| 7                 | admin    | confirm / reject                             | I can respond the application                                        |
| 8                 | admin    | add / update / delete a customer of the bank | I can manage customer records                                        |
| 9                 | admin    | send sms to customer                         | I can notify customer about the outcome of his/her loan application  |  
| 10                | admin    | login & logout                               | I can securely enter and leave the system                            |



### ERD Database Design



![](https://github.com/gulbalasalamov/bank-loan-application/blob/master/doc/loan-application-erd.png)


