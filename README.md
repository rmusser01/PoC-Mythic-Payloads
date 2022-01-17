# PoC-Mythic-Payloads
- [Boa](#boa)
- [Dog](#dog)
- [Frog](#frog)
- [Ode](#ode)

### Proof-of-Concept Payload Types Based on Existing Mythic Payloads
- **101**
	* These are slimmed down payload types based on the existing C#, Go, Nim and Python payloads, available at https://github.com/orgs/MythicAgents/repositories.
	* Goal of this repo is to to be bare-bones examples of the payloads, implementing one or two operator exposed functions, tasking, and HTTP/S Comms.
	* Idea is to demonstrate what a simple, working payload looks like, along with providing a possible base for additional modifications and additions. This is to help with others in learning how to write a sample C2 agent, following an established model, with extensibility.
	* Each of the payloads have had their folder setup appropriately, such that you can clone/download this repo, and point mythic to any of the contained folders.
		* The designated payload should load without any issues.
- **Boa**<a name="boa"></a>
	- **Execution Flow**
- **Dog**<a name="dog"></a>
	- **Execution Flow**
- **Frog**<a name="frog"></a>
	- **Execution Flow**
- **Ode**<a name="ode"></a>
	- This is a fork/slimmed version of Atlas. (https://github.com/MythicAgents/atlas)
	- **Execution Flow**
		1. Execution Starts in `Main()` in `Program.cs`
		2. One function is immediately called,
            1. `Utils.GetServers();`
            	* This function's execution chain ends with a list of servers being added to the `Config.Servers` list
       	3. The next step is a compile-time flag seeing whether to validate the TLS certificate on callback.
       	4. Next up is a while loop, that checks for if the `HTTP.CheckIn` function returns false, and if so, generates an int and sleeps for that amount of time.
       		* `while (!Http.CheckIn())` - Evals whether `Http.CheckIn()` returns true.
       	5. After the `HTTP.CheckIn` is eval'd, a `JobList` object is created, from `Messages.JobList`. This sets the internal var `job_count` to 0, and `jobs{}` as an array.
       		* `Messages.JobList JobList = new Messages.JobList`
       		* This creates the List object `jobs`
       			* `public List<Job> jobs { get; set; }`
       		* This object also contains a function called `JobList()`
       			* This function creates a new variable `jobs` with the value of `new List<Job>()`
       	6. Finally, we have a while loop `while (true)`, which performs 3 tasks:
       		1. `Dispatch.Loop(JobList);`
       			* This function, as the name implies, first performs an `if/else` check regarding the current date, and should that be satisfied, continues on to:
       				* Retrieve tasking using `Http.GetTasking(JobList);`
       				* Perform said tasking using: `StartDispatch(JobList);`
       				* Returns the results of task execution to the Mythic server: `Http.PostResponse(JobList);`
       		2. `int Dwell = Utils.GetDwellTime();`
       			* `GetDwellTime()`
       			* Creates a high and low number using `Config.Sleep + (Config.Sleep * (Config.Jitter * 0.01))` and `Random()` to pick a 'random' number for each one.
       				* `int Dwell = random.Next(Convert.ToInt32(Low), Convert.ToInt32(High));`
       			* Then multiplies the result by `1000`, and returns that as the value for `Dwell`
       	   	3. `System.Threading.Thread.Sleep(Dwell);`
                * Self-explanatory, process 'sleeps' for the `Dwell` time value established above.
    - **Understanding Tasking of Ode**
    	0. Tasking occurs During Step 6 above, specifically with the `Dispatch.Loop(JobList);` function.
    	1. 















