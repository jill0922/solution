```
drop database if exists  airplane;
create database airplane;
use airplane;

drop table if exists passenger;
create table passenger (
    Name          varchar(100),
    Gender        char(2),
    Passport      varchar(20),
    Birthday      date,
    Flight_No     varchar(20),
    Flight_date   date,
    Luggage       double
);

truncate passenger;
insert into passenger (Name, Gender, Passport, Birthday, Flight_No, Flight_date, Luggage)
values  ('Hope Smith', 'M', 'PA094054', '1984-06-26', 'AC127', '2019-04-08', 23.20),
        ('Emily Brown', 'F', 'D88800548', '1999-02-23', 'AC127', '2019-04-08', 45.80),
        ('Emma Sheffield', 'F', 'A11163131', '1989-01-03', 'AC127', '2019-04-08',21.78),
        ('Chalie Chow', 'M', 'EA39054', '1992-09-29', 'UA4570', '2019-04-09', 21.80),
        ('Jason Roberts', 'M', 'G095486', '1962-11-18', 'OS8221', '2019-04-09', 26.36);


drop table if exists airplane;
create table airplane (
    Company                varchar(50),
    Flight_No              varchar(20),
    Model                  varchar(20),
    Weight_threshold       double
);

truncate airplane;
insert into airplane (Company, Flight_No, Model, Weight_threshold)
values ('Air Canada', 'AC127','Boeing B787-800', 18534),
       ('American Airline', 'AA2262', 'B737-800', 27937),
       ('United Airline', 'UA4570', 'CRJ700', 2438 ),
       ('Australia Airlines','OS8221','A220-100',3629),
       ('Australia Airlines','OS8342','B787-800', 18534),
       ('Air Canada', 'AC7673', 'Embraer RJ-175', 10110);

drop table if exists flight;
create table flight (
Flight_No              varchar(20),
Destination            varchar(50),
Distance               INT,
Departure              time,
Arrive                 time
);

truncate flight;
insert into flight (Flight_No, Destination, Distance, Departure, Arrive)
values ('AC127', 'Vancouver(BC), CA', 3356, '20:15', '22:02'),
       ('UA8064', 'New Delhi,IND', 11662,'20:55', '20:20'),
       ('UA4570', 'Chicago, US', 701, '7:22', '8:10'),
       ('OS8221', 'Edmonton(AB), CA', 2697, '20:15', '22:30'),
       ('OS8342', 'Vancouver(BC), CA', 3349, '20:15', '22:02'),
       ('AC7673', 'Chicago, US', 701, '15:00', '15:41');

Select Flight_No, Flight_Date, count(*) from passenger
group by Flight_No, Flight_Date
having count(*)>1;

# got result: AC127 and flight_Date is 2019-04-08

Select Name from passenger
where Flight_No = 'AC127' and flight_Date = '2019-04-08';

# return: Hope Smith, Emily Brown, Emma Sheffield

Select Destination from flight
group by Destination
having count(*) > 1;

# got the destination 'Vancouver(BC), CA' and 'Chicago, US'

SELECT Flight_No, destination from flight
where destination = 'Vancouver(BC), CA';

# result: 'AC127', 'OS8342';

SELECT Flight_No, destination from flight
where destination = 'Chicago, US';

# result: 'UA4570', 'AC7673';

drop table if exists flight_temp;
create table flight_temp as
   Select airplane.Flight_No, Flight_Date, Luggage, Weight_threshold
   From passenger
   right join airplane
   on passenger.Flight_No = airplane.Flight_No;

select * from
(select Flight_No, Flight_Date, sum(luggage) as lug_sum, weight_threshold
from flight_temp
group by Flight_No, Flight_Date, weight_threshold) as a
where  a.lug_sum > a.Weight_threshold;

# return no result
```
