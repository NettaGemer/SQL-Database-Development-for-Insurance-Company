# SQL-Database-Development-for-Insurance-Company
As part of a database course, we were given a case story on which we had to build an SQL schemas. Later we had tasks on the same schemes.

**Case story:**
The Chinese insurance company lee-Slow is considering entering the field of vehicle insurance in Israel.
Since the company wants to be competitive, it wants to learn about the driving habits of the different vehicles, to know who the people are in the car on each trip,
and who among them is the driver. Create a database for the lee-Slow company, which will make it possible to record the details of the drivers,
passengers and vehicles, and the travel routes. The details of the trip should include, among other things, the details of the starting point,
for example, 2 Rabbino Yeruchem, Tel Aviv-Yafo. The location of the accident must be documented (according to the driver's reports).

**1. Create a database for this section**

People(PersonID, fullName, City, DriverID(FK))

Vehicles (CarNumber, Model, DriverID(FK))

Trips(Hour_ ,Date_, StartLocation, EndLocation, DriverID(FK), CarNumber(FK))

Accidents(Accident_Hour, City, Street, StreetNumber,DriverID(FK), CarNumber(FK),Hour_(FK) ,Date_(FK))

create database slow_lee;

use slow_lee;

CREATE TABLE People (

  PersonID INT,
  
  fullName VARCHAR(50),
  
  City VARCHAR(100),
  
  DriverID INT,
  
  FOREIGN KEY (DriverID) REFERENCES People(PersonID),
  
  PRIMARY KEY (PersonID)
  
);

CREATE TABLE Vehicles (

	CarNumber int,
 
    Model VARCHAR(50),
    
	OwnerID int,
 
	FOREIGN KEY (OwnerID) REFERENCES People(PersonID),
 
    PRIMARY KEY (CarNumber,OwnerID)
    
    );

CREATE TABLE Trips (

	DriverID int,
 
    CarNumber int,
    
	Hour_ VARCHAR(30),
 
	Date_ VARCHAR(30),  
 
    StartLocation VARCHAR(100),
    
    EndLocation VARCHAR(100),
    
	FOREIGN KEY (DriverID) REFERENCES People(DriverID),
 
	FOREIGN KEY (CarNumber) REFERENCES Vehicles(CarNumber),
 
	PRIMARY KEY (Date_, Hour_,DriverID,CarNumber)
 
    );    
  
CREATE TABLE Accidents (

	DriverID int,
 
	City VARCHAR(100),
 
	Street VARCHAR(100),
 
	StreetNumber int,
 
    CarNumber int,
    
    Hour_ VARCHAR(30),
    
    Date_ VARCHAR(30),
    
    Accident_Hour VARCHAR(30),
    
    FOREIGN KEY (DriverID) REFERENCES People(DriverID),
    
    FOREIGN KEY (Date_, Hour_) REFERENCES Trips(Date_, Hour_),
    
	FOREIGN KEY (CarNumber) REFERENCES Vehicles(CarNumber),
 
    PRIMARY KEY (Accident_Hour, DriverID, Date_, Hour_, CarNumber)
    
);

**2. Enter five records for each table in the database you created.**

INSERT INTO People VALUE (123,'Bar Malik', 'Tel-Aviv', NULL);

INSERT INTO People VALUE (124,'Netta Gemer', 'Rosh-Haayin', NULL);

INSERT INTO People VALUE (129,'Noam Gemer', 'Rosh-Haayin', NULL);

INSERT INTO People VALUE (128,'Gil Levi', 'Raanana', NULL);

INSERT INTO People VALUE (127,'Hadar Pinhas', 'Yavne', NULL);

INSERT INTO People VALUE (125,'Tali Mul', 'Beit-Shemesh', 123);

INSERT INTO People VALUE (126,'Yuval Ben-Ami', 'Petah-Tikva', 123);

INSERT INTO People VALUE (120,'Adi Gilboa', 'Raanana', 124);

INSERT INTO People VALUE (121,'Ronit Gemer', 'Tel-Aviv', 124);

INSERT INTO People VALUE (122,'Gal Gemer', 'Raanana', 129);

INSERT INTO People VALUE (111,'Noa Shtarkman', 'Petah-Tikva', 128);

INSERT INTO People VALUE (112,'Yam Shoam', 'Hod-Hasharon', 127);

INSERT INTO Vehicles VALUE (123453, 'SKODA', 123);

INSERT INTO Vehicles VALUE (123454, 'Hyundai', 124);

INSERT INTO Vehicles VALUE (123459, 'Toyota', 129);

INSERT INTO Vehicles VALUE (123458, 'Kia', 128);

INSERT INTO Vehicles VALUE (123457, 'Mazda', 127);

INSERT INTO Trips VALUE (123, 123453,'1/1/23','09:00', 'Rothschild Boulevard', 'Jaffa Street');

INSERT INTO Trips VALUE (124, 123454, '1/1/23','09:00',  'Herzl Street', 'Derech Eilat');

INSERT INTO Trips VALUE (129, 123459, '1/1/23','09:00','David Street', 'Masada National Park Road');

INSERT INTO Trips VALUE (128, 123458, '1/1/23','09:00','Arava Road', 'Dead Sea Main Road');

INSERT INTO Trips VALUE (127, 123457, '2/2/23','09:00', 'Ben Yehuda Street', 'Caesarea National Park Road');

INSERT INTO Accidents VALUE (123, 'Tel-Aviv', 'Dizengoff Street', 1,123453,'1/1/23','09:00','09:30');

INSERT INTO Accidents VALUE (123, 'Petah-Tikva', 'Ben Yehuda Street', 11,123453,'1/1/23','09:00','09:32');

INSERT INTO Accidents VALUE (123, 'Tel-Aviv', 'Rothschild Boulevard', 10,123453,'1/1/23','09:00','10:30');

INSERT INTO Accidents VALUE (124, 'Rosh-Haayin', 'Kings Road', 21,123454,'1/1/23','09:00','09:35');

INSERT INTO Accidents VALUE (124,'Eilat', 'Coral Beach Road', 22, 123454,'1/1/23','09:00','09:30');

INSERT INTO Accidents VALUE (124, 'Rosh-Haayin', 'Kings Road', 24,123454,'1/1/23','09:00','09:38');

INSERT INTO Accidents VALUE (124, 'Eilat', 'Coral Beach Road', 25,123454,'1/1/23','09:00','09:50');

INSERT INTO Accidents VALUE (129, 'Jerusalem', 'Jaffa Street', 31,123459,'1/1/23','09:00','10:30');

INSERT INTO Accidents VALUE (129, 'Haifa', 'Allenby Street', 32,123459,'1/1/23','09:00','09:20');

INSERT INTO Accidents VALUE (128, 'Jerusalem', 'Jaffa Street', 4,123458,'1/1/23','09:00','09:40');

INSERT INTO Accidents VALUE (127,'Petah-Tikva', 'Dizengoff Street', 5,123457,'2/2/23','09:00','09:55');

**3. Apart from the information requirements that appeared above, the database that you are required to build, is required to support the queries that follow. Here are the questions you must produce:**

**3.1 How many vehicles were reported to have been involved in a traffic accident?**

select count(Vehicles.CarNumber) as amount_cars_in_accident

from Vehicles

inner join Accidents on Vehicles.DriverID=Accidents.DriverID;

**3.2 In which city is the percentage of car owners the highest?**

CREATE TABLE people_owner AS

SELECT p.City, COUNT(p.PersonID) AS count_people, COUNT(v.OwnerID) AS count_owner

FROM People p

LEFT JOIN Vehicles v ON v.OwnerID = p.PersonID

GROUP BY p.City;


select City, concat((count_owner/count_people) *100 , '%') as  Max_Precentage_owner_city

from people_owner

where count_owner/count_people = (select max(count_owner/count_people) from  people_owner) ;

**3.3. In which city is the highest percentage of damaged vehicles out of the total number of vehicles belonging to the city's residents?**

CREATE TABLE acc_city_people AS

SELECT cp.City, ca.count_acc, cp.count_people

FROM

  (SELECT City, COUNT(carNumber) AS count_acc
  
   FROM Accidents
   
   GROUP BY City) AS ca
   
JOIN

  (SELECT City, COUNT(City) AS count_people
  
   FROM people
   
   GROUP BY City) AS cp
   
   ON ca.City = cp.City;

select City, concat((count_acc/count_people) *100 , '%') as  max_precent_acc_in_city

from acc_city_people

where count_acc/count_people = (select max(count_acc/count_people) from  acc_city_people );

**3.4 How many accidents occurred on each street, in each city?**

SELECT City, Street, COUNT(*) AS NumAccidents

FROM Accidents

GROUP BY City, Street;

**3.5 Create a sorted list of the vehicles that were involved in accidents, and the names of their owners, at the top of which will appear those that were damaged only once, and at the end the vehicles that were damaged the most times.**

SELECT Vehicles.CarNumber,People.fullName, COUNT(*) AS NumAccidents

FROM Accidents

JOIN Vehicles ON Accidents.CarNumber = Vehicles.CarNumber

JOIN People ON Vehicles.OwnerID = People.PersonID

GROUP BY Vehicles.CarNumber, People.fullName

ORDER BY NumAccidents ASC;

**3.6. Now create a View whose display will display a sorted list of the vehicles that were involved in accidents, the names of their owners and their city of residence,
at the top of which will appear those that were involved in accidents the most times, and at the end, those that were involved in an accident only once.**

create view car_accident as

SELECT Vehicles.CarNumber,People.city, People.fullName, COUNT(Vehicles.CarNumber) AS NumAccidents

FROM Accidents

JOIN Vehicles ON Accidents.CarNumber = Vehicles.CarNumber

JOIN People ON Vehicles.OwnerID = People.PersonID

GROUP BY Vehicles.CarNumber, People.fullName,People.city

ORDER BY NumAccidents DESC;

**3.7 Define a user and give this user viewing privileges in the view you created in section 3.6**

CREATE USER ‘rotem’ IDENTIFIED BY ‘rotempass’;

GRANT SELECT ON car_accident TO rotem;
