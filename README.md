Mike Coon

# assignmnent_viking_analytics

## Queries 1: Warmups

1.

```
SELECT * 
FROM users 
JOIN states ON state_id=states.id 
WHERE states.name='California'

```

2. 

```
SELECT * 
FROM airports 
JOIN states ON state_id=states.id 
WHERE states.name='Minnesota'

```

3. 

```
SELECT payment_method 
FROM itineraries 
JOIN users ON user_id=users.id 
WHERE email='heidenreich_kara@kunde.net'"

```

4.

```
SELECT price 
FROM flights 
JOIN airports ON origin_id=airports.id 
WHERE long_name LIKE 'Kochfurt%'

```

5.

```
SELECT long_name, code 
FROM airports JOIN flights fo ON airports.id=fo.origin_id 
JOIN flights fd ON airports.id=fd.destination_id 
WHERE fo.destination_id != (SELECT id FROM airports WHERE code = 'PGY') OR fd.origin_id != (SELECT id FROM airports WHERE code = 'PGY')

```

6.

Getting 'missing FROM-clause' error

```
SELECT long_name 
FROM airports 
JOIN flights ON flights.destination_id=airports.id 
JOIN tickets ON flights.id=tickets.flight_id 
JOIN itineraries ON tickets.itinerary_id=intineraries.id 
JOIN users ON itineraries.user_id=users.id 
WHERE user_id = (SELECT id FROM users WHERE last_name = 'Hills')

```



SELECT * FROM flights JOIN states ON destination_id=states.id  WHERE states.name = 'California' ORDER BY price DESC LIMIT 5

