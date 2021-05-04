- Condition variables, Java monitors, and Ada protected objects are quite similar in what they do (temporarily yield execution so some other task can unblock us). 
   - But in what ways do these mechanisms differ? 
        -A condition variable blocks a thread using 
        - Protected objects in Ada use guards. They get a "condition" that must be true before unblocking the thread, and when that condition is false they just keep blocking it. This is checked then another thread finishes its work, so it always keeps track. 
        - Java monitors

 - Bugs in this kind of low-level synchronization can be hard to spot.
   - Which solutions are you most confident are correct? Why, and what does this say about code quality?
        - I am confident that the select-method with golang works, because it is the method I am most used to from the elevator-lab,  and it just makes sense to me that it should be written that way, because of how the default-case works. I am also confident that the protectedobj written in Ada is correct, because of the very small changes I had to make in order to make the problem work as it should. My other codes did not actually manage to do what they should, sooo yeah :) I think that what is best with the two I like the most is that they are in general easy to read and understand. For condvar and semaphores I think the problem is that it for me is hard to read what actually happens and keep track of it (but this may not be due to code quality, but more due to me not being as comfortable with semaphores and conditionvariables as I maybe should be by now hehe). request also demanded some logic, and I should probably have given better names to have better code quality here. I think the naming and commenting is really the most important for these codes.
   
 
 - We operated only with two priority levels here, but it makes sense for this "kind" of priority resource to support more priorities.
   - How would you extend these solutions to N priorities? Is it even possible to do this elegantly? 
   - What (if anything) does that say about code quality?
  
 - In D's standard library, `getValue` for semaphores is not even exposed (probably because it is not portable - Windows semaphores don't have `getValue`, though you could hack it together with `ReleaseSemaphore()` and `WaitForSingleObject()`).
   - A leading question: Is using `getValue` *ever* appropriate?
   - Explain your intuition: What is it that makes `getValue` so dubious?
 
 - Which one(s) of these different mechanisms do you prefer, both for this specific task, and in general? (This is a matter of taste - there are no "right" answers here)