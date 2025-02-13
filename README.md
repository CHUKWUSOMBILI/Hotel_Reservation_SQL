# HOTEL RESERVATION SQL
# ![onavs65u](https://github.com/user-attachments/assets/43016134-f462-4a36-83b5-2a6c4b70b3c2)
## Table Of Contents
-Introduction 
-Dataset Overview
-Data Cleaning And Transformation
-[SQL Analysis]
- Conclusion And Recommendation

## Introduction
Introduction: This is a hotel reservation dataset comprising of 11,288 males, 10321 females and 605 non-conforming employees across varying race of United State Of America.

## Dataset Overview: : This dataset is comprised of 12 columns and 22,215 rows in  the following sequence
 • Booking_ID: A unique identifier for each hotel reservation. 
• no_of_adults: The number of adults in the reservation.
 • no_of_children: The number of children in the reservation. 
• no_of_weekend_nights: The number of nights in the reservation that fall on weekends. 
• no_of_week_nights: The number of nights in the reservation that fall on weekdays.
• type_of_meal_plan: The meal plan chosen by the guests.
 • room_type_reserved: The type of room reserved by the guests. 
• lead_time: The number of days between booking and arrival. 
• arrival_date: The date of arrival.
 • market_segment_type: The market segment to which the reservation belongs.
 • avg_price_per_room: The average price per room in the reservation. 
• booking_status: The status of the booking. 

![Screenshot 2025-02-13 105700](https://github.com/user-attachments/assets/10add28e-7015-4f98-824d-de56b787295b)

## Data Cleaning And Transformation:
-	In the course of analyzing this dataset, the column headings with abnormality was altered to the normal data type in SQL. 
-	3 new columns were also added to the table making a sum of 15 columns
-	The data was changed to set sql_safe_updates=0;
-	update hotel_reservation_dataset
-	set arrival_date= str_to_date(arrival_date,"%d-%m-%YYYY")
- Handling missing values
- ```sql
  SELECT*
  FROM HOTEL_RESERVATION_DATASET
  WHERE no_of_adults IS NULL
  OR no_of_chidren IS NULL
  OR no_weekend_nights IS NULL
  OR  type_of_meal_plan IS NULL
  OR type_of_room_reseved IS NULL
  OR lead_time IS NULL
  OR arrival_date ISNULL
  OR market_segment_type IS NULL
  OR avg_price_per_room IS NULL
  OR booking_status IS NULL
  ``` Checking for duplicate values
  ``` SQL
  select booking_id,count(*) from hotel_reservation_dataset
  group by booking_id
  having count(*) >1;
  ```
update hotel_reservation_dataset
set reservation_date = date_sub(arrival_date,interval lead_time day);

## SQL Analysis:
-- 1. What is the total number of reservations in the dataset? 
select * from hotel_reservation_dataset;
select count(*) as total_reservation from hotel_reservation_dataset;

-- 2. Which meal plan is the most popular among guests? 
select distinct type_of_meal_plan from hotel_reservation_dataset;
select type_of_meal_plan,count(type_of_meal_plan)as count_meal_plan from hotel_reservation_dataset
group by type_of_meal_plan
order by count_meal_plan desc limit 1;

-- 3. What is the average price per room for reservations involving children? 
select round(avg(avg_price_per_room),2)  from hotel_reservation_dataset
 where no_of_children >0;


-- 4. How many reservations were made for the year 20XX (replace XX with the desired year)? 
 select count(reservation_date)as 2017_reserved from hotel_reservation_dataset
where left(reservation_date,4) =2017;

-- number of reservation in 2018
select count(reservation_date) as 2018_reserved from hotel_reservation_dataset
where year(reservation_date) = 2018;

-- 5. What is the most commonly booked room type? 
select room_type_reserved,count(room_type_reserved) as count from hotel_reservation_dataset
group by room_type_reserved
order by count desc limit 1;

-- 6. How many reservations fall on a weekend (no_of_weekend_nights > 0)? 
select count(no_of_weekend_nights)  as weekend_reservation from hotel_reservation_dataset
where no_of_weekend_nights >0;


-- 7. What is the highest and lowest lead time for reservations?
 select min(lead_time),max(lead_time) from hotel_reservation_dataset;


 -- 8. What is the most common market segment type for reservations? 
select market_segment_type, count(market_segment_type) as common_segment  from hotel_reservation_dataset
group by market_segment_type
order by common_segment desc limit 1;


-- 9. How many reservations have a booking status of "Confirmed"? 
select count(*)  from hotel_reservation_dataset
where booking_status="not_canceled";


-- 10. What is the total number of adults and children across all reservations? 
select sum(no_of_adults) as total_adults, sum(no_of_children) as total_children,
sum(no_of_adults +no_of_children) from hotel_reservation_dataset;

-- 11. What is the average number of weekend nights for reservations involving children? 
select avg(no_of_week_nights) as avg_weekend_nights from hotel_reservation_dataset 
where no_of_children >0;


-- 12. How many reservations were made in each month of the year?
select monthname(reservation_date) as month_name, month(reservation_date) as mnt_number, count(reservation_date)
from hotel_reservation_dataset
group by monthname(reservation_date) , month(reservation_date)
order by month(reservation_date);


-- 13. What is the average number of nights (both weekend and weekday) spent by guests for each room 
-- type? 
select room_type_reserved,round(avg( no_of_week_nights),2)  as avg_nights,round(avg(no_of_weekend_nights),2) as avg_week from hotel_reservation_dataset
where booking_status ="Not_Canceled"
group by  room_type_reserved
order by  avg_nights desc;


-- 14. For reservations involving children, what is the most common room type, and what is the average 
-- price for that room type? 
select room_type_reserved, count(room_type_reserved) as children_room, round(avg(avg_price_per_room),2)
 as avg_price from  hotel_reservation_dataset
 where no_of_children >0
group by room_type_reserved
order by avg_price desc;


-- 15. Find the market segment type that generates the highest average price per room.
select market_segment_type, round(avg(avg_price_per_room),2) as avg_market_segment from hotel_reservation_dataset
group by  market_segment_type
order by avg_market_segment desc limit 1;

## Conclusion And Recommendation: 
- From the analysis so far, it was observed that room type 1  generated most of the revenue via online segment.
- meal plan 1 was mostly ordered by the customers.
- 2018 made the most reservations.
- The hotel was majorly booked during weekend nights than weekday
- Reservation were done more in January than all other months
- the customers were more of adults without children

  ## Recommendation
  - More advertisement should be done 2nd and 3rd quarter of the year.
  - The prices of other meal plan should be considered so as to improve sales.
  - Weekend nights should be more fun than ever before.
  - children package should be enhanced to attract more parents to the hotel.
  - The prices of other rooms should be considered to facilitate more bookings
  
  

