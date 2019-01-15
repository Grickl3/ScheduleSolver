# ScheduleSolver
One evening, I found myself sitting in the DBC Beer Hall talking to Jessica, the owner. She was working on the schedule when I arrived and the bar was more or less empty. An hour or two went by, I spoke with a couple of bar patrons and Jessica, things were pretty easy going. Then Jessica said something else about the schedule. I was stunned and asked, "Wait, I thought you used that application ***! Doesn't it sort the schedule for you? You were working on that over an hour ago!?" Jessica informed me that the application allowed everyone to input their availability, but did not actually perform the operation of solving for a proposed schedule. I am still agast. This is what computers are for! This is computing. 

## Enter the Schedule Solver
I did look around at the various available applications in the problem space. It would seem that no one has taken on this problem with any real interest. I spoke with a few other business owners. They had looked too, except Masoud at Backstage. He enjoys the quiet puzzle solving ritual once a week. 

## How it Works
**Beta Phase**
For Beta, the Solver will only feature user management for managers/owners and not functionality for employees. At full release, functionality for employees to edit their own profiles and full message functionality should be included, but not at present. Right now, we just want single accounts with no relation to any others, capable of creating and storing schedules.


The Schedule Solver sequence is as follows:
* User creates a schedule of one of two types:
	* A Weekly Schedule: Sun – Sat, _meant to repeat_
	* A Span Schedule: contiguous segment of up to 35 calendar days
* User is prompted to save (and setup an account), but isn't required to.
* User creates employees, initial information includes:
	* Name
	* Start Date (used to determine seniority, _basis for schedule preference_ )
	* Email (will be used when employee invites are a thing)
	* Weekly hours preference
	* Which types of roles the employee may occupy (created in the schedule making process, _see schedule creation below_ )
* User is strongly encouraged to save and signup (if they haven't)
* User is prompted to enter employee availability into a template generated to match the schedule which the user has provided. 
* Once availability has been entered for the final employee a "SOLVE" button will appear, and maybe pulse. 


### Schedule Creation
**Weekly Schedules**
- allow user to determine schedule start

**Span Schedules**
- allow owners to make some shifts more valuable than others
- allow user to determine schedule start

### Employee Creation


### Employee Availability


### SOLVING!
- assign points based on seniority
- rank employees by points
- inerate through days beginning with user defined starting point or …
- begin iteration at last shift Fri.
- at first shift
	- create array of employees who want shift
	- assign shift to employee with most points
	- deduct points from employee
	- if no employee wants shift, create array of available employees
	- sort array according to desired hours
	- assign shift to employee with highest desired hours
	- do not deduct points from employee
	- if no available employees, make shift uncovered 
- repeat for each shift until all shifts have been assigned or marked

### User Management
- admin, mgmt, employee
- 

### Landing Page


### Accounts
- email address
- name
- 30 day trial w/out payment information
- stripe reoccuring payment after 30 days
- information saved and available in the event of non-payement
- no edit or new schedule functionality in the event of non-payment
- up to 150 employees total
- up to 50 saved schedules









