 The answer to some of these questions are influenced by BW - Chapter 5 -Shared variable-based synchronization and communication. I assume that this is okay.

- Condition variables, Java monitors, and Ada protected objects are quite similar in what they do (temporarily yield execution so some other task can unblock us). 
   - But in what ways do these mechanisms differ? 
        - Condition variables are kind of similar to semaphores, in that they use a type of waiting-mechanism, but unlike the wait()-function in a semaphore the same function regarding condition variables also always blocks, and notify allows a task to enter if there is one. Unlike protected objects in Ada and Java monitors, it seems from the book like condition variables are used in monitors to provide the condition synchronization needed, and usually not used by itself to fix both serialization of concurrent calls and the condition synchronization.
        - Protected objects in Ada are objects that give protected subprograms or protected entries access to a shared resource/ shared data, and the language causes the data to only be updated under mutual exlusion. Instead of for instance using condition variables to manage the synchronization part of things, Ada uses guards, "condition statements" that must evaluate true before allowing task entries. The difference is mostly the use of guards.
        - Java monitors put critical regions as procedures in a single module calles a monitor, which makes serializing easier. This part is similar to the concurrency-part of protected objects in Ada. In Java each object has a lock associated with it, that is influenced by the monitor-part of the program and can in that way provide condition synchronization. If some process then has synchronized written on its declaration, it can only run if it has the lock associated with the object it wants to use. What makes Java monitors stand out is that they don't have any explicit condition variables. Since Java uses notifyAll, all threads that are suspended will then be awoken, and they do not know wether their condition is true or not, like for protected objects for instance. The solution can be to use wait in a while-loop, which is inefficient, so protected objects in Ada seems more practical.

 - Bugs in this kind of low-level synchronization can be hard to spot.
   - Which solutions are you most confident are correct? Why, and what does this say about code quality?
        - I am most confident that the protected object with Ada works, because it is the language itself that guarantees that the tasks will be executed in mutual exclusion, and when changing so few codelines it would be kind of awkward if I managed to fuck this one up. I am not as confident in semaphores and conditional variables, but I am confident in the default-select one in Golang. The reason why I am not as confident in semaphores and condition variables is because there may be bugs somewhere there. If I have written a line that was supposed to be in the critical section for instance, this would be a bug that I assume would be hard to find if my problem was bigger. I think this is because unlike for instance the protected objects and guards in Ada, it will be harder to actually figure out where the critical section is when using this low-level synchronization. I think this says a lot about how important comments can be when the code is not self-explanatory. If I added a comment which said "CRITICAL SECTION HERE" in my code or something, the bug could have been easier to find for inexperienced programmers (like myself hehe). Ada is set up better, so I would have known that the bug would either have been in the guard condition or in the entries (now I assume that the problem is that processes with different priorities are not correctly executed).
   
 
 - We operated only with two priority levels here, but it makes sense for this "kind" of priority resource to support more priorities.
   - How would you extend these solutions to N priorities? Is it even possible to do this elegantly? 
      - For the for-select with default case in Golang I don't know how I would do this. It would probably not be a good idea to use default after default. Condition variables seem like they would have an elegant solution regarding this tho, as one could just expand the queue and use the same code. Semaphores also don't seem to have a way to do this elegantly, just by expanding the queue and adding else-if sentences. For the protected objects in Ada one could also add N entries and use the same logic with the guard conditions.
   - What (if anything) does that say about code quality?
      - It says that the conditional variables were maybe not so bad after all?
  
 - In D's standard library, `getValue` for semaphores is not even exposed (probably because it is not portable - Windows semaphores don't have `getValue`, though you could hack it together with `ReleaseSemaphore()` and `WaitForSingleObject()`).
   - A leading question: Is using `getValue` *ever* appropriate?
      - No, because this will allow someone to read a value that may change in an instant both with wait and signal. The wait and   signal executable are also meant to be atomic (does this matter?). Meant to be extremely fast - would waste CPU to check? Idk
   - Explain your intuition: What is it that makes `getValue` so dubious?
 
 - Which one(s) of these different mechanisms do you prefer, both for this specific task, and in general? (This is a matter of taste - there are no "right" answers here)





