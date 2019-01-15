# ScheduleSolver
One evening, I found myself sitting in the DBC Beer Hall talking to Jessica, the owner. She was working on the schedule when I arrived and the bar was more or less empty. An hour or two went by, I spoke with a couple of bar patrons and Jessica, things were pretty easy going. Then Jessica said something else about the schedule. I was stunned and asked, "Wait, I thought you used that application ***! Doesn't it sort the schedule for you? You were working on that over an hour ago!?" Jessica informed me that the application allowed everyone to input their availability, but did not actually perform the operation of solving for a proposed schedule. I am still aghast. This is what computers are for! This is computing. 

## Enter the Schedule Solver
I did look around at the various available applications in the problem space. It would seem that no one has taken on this problem with any real interest. I spoke with a few other business owners. They had looked too, except Massoud at Backstage. He enjoys the quiet puzzle solving ritual once a week. 

## How it Works
**Early Beta**

In early beta, the Solver will allow access to managers/owners with no interface for employees. In late beta, functionality for employees to edit their own profiles and for intra organizational messaging will be necessary. Right now, we just want single accounts with no relation to any others, capable of creating and storing schedules.


The Schedule Solver sequence is as follows:
* User creates a schedule of one of two types:
	* A Weekly Schedule: Sun – Sat, _meant to repeat_
	* A Span Schedule: contiguous segment of up to 35 calendar days
