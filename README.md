# Event-Management

Query 1: Join Query with Aggregate Function to Retrieve All Events Along with 
Their Organizers: 
SELECT e.event_name, e.start_date, e.end_date, e.location, o.organizer_name 
FROM Event e 
INNER JOIN Organizer o ON e.organizer_id = o.organizer_id;

Query 2: Join Query to Retrieve Session Performances with Speaker Performer 
and Event Details: 
SELECT s.title, s.start_time, s.end_time, sp.first_name, sp.last_name, e.event_name 
FROM Session_Performance s 
INNER JOIN Speaker_Performer sp ON s.speaker_performer_id = 
sp.speaker_performer_id 
INNER JOIN Event e ON s.event_id = e.event_id;

Query 3: Join Query with Aggregate Function to Calculate Total Cost per 
Event: 
SELECT e.event_name, SUM(s.cost) AS total_cost 
FROM Event e 
LEFT JOIN Vendor v ON e.organizer_id = v.vendor_id 
LEFT JOIN Service s ON v.vendor_id = s.vendor_id 
GROUP BY e.event_name;

Query 4: Join Query to Retrieve Top 5 Expensive Services with Vendor 
Information: 
SELECT v.vendor_name, s.service_type, s.cost 
FROM Service s 
INNER JOIN Vendor v ON s.vendor_id = v.vendor_id 
ORDER BY s.cost DESC 
LIMIT 5;

Query 5: Join Query to Retrieve Events with Expenses Exceeding 50000: 
SELECT e.event_name, ex.description, ex.amount 
FROM Event e 
INNER JOIN Expense ex ON e.event_id = ex.event_id 
WHERE ex.amount > 50000;

Query 6: Join Query to Retrieve Vendors Associated with Active Events: 
SELECT DISTINCT v.vendor_name 
FROM Vendor v 
INNER JOIN Service s ON v.vendor_id = s.vendor_id 
WHERE EXISTS ( 
SELECT 1 
FROM Event e 
WHERE e.status = 'Active' 
);

Query 7: Join Query with Aggregate Function to Calculate Average Cost per 
Vendor: 
SELECT v.vendor_name, AVG(s.cost) AS average_cost FROM Vendor v 
LEFT JOIN Service s ON v.vendor_id = s.vendor_id 
GROUP BY v.vendor_name;

Query 8: Join Query to Retrieve Event Details with Session Performances by 
Speaker Amit Sharma: 
SELECT e.event_name, s.title, sp.first_name, sp.last_name FROM Event e 
INNER JOIN Session_Performance s ON e.event_id = s.event_id 
INNER JOIN Speaker_Performer sp ON s.speaker_performer_id = 
sp.speaker_performer_id 
WHERE sp.first_name = 'Amit' AND sp.last_name = 'Sharma';

Query 9: Join Query to Retrieve Event Details with Venue Information, 
Ordered 
by Venue Price (Descending): 
SELECT e.event_name, v.venue_name, v.venue_address, v.venue_price FROM Event e 
27INNER JOIN Venue v ON e.event_id = v.event_id 
ORDER BY v.venue_price DESC;

Query 10: Join Query with Nested Subquery and Aggregate Function to 
Retrieve 
Events with Expenses Exceeding the Average Expense Amount: 
SELECT e.event_name, ex.description, ex.amount 
FROM Event e INNER JOIN Expense ex ON e.event_id = ex.event_id 
WHERE ex.amount > ( 
SELECT AVG(amount) FROM Expense 
);

Query 11: Join Query with Nested Subquery to Retrieve Events with Start 
Date 
Equal to the Earliest Start Date and End Date Equal to the Latest End Date 
for 
Each Organizer: 
SELECT e.event_name, o.organizer_name, e.start_date, e.end_date 
FROM Event e 
INNER JOIN Organizer o ON e.organizer_id = o.organizer_id 
WHERE e.start_date <= ALL ( 
SELECT start_date 
FROM Event 
WHERE organizer_id = e.organizer_id 
) AND e.end_date >= ALL ( 
SELECT end_date 
FROM Event 
WHERE organizer_id = e.organizer_id 
);

Query 12: Join Query with Nested Subquery and Aggregate Function to 
Retrieve 
Event Details with Organizer Information and Average Service Cost per 
Event: 
SELECT e.event_name, 
o.organizer_name, 
(SELECT AVG(cost) 
FROM Service 
WHERE vendor_id IN ( 
SELECT vendor_id 
FROM Vendor 
WHERE event_id = e.event_id 
) 
) AS average_service_cost 
FROM Event e 
INNER JOIN Organizer o ON e.organizer_id = o.organizer_id; 

Query 13: Join Query with Nested Subquery and Aggregate Function to 
Retrieve 
Event Details with Organizer Information and Count of Sessions with Speaker 
Name Starting with 'A': 
SELECT e.event_name, 
o.organizer_name, 
(SELECT COUNT(*) 
FROM Session_Performance 
30WHERE event_id = e.event_id AND speaker_performer_id IN ( 
SELECT speaker_performer_id 
FROM Speaker_Performer 
WHERE first_name LIKE 'A%' 
) 
) AS sessions_with_a_speaker 
FROM Event e 
INNER JOIN Organizer o ON e.organizer_id = o.organizer_id;

Query 14: Join Query with Aggregate Function to Calculate Total Expenses 
per 
Organizer and Retrieve Top 5 Organizers by Total Expenses: 
SELECT o.organizer_name, SUM(e.amount) AS total_expenses 
FROM Organizer o 
JOIN Event ev ON o.organizer_id = ev.organizer_id 
JOIN Expense e ON ev.event_id = e.event_id 
GROUP BY o.organizer_name 
ORDER BY total_expenses DESC 
LIMIT 5;
