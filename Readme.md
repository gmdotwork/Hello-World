# Requirements realization and explanation
1. When the app is started, the user is presented with the main menu, which allows the user to (1) enter or edit current job details, (2) enter job offers, (3) adjust the comparison
settings, or (4) compare job offers (disabled if no job offers were entered yet1).
	* **To realize this requirement we are starting with a user class having direct association with job details, current job, compare settings and compare job classes. Since job offers and current job share most of the attributes, a parent class 'JobDetails' is created. Both CurrentJob and JobOffer class inherit the attributes from its parent class 'JobDetails'.**
2. When choosing to enter current job details, a user will:<br/>
- a. Be shown a user interface to enter (if it is the first time) or edit all of the details of their current job, which consist of:
i. Title
ii. Company
iii. Location (entered as city and state)
iv. Cost of living in the location (expressed as an index )
v. Possibility to work remotely (expressed as the number of days a week one could work remotely, between 1 and 5)
vi. Yearly salary
vii. Yearly bonus
viii. Retirement benefits (as percentage matched)
ix. Leave time (vacation days and holiday and/or sick leave, as a single overall number of days)
	* **For this requirement, currentJob class is created which inherits all the job details as attributes like job title, company name, location, cost of living, remote work possibility, salary, bonus, retirement benefits and leave time from its parent class JobDetails. In addition to inherited attributes it has job start date, end date and boolean flag to suggest whether a job is current or not.**
- b. Be able to either save the job details or cancel and exit without saving, returning in both cases to the main menu.
    *	**Parent class 'JobDetails' has save and cancel operations/methods and they are inherited by currentJob class.**
	
3. When choosing to enter job offers, a user will:
a. Be shown a user interface to enter all of the details of the offer, which are the same ones listed above for the current job.
b. Be able to either save the job offer details or cancel.
c. Be able to (1) enter another offer, (2) return to the main menu, or (3) compare the offer with the current job details (if present).
	* **when a user performs an add or update action for Job Offers, 'JobOffer' class is called allowing user to enter job offer data, save, cancel without saving and return (with no action). For comparing the entered job offer, 'JobOffer' class gets the current job from 'currentJob' class and uses its association with 'CompareJob' class to perform job comparison.**
  
4. When adjusting the comparison settings, the user can assign integer weights to:
a. Possibility to work remotely
b. Yearly salary
c. Yearly bonus
d. Retirement benefits
e. Leave time
If no weights are assigned, all factors are considered equal.
	* **User can assign weights through compareSettings class**
5. When choosing to compare job offers, a user will:
a. Be shown a list of job offers, displayed as Title and Company, ranked from best to worst (see below for details), and including the current job (if present), clearly indicated.
b. Select two jobs to compare and trigger the comparison.
c. Be shown a table comparing the two jobs, displaying, for each job:
i. Title
ii. Company
iii. Location
iv. Commute time
v. Yearly salary adjusted for cost of living
vi. Yearly bonus adjusted for cost of living
vii. Retirement benefits (as percentage matched)
viii. Leave time
d. Be offered to perform another comparison or go back to the main menu.
  * **User performs select and compare operation through compareJob, the jobs are ranked according to the weighted score. CompareJob class has dependency on compareSettings class and gets parameter weights, current job from 'currentJob' class and job offers from its association with 'JobOffer' class.  **

6. When ranking jobs, a job’s score is computed as the weighted sum of:
AYS + AYB + (RBP * AYS) + (LT * AYS / 260) - ((260 - 52 * RWT) * (AYS / 260) / 8)
where:
AYS = yearly salary adjusted for cost of living
AYB = yearly bonus adjusted for cost of living
RBP = retirement benefits percentage
LT = leave time
RWD = remote work days
The rationale for the RWT subformula is:
a. value of an employee hour = (AYS / 260) / 8
b. commute hours per year (assuming a 1-hour/day commute) = 1 * (260 - 52 * RWT)
c. therefore commute-time cost = (260 - 52 * RWT) * (AYS / 260) / 8 
For example, if the weights are 2 for the yearly salary, 2 for the retirement benefits, and 1 for all other factors, the score would be computed as: 2/7 * AYS + 1/7 * AYB + 2/7 * (RBP * AYS) + 1/7 * (LT * AYS / 260) - 1/7 * ((260 - 52 * RWT) * (AYS / 260) / 8)
  * **This calculation takes place in CompareJob class taking help from its association to JobOffer and CompareSettings classes.**
7. The user interface must be intuitive and responsive.
8. For simplicity, you may assume there is a single system running the app (no communication or saving between devices is necessary).

# UML Diagram Elements

1. Classes
	* User
	* JobDetails
	* JobOffer
	* CurrentJob
	* CompareJob
	* CompareSettings
2. Attributes
	* JobDetails
	  * companyName as String
	  * location as String
	  * costOfLiving as int
	  * remoteWork as int
	  * salary as float
	  * bonus as float
	  * retirementBenefits as float
	  * leaves as int
	* CurrentJob
		* startDate as Date
		* endDate as Date
		* currentJob as boolean
	* CompareJob
		* rank as derived int
		* commuteTime as derived float
		* adjSalary as derived float
		* adjBonus as derived float
	* CompareSettings
	  * remoteWeight as int 
	  * salaryWeight as int
	  * bonusWeight as int
	  * retirementBenefitsWeight as int
	  * leaveWeight as int
3. Operations
	* User
		* addJobOffer
		* updateJobOffer()
		* addCurrentJob()
		* updateCurrentJob()
		* updateCompareSettings()
		* compareJob()
	* JobDetails
		* save()
		* cancel()
	* JobOffer
		* return()
		* getCurrentJob()
		* compareJob()
	* CompareJob
	  * getCurrentJob()
	  * getJobOffer()
	  * displayJobList()
	  * displayResultsTable
	  * return()
	* CompareSettings
	  * save()
	  * getWeights()
	  * displayJobList()
	  * displayResultsTable
	  * return()
4. Relationships
	* User can access CurrentJob, JobOffer, CompareSettings, CompareJob
	* User can compare job through JobOffer and CompareJob
	* CompareJob has a dependency on CompareSettings
	* CompareJob needs current job from CurrentJob class
	* JobOffer needs current job from CurrentJob class