* User is prompted to save (and setup an account), but isn't required to.
* User creates employees
* User is strongly encouraged to save and sign-up (if they haven't)
* User is prompted to enter employee availability into a template generated to match the schedule which the user has provided. 
* Once availability has been entered for the final employee a "SOLVE" button will appear, and maybe pulse. 


### Schedule Creation
Manager will first be prompted to choose to create either a weekly repeating schedule or a schedule for a specific span up to 35 days.

Then, the manager will be given the shift creation window. In the weekly view they will have a row of checkboxes that gives the option of applying the day plan in question to any number of the days of the week. In the span view they will be shown a dropdown calendar spanning their selected time, for the same purpose.

Manager will be given a view of a day from 4am – 4am. The day will be pre-populated with three editable shift boxes: open, mid, close. Selecting a shift will bring its details into the editing area. The manager may select any box to edit its contents. They may change the name of the shift, change the job title names, adjust the number of employees of each title needed, determine the beginning and ending times of that shift, and add or remove shifts.

From this view the manager may move forward to view their full schedule, move backward to the choice concerning weekly versus span schedule creation, or make another day template. 

The manager will be able to determine the starting point of the iteration cycle. Shifts immediately following the starting point are seen as more valuable. 

From the full week view the manager may move on to employee creation.

### Employee Creation
Enter the following:
- Name
- Start Date (used to determine seniority, _basis for schedule preference_)
- Email (will be used when employee invites are a thing)
- Weekly hours preference
- Which types of roles the employee may occupy (created in the schedule making process)

### Employee Availability
**Early Beta**
For the time being, we'll only be concerning ourselves with interacting with owners/managers. It will be on the manager to enter the availability for each employee. Rather than entering availability as a binary, we will allow for four states per shift:
	* Preferred
	* Available
	* Unavailable
	* Unspecified/Ask

**Late Beta**
Before we exit beta, we obviously want to incorporate employee login and schedule editing. We probably want to do this around the time we incorporate a messaging system and a live calendar view. 

### SOLVING!

The solving function can be entirely self contained only returning its solution. It involves the assignment of points based on seniority and the the spending of these points. These points need not persist beyond any particular round of solving and are not stored in memory on the persistent employee objects themselves. 
* assign points based on seniority
	The early algorithm should be kept very simple. It will be Deacon's job to interact with a beta-test group of business owners and managers to get actual feedback on the efficacy of the solver. 

	For now, let us simply give each employee one point for every day they've been employed. 
* rank employees by points
	The function will need to maintain an array of employees which will be resorted after every assigned shift. We'll call it the sorting ledger.
* iterate through days beginning with user defined starting point or …
	In this process, we will go from shift to shift, in sequence, assigning the shift, adjusting the sorting ledger, and moving on in direct chronological order.

	_My hypothesis, and I do intend to get feedback on this point, is that in any given service business the busiest days are clustered. In a to-go focused coffee shop on a high morning traffic street the cluster will be centered on Tuesday's opening shift. In a restaurant/bar the center of the cluster will be Friday dinner/close. Since there are businesses in the later category, we should set the default starting point just before Friday dinner._ 
* begin iteration at last shift Fri.
	I should further note that part of my hypothesis is that, while we could allow the user to hand rank the value of each shift, it does not serve for the following reasons:
		* It would only have value to a small number of venues.
		* Shift valuation is somewhat subjective, dependent on circumstances of the employees life.
		* Shift valuation is handled in more fine grain detail by employee availability input.
* at first shift
	* create array of employees who want shift (shift marked as preferred)
	* assign shift to employee with most points
	* deduct 5 points from employee
	* if no employee wants shift, create array of available employees
	* sort array according to desired hours
	* assign shift to employee with highest desired hours
	* do not deduct points from employee
	* if no available employees, make shift uncovered 
	* proceed to next shift

* repeat for each shift until all shifts have been assigned or marked
* display completed schedule. 
	It is possible for a schedule to have holes. The function should still run and return a solution if a restaurant has three employees input who only want five hours a week and are only available on Wednesday morning. It simply marks everything else as unfilled.
* make completed schedule available for download as PDF
* prompt user to save schedule to their account. 

### User Management
**Early Beta**
In early beta we'll only have one kind of user and one level of permissions. We'll want to keep a white-list of emails for those invited to keep scale down for a bit. Anyone on that list will be able to create and save a listing of employees and schedules.  

**Late Beta**
* Organization Calendar
Before including anyone other than the owner, there is no need to have a persistent organization calendar; none of the employees can see it. A PDF will suffice. In late beta, we introduce the persistent calendar. 

_I'm going to bullet in some of the ideas about the persistent calendar here, but not detail them because they aren't pressing._
* Employees set their weekly availability in the four categories previously described.
* Employees can set unavailability and make specific requests in a full calendar view.
* When the sorter goes to fetch the employee's availability it will overlay the specific/calendar availability for the time span chosen by the manager and overwrite any differences in weekly selection with those. 
* This eliminates some of the this/all edit failures of applications like When I Work ( _which doesn't even solve!?_ ).
* The presence of an organizational calendar doesn't mean that the solving function runs continual whenever the calendar is being viewed. The owner must still create schedules and commit then into the persistent schedule before they become official.
* However, it would allow anyone within the organization to run a speculative forcast of the organization's schedule based on current data for any 35 day period in the future. This feature could be disabled by the owner.

* Owner
	The flat and global permissions associated with the early beta account. Now, owners will be able to add managers and employees as well. 

* MGMT
	Managers can add and remove employees, but cannot add or remove managers. Managers can create and save schedules, Push schedules live to their organization calendar, edit their availability (though they don't strictly speaking need to be on the schedule), see their organization's schedule, and message anyone within their organization.

* Employee
Employees cannot add or remove anyone. Employees can edit their availability, see their organization's schedule, and message anyone within their organization. 

### Messaging
**Early Beta**
There will only be managers in the system and they should be able to send me an email easily. I'll add an icon for it to the nav bar.

**Late Beta**
We include employees and the ability to message anyone within a business, in app. Design specs coming soon.

_One significant advantage that we have over other systems is that we do not have any need to build as robust a messaging system. Other solutions in this problem space depend mostly on messaging to determine schedules. Our solution uses solutions to determine and messaging to patch last minute changes of circumstance._

While in beta we will allow all users to share SQGLZ, meaning Deacon, in on their messages easily, probably even make it the default. We want to see how it's working.

**At Launch**
We remove the 'trivially bother Deacon' feature.

### Landing Page


### Accounts
* email address
* name
* 30 day trial w/out payment information
* stripe reoccurring payment after 30 days
* information saved and available in the event of non-payment
* no edit or new schedule functionality in the event of non-payment
* up to 150 employees total
* up to 50 saved schedules









