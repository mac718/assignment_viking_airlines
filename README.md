Mike Coon

# assignmnent_viking_analytics

## Queries 1: Warmups

1. Get a list of all users in California

```
SELECT * 
  FROM users 
  JOIN states ON state_id=states.id 
  WHERE states.name='California'
;

```

2. Get a list of all airports in Minnesota

```
SELECT * 
  FROM airports 
  JOIN states ON state_id=states.id 
  WHERE states.name='Minnesota'
;

```

3. Get a list of all payment methods used on itineraries by the user with email address "heidenreich_kara@kunde.net"

```
SELECT payment_method 
  FROM itineraries 
  JOIN users ON user_id=users.id 
  WHERE email='heidenreich_kara@kunde.net'"
;

```

4. Get a list of prices of all flights whose origins are in Kochfurt Probably International Airport.

```
SELECT price 
  FROM flights 
  JOIN airports ON origin_id=airports.id 
  WHERE long_name LIKE 'Kochfurt%'
;

```

5. Find a list of all Airport names and codes which connect to the airport coded XQT.

```
SELECT long_name, code 
  FROM airports 
  JOIN flights fo ON airports.id=fo.origin_id 
  JOIN flights fd ON airports.id=fd.destination_id 
  WHERE fo.destination_id = (SELECT id FROM airports WHERE code = 'XQT') 
    OR fd.origin_id = (SELECT id FROM airports WHERE code = 'XQT')
;

```

6. Get a list of all airports visited by user Dannie D'Amore after January 1, 2012.

Getting 'missing FROM-clause' error

```
SELECT long_name 
  FROM airports 
  JOIN flights ON flights.destination_id=airports.id 
  JOIN tickets ON flights.id=tickets.flight_id 
  JOIN itineraries ON tickets.itinerary_id=intineraries.id 
  JOIN users ON itineraries.user_id=users.id 
  WHERE user_id = (SELECT id FROM users WHERE last_name = 'Hills')
;

```


## Queries 2: Adding in Aggregation

1. Find the top 5 most expensive flights that end in California.

```
SELECT * 
  FROM flights 
  JOIN states ON destination_id=states.id  
  WHERE states.name = 'California' 
  ORDER BY price DESC 
  LIMIT 5
;

```

2. Find the shortest flight that username "ryann_anderson" took.

```
SELECT * 
  FROM flights f 
  JOIN tickets t ON f.id=t.flight_id 
  JOIN itineraries i ON t.itinerary_id=i.id 
  JOIN users u ON i.user_id=u.id 
  WHERE u.username = 'raul' 
  ORDER BY f.distance DESC 
  LIMIT 1
;

```

3. Find the average flight distance for flights entering or leaving each city in Florida

```
SELECT DISTINCT (SUM(fd.distance)/COUNT(fd)) 
  FROM flights fo 
  JOIN states ON fo.origin_id=states.id 
  JOIN flights fd ON fd.destination_id=states.id 
  WHERE states.name = 'Florida'
;

```

4. Find the 3 users who spent the most money on flights in 2013

```
SELECT users.username
  FROM users 
  JOIN itineraries ON users.id=itineraries.user_id 
  JOIN tickets ON itineraries.id=tickets.itinerary_id 
  JOIN flights ON tickets.flight_id=flights.id 
  WHERE departure_time > '2013-01-01'AND departure_time <= '2013-12-31' 
  GROUP BY users.username 
  ORDER BY SUM(flights.price) DESC
  LIMIT 3
;

```

5. Count all flights to or from the city of Port Mac that did not land in Florida

```
SELECT COUNT(*)
  FROM flights
  JOIN airports ao ON ao.city_id = flights.origin_id
  JOIN cities co ON ao.city_id = co.id
  JOIN airports ad ON ad.city_id = flights.destination_id
  JOIN cities cd ON ad.city_id = cd.id
  JOIN states sd ON ad.state_id = sd.id
  WHERE (cd.name = 'Port Mac' OR co.name = 'Port Mac') AND sd.name != 'Florida'
;

```

6. Return the range of lengths of flights in the system(the maximum, and the minimum).

```
SELECT MIN(distance), MAX(distance)
  FROM flights
;

```


## Queries 3: Advanced

1. Find the most popular travel destination for users who live in Kansas.

```
SELECT c.name
  FROM cities c 
  JOIN airports a ON c.id = a.city_id
  JOIN flights f ON a.id = f.destination_id
  JOIN tickets t ON f.id = t.flight_id
  JOIN itineraries i ON t.itinerary_id = i.id
  JOIN users u ON i.user_id = u.id
  WHERE u.state_id = (SELECT id FROM states WHERE name = 'Wyoming')
  GROUP BY c.name
  ORDER BY COUNT(f.destination_id) DESC
  LIMIT 1
;

```

2. How many flights have round trips possible? In other words, we want the count of all airports where there exists a flight FROM that airport and a later flight TO that airport.

```
SELECT COUNT(f.*)
  FROM flights f
  JOIN flights df ON f.origin_id = df.destination_id
  WHERE f.destination_id = df.origin_id AND f.arrival_time < df.departure_time;

```

3. Find the cheapest flight that was taken by a user who only had one itinerary.

```
SELECT f.id, f.price
  FROM flights f 
  JOIN tickets t ON f.id = t.flight_id 
  JOIN itineraries i ON t.itinerary_id = i.id  
  GROUP BY i.user_id 
  HAVING COUNT(i.user_id) = 1 
  ORDER BY f.price
  LIMIT 1
;

```

4. Find the average cost of a flight itinerary for users in each state in 2012.

```
SELECT s.name, AVG(price) 
  FROM flights f 
  JOIN tickets t ON f.id = t.flight_id 
  JOIN itineraries i ON t.itinerary_id = i.id 
  JOIN users u ON i.user_id = u.id 
  JOIN states s ON u.state_id = s.id 
  WHERE f.departure_time BETWEEN '2012-01-01' AND '2012-12-31' 
  GROUP BY s.name

```


