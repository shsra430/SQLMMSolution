### :policewoman: SQL MURDER MYSTERY CHALLENGE :policewoman:

💡 Built using MySQL
- SQL Level: Beginner
- SQL joins, Sub-Queries, Wildcard Operators

![image](https://user-images.githubusercontent.com/54994083/180205133-97c5cee4-fa59-4322-a156-65f083dfbb09.png)

:female_detective: This is a fun challenge hosted [here](https://mystery.knightlab.com/) for SQL enthusiasts. I thoroughly enjoyed solving this mystery. So, Let's solve the case!
***

- We can start by looking at the `crime_scene_report` table  
````sql
SELECT 
    *
FROM
    crime_scene_report
WHERE
    city = 'SQL City' AND date = '20180115'
        AND type = 'murder';
````
- There are two witnesses. One lives on "Northwestern Dr" and the second named Annabel, 
- She lives somewhere on "Franklin Ave". Let's get more information about these witnesses.

````sql
SELECT 
    *
FROM
    person
WHERE
    address_street_name = 'Northwestern Dr'
ORDER BY address_number DESC
LIMIT 1;
````
-  This is Morty Shapiro with id 14887    

````sql
SELECT 
    *
FROM
    person
WHERE
    address_street_name = 'Franklin Ave'
        AND name LIKE '%Annabel%';
````

- This is Annabel Miller with id 16371
- Let us now look at the interviews of these witnesses
- Only an interview of Annabel exists

````sql

SELECT 
    *
FROM
    get_fit_now_member
WHERE
    person_id = '16371';
````
- Here she says that she recognizes the killer from her gym and has seen the killer on 9th January in the gym.
- Let's find out who all checked into the gym on Jan 9th in the duration when Annabel was present.

````sql
SELECT 
    *
FROM
    get_fit_now_check_in
        LEFT JOIN
    get_fit_now_member ON get_fit_now_check_in.membership_id = get_fit_now_member.id
WHERE
    check_in_date = '20180109'
        AND check_in_time <= 1600
        AND check_out_time >= 1700;
````
- We now know that the person id of these two are: 28819 and 67318. Let us look for their interviews
- An interview of **Jeremy Bowers** (person_id 67318)  seems promising

````sql
SELECT 
    *
FROM
    interview
WHERE
    person_id = '67318';
````

-  He says he was hired by a woman with lot of money

- Between 65 and 67 inches , red hair and drives a tesla model S. 
- Attended a SQL Symphony Concert 3 times in december 2017
- That's a lot of clues!
***
````sql
SELECT 
    *
FROM
    drivers_license
        JOIN
    person ON drivers_license.id = person.license_id
        AND person.id IN (SELECT 
            person_id
        FROM
            (SELECT 
                *
            FROM
                facebook_event_checkin
            WHERE
                event_name = 'SQL Symphony Concert'
                    AND date >= 20171201
                    AND date <= 20171231
            ORDER BY person_id) first
        GROUP BY person_id
        HAVING COUNT(*) = 3)
WHERE
    hair_color = 'red'
        AND car_make = 'Tesla'
        AND car_model = 'Model S'
````
#### And et voilà!

Try the challenge to know how the culprit is!
