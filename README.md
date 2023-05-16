# Algorithm Design and Analysis
## Final Project
The project is has 2 problems:
### Problem 1
Given a set of **_N_** jobs where each jobi has a deadline and profit associated with it. Each **_job<sub>i</sub>_** takes **_1_** unit of time to complete and only one job can be scheduled at a time. We earn the profit associated with job if and only if the job is completed by its deadline. Find the **_number of jobs_** done and the **_maximum profit_**. Jobs will be given in the form `(job_id, deadline, profit)` associated with that Job.

**Sample Input**
```python
jobs = {(1,4,20),(2,1,10),(3,1,40),(4,1,30)}
```

**Solution**
```python
#GREEDY ALGORITHM 
def max_profit(jobs):
# Sort jobs based on their profits in descending order
    jobs.sort(key=lambda x: x[2], reverse=True)
      # initialize empty schedule and set of used time slots
    n=len(jobs) 
    schedule = [None] * n
    used_slots = set()  # Set to keep track of used time slots
     # assign jobs to latest possible time slot before their deadline
    for job in jobs:
        for slot in range(min(job[1], n), 0, -1):
             # Check for available time slots in reverse order (latest possible time slots)
            if slot not in used_slots:
                # If the time slot is not used
                # Assign the job to the time slot
                schedule[slot-1] = job
                used_slots.add(slot) # Add the time slot to the set of used slots
                break   #Move on to the next job
    # compute total profit and number of assigned jobs   
    total_profit = sum(job[2] for job in schedule if job is not None)  #Calculate the total profit of assigned jobs
    num_jobs = sum(1 for job in schedule if job is not None)    #Count the number of assigned jobs
    
    return num_jobs, total_profit
```

### Problem 2
You are given a list of **_N_** jobs with their start time, end time, and the profit you can earn by doing that job. Your task is to find the **_maximum profit_** you can earn by picking up some (or all) jobs, ensuring **_no two jobs overlap_**. If you choose a job that ends at time **_X_**, you will be able to start another job that starts at time **_X_**.

**Sample Input**
```python
jobs = {(1, 6, 6), (2, 5, 5), (5, 7, 5), (6, 8, 3)}
```
**Solution**
```python
def find_max_profit(jobs):
    jobs.sort(key=lambda x: x[1])  # Sort jobs by end times

    n = len(jobs)
    dp = [0] * n
    dp[0] = jobs[0][2]  # Initialize dp[0] with the profit of the first job

    for i in range(1, n):
        last = None
        j = i - 1

        # Find the latest job that ends before the start time of the current job
        while j >= 0:
            if jobs[j][1] <= jobs[i][0]:
                last = j
                break
            j -= 1

        if last is not None:
            dp[i] = max(dp[last] + jobs[i][2], dp[i - 1])
        else:
            dp[i] = max(jobs[i][2], dp[i - 1])
    return dp[n - 1]  # Return the maximum profit
```
