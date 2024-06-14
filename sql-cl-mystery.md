My Notes for The SQL Murder Mystery (https://mystery.knightlab.com/)

A crime has taken place and the detective needs your help. The detective gave you the crime scene report, but you somehow lost it. You vaguely remember that the crime was a ​murder​ that occurred sometime on ​Jan.15, 2018​ and that it took place in ​SQL City​. Start by retrieving the corresponding crime scene report from the police department’s database.

```sql
SELECT * 
FROM crime_scene_report 
WHERE type = 'murder' 
  AND city = 'SQL City';
```

20180115	murder	Security footage shows that there were 2 witnesses. The first witness lives at the last house on "Northwestern Dr". The second witness, named Annabel, lives somewhere on "Franklin Ave".	SQL City

```sql
SELECT * 
FROM person 
WHERE address_street_name = 'Northwestern Dr' 
ORDER BY address_number DESC;
```

First Witness:
id	name	license_id	address_number	address_street_name	ssn
14887	Morty Schapiro	118009	4919	Northwestern Dr	111564949

person_id	transcript
14887	I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W".

Second Witness:
id	name	license_id	address_number	address_street_name	ssn
16371	Annabel Miller	490173	103	Franklin Ave	318771143

Annabel's Interview Transcript
I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.

membership_id	check_in_date	check_in_time	check_out_time
X0643	20180109	957	1164
UK1F2	20180109	344	518
XTE42	20180109	486	1124
1AE2H	20180109	461	944
6LSTG	20180109	399	515
7MWHJ	20180109	273	885
GE5Q8	20180109	367	959
48Z7A	20180109	1600	1730
48Z55	20180109	1530	1700
90081	20180109	1600	1700

48Z7A	20180109	1600	1730
48Z55	20180109	1530	1700

```sql
SELECT * 
FROM get_fit_now_member 
WHERE id LIKE '48Z%';
```

id	person_id	name	membership_start_date	membership_status	city
-- 48Z38	49550	Tomas Baisley	20170203	silver	null

48Z7A	28819	Joe Germuska	20160305	gold	null
NO DRIVER'S LICENSE

48Z55	67318	Jeremy Bowers	20160101	gold	null
id	age	height	eye_color	hair_color	gender	plate_number	car_make	car_model
423327	30	70	brown	brown	male	0H42W2	Chevrolet	Spark LS

Congrats, you found the murderer! But wait, there's more... If you think you're up for a challenge, try querying the interview transcript of the murderer to find the real villain behind this crime. If you feel especially confident in your SQL skills, try to complete this final step with no more than 2 queries. Use this same INSERT statement with your new suspect to check your answer.

person_id	transcript
67318	I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017.

```sql
SELECT * 
FROM drivers_license 
WHERE height BETWEEN 65 AND 67
  AND hair_color = 'red' 
  AND car_make = 'Tesla' 
  AND car_model = 'Model S';
```

Sure, to look up the `drivers_license` table with the height in the range of 65 to 67 (inclusive) and the other specified conditions, you can use the following SQL query:

```sql
SELECT person_id, count(*), event_name
FROM facebook_event_checkin 
GROUP BY person_id
having count(*) = 3 AND event_name = "SQL Symphony Concert" AND date like "%201712%";
```

- **SELECT person_id, count(*), event_name**: This line specifies the columns to be returned in the result set. It selects the `person_id`, the count of rows for each `person_id` (using `count(*)`), and the `event_name`.

- **GROUP BY person_id**: This line groups the result set by the `person_id` column. This means that the `count(*)` will be calculated for each unique `person_id`.

- **HAVING count(*) = 3 AND event_name = "SQL Symphony Concert" AND date like "%201712%"**: This line filters the groups created by the `GROUP BY` clause. It ensures that only groups (i.e., `person_id`s) with exactly 3 check-ins are included, and that these check-ins are for the event named "SQL Symphony Concert" and occurred in December 2017 (as indicated by the date pattern `"%201712%"`).

**HIRED THE HITMAN: Miranda Priestly**

---

The Command Line Murders

(base) PS E:\sports\book-shelf\clmystery\mystery> Get-Content "crimescene" | Out-File -FilePath "cs.txt"

(base) PS E:\sports\book-shelf\clmystery\mystery>  Select-String -Path "cs.txt" -Pattern "^CLUE:"

cs.txt:9213:CLUE: Footage from an ATM security camera is blurry but shows that the perpetrator is a tall    
male, at least 6'.
cs.txt:9370:CLUE: Found a wallet believed to belong to the killer: no ID, just loose change, and membership 
cards for AAA, Delta SkyMiles, the local library, and the Museum of Bash History. The cards are totally     
untraceable and have no name, for some reason.
cs.txt:11002:CLUE: Questioned the barista at the local coffee shop. He said a woman left right before they  
heard the shots. The name on her latte was Annabel, she had blond spiky hair and a New Zealand accent. 

- Male, at least 6 foot tall
- Member of AAA, Delta SkyMiles, local library, Museum of Bash
- Connection to Annabel, blond spiky hair, New Zealand accent