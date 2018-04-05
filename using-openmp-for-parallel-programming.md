<div id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#sec-1">1. Chapter04 OpenMP Language Features</a>
<ul>
<li><a href="#sec-1-1">1.1. 4.1 Introduction</a>
<ul>
<li><a href="#sec-1-1-1">1.1.1. Most commonly used features of the OpenMP library routines:</a></li>
<li><a href="#sec-1-1-2">1.1.2. Features enable the programmer to orchestrate the actions of different threads (4.6)</a></li>
</ul>
</li>
<li><a href="#sec-1-2">1.2. 4.2 Terminology</a></li>
<li><a href="#sec-1-3">1.3. 4.3 Parallel Construct</a></li>
<li><a href="#sec-1-4">1.4. 4.4 Sharing the Work among Threads in an OpenMP Program</a>
<ul>
<li><a href="#sec-1-4-1">1.4.1. 4.4.1 Loop Construct</a></li>
<li><a href="#sec-1-4-2">1.4.2. 4.4.2 The Section Construct</a></li>
<li><a href="#sec-1-4-3">1.4.3. 4.4.3 The Single Construct</a></li>
<li><a href="#sec-1-4-4">1.4.4. 4.4.4 Workshare Construct</a></li>
<li><a href="#sec-1-4-5">1.4.5. 4.4.5 Combined Parallel Work-sharing Construct</a></li>
</ul>
</li>
<li><a href="#sec-1-5">1.5. 4.5 Clauses to Control Parallel and Work-sharing Construct</a>
<ul>
<li><a href="#sec-1-5-1">1.5.1. 4.5.1 Shared Clause</a></li>
<li><a href="#sec-1-5-2">1.5.2. 4.5.2 Private Clause</a></li>
<li><a href="#sec-1-5-3">1.5.3. 4.5.3 Lastprivate Clause</a></li>
<li><a href="#sec-1-5-4">1.5.4. 4.5.4 Fristprivate Clause</a></li>
<li><a href="#sec-1-5-5">1.5.5. 4.5.5 Default Clause</a></li>
<li><a href="#sec-1-5-6">1.5.6. 4.5.6 Nowait Clause</a></li>
<li><a href="#sec-1-5-7">1.5.7. 4.5.7 Schedule Clause</a></li>
</ul>
</li>
<li><a href="#sec-1-6">1.6. 4.6 OpenMP Synchronization Construct</a>
<ul>
<li><a href="#sec-1-6-1">1.6.1. 4.6.1 Barrier Construct</a></li>
<li><a href="#sec-1-6-2">1.6.2. 4.6.2 Ordered Construct</a></li>
<li><a href="#sec-1-6-3">1.6.3. 4.6.3 Critical Construct</a></li>
<li><a href="#sec-1-6-4">1.6.4. 4.6.4 Atomic Construct</a></li>
<li><a href="#sec-1-6-5">1.6.5. 4.6.5 Locks</a></li>
<li><a href="#sec-1-6-6">1.6.6. 4.6.6 Master Construct</a></li>
</ul>
</li>
<li><a href="#sec-1-7">1.7. 4.7 Interaction with the Execuation Environment</a></li>
<li><a href="#sec-1-8">1.8. 4.8 More OpenMP Clauses</a>
<ul>
<li><a href="#sec-1-8-1">1.8.1. 4.8.1 If Clause</a></li>
<li><a href="#sec-1-8-2">1.8.2. 4.8.2 Num<sub>threads</sub> Clause</a></li>
<li><a href="#sec-1-8-3">1.8.3. 4.8.3 Ordered Clause</a></li>
<li><a href="#sec-1-8-4">1.8.4. 4.8.4 Reduction Clause</a></li>
<li><a href="#sec-1-8-5">1.8.5. 4.8.5 Copying Clause</a></li>
<li><a href="#sec-1-8-6">1.8.6. 4.8.6 Copyprivate Cluase</a></li>
</ul>
</li>
<li><a href="#sec-1-9">1.9. 4.9 Advanced OpenMP Construct</a>
<ul>
<li><a href="#sec-1-9-1">1.9.1. 4.9.1 Nested Parallelism</a></li>
<li><a href="#sec-1-9-2">1.9.2. 4.9.2 Flush Directive</a></li>
<li><a href="#sec-1-9-3">1.9.3. 4.9.3 Threadprivate Directive</a></li>
</ul>
</li>
<li><a href="#sec-1-10">1.10. Summary</a></li>
</ul>
</li>
<li><a href="#sec-2">2. Chapter05 How to Get Good Performance by Using OpenMP</a></li>
</ul>
</div>
</div>

# Chapter04 OpenMP Language Features<a id="sec-1" name="sec-1"></a>

## 4.1 Introduction<a id="sec-1-1" name="sec-1-1"></a>

### Most commonly used features of the OpenMP library routines:<a id="sec-1-1-1" name="sec-1-1-1"></a>

1.  Parallel Construct

2.  Work-sharing Construct

    1.  Loop Construct
    2.  Section Construct
    3.  Single COnstruct
    4.  Workshare Construct(Fortran only)

3.  Data-sharing, No Wait, and Schedule Clauses (see [OpenMP Clauses](https://msdn.microsoft.com/en-us/library/2kwb957d.aspx))

### Features enable the programmer to orchestrate the actions of different threads (4.6)<a id="sec-1-1-2" name="sec-1-1-2"></a>

1.  Barrier Construct

2.  Critical Construct

3.  Atomic Construct

4.  Locks

5.  Master Construct

## 4.2 Terminology<a id="sec-1-2" name="sec-1-2"></a>

-   OpenMP Directive
-   Executable directive
-   Construct &#x2013; An OpenMP executable directive and the associated statement, loop, or structured block.

## 4.3 Parallel Construct<a id="sec-1-3" name="sec-1-3"></a>

    #pragma omp parallel
       {
         printf("The parallel region is executed by thread %d\n",
                omp_get_thread_num());
         if ( omp_get_thread_num() == 2 ) {
           printf("  Thread %d does things differently\n",
                  omp_get_thread_num());
         }  /*-- End of parallel region --*/

-   The construct is used to specify the computation that should be executed in parallel
-   A team of treads is created; But it does not distribute the work of the region among the threads in paralle.
-   If user does not use appropriate syntax to specify the action, the work will be replicated
-   Implied barrier that forces all threads to wait until the work inside the region has been completed.
-   The thread that encounters the parallel construct becomes the *master* of the new team.
-   `omp get thread num()` is used to obtain the number of each thread executing the parallel region
-   Note that one cannot make any assumptions about the order in which the threads will execute the first `printf` statement.

## 4.4 Sharing the Work among Threads in an OpenMP Program<a id="sec-1-4" name="sec-1-4"></a>

OpenMP's Work-sharingconstructs are used to distribute computation among the threads in a team.
Two main rules regarding work-sharing construct are as follow:
-   Each work-sharing region must be encountered by all threads in a team or by none at all.
-   THe sequence of work-sharing regions and barrier regions encountered must be the same for every thread in a team

### 4.4.1 Loop Construct<a id="sec-1-4-1" name="sec-1-4-1"></a>

-   The loop must an inteeger counter variable whose value is changed by a fixed amount at each iteration until a specified bound is reached.

    #pragma omp parallel shared(n) private(i)
    {
    #pragma omp for
      for (i=0; i<n; i++)
        printf("Thread %d executes loop iteration %d\n",
               omp_get_thread_num(),i);
    } /*-- End of parallel region --*/

-   use a parallel directive to define a parallel region, then share its work among threads via the `for` work sharing directive
-   the `#pragma omp for` directive states that iterations of the loop following it will be distributed.
-   Clauses state which data in the region is shared and which is private.
-   The order of result is **not** deterministic.

### 4.4.2 The Section Construct<a id="sec-1-4-2" name="sec-1-4-2"></a>

-   It let us specify several different code regions to get different threads to carry out different kinds of work.
-   It consists of two directives:
    -   first, `#pragma omp sections` indicate the start of the construct.
    -   second, `#pragma omp section` mark each distinct section.
-   Each section must be a structured block of code that is independent of the other section.
-   At run time, the specified code blocks are executed by the threads in the team. Each thread executes one code block at a time, and each code block will be executed exactly once.
-   The most common use is to execute function or subroutine calls in parallel.

1.  Example of parallel sections

        #pragma omp parallel
        {
        #pragma omp sections
          {
        #pragma omp section
            (void) funcA();
        #pragma omp section
            (void) funcB();
          } /*-- End of sections block --*/
        } /*-- End of parallel region --*/
    
    1.  It contains one `sections` construct comprising two sections.
    2.  It limits the parallelism to two threads.
    3.  **Note** one cannot make any assumption on the specific order in which section blocks are executed. Even if when there are only thread which makes it executed sequentially.

### 4.4.3 The Single Construct<a id="sec-1-4-3" name="sec-1-4-3"></a>

-   The *single construct* is associated with the structured block of code immediately following it and specifies that this block should be executed by one thread only.

    #pragma omp parallel shared(a,b) private(i)
    {
    #pragma omp single
      {
        a = 10;
        printf("Single construct executed by thread %d\n",
               omp_get_thread_num());
        /* A barrier is automatically inserted here */
    #pragma omp for
        for (i=0; i<n; i++)
          b[i] = a;
      } /*-- End of parallel region --*/
      printf("After the parallel region:\n");
      for (i=0; i<n; i++)
        printf("b[%d] = %d\n",i,b[i]);

-   One thread initializes the shared variable `a`. This variable is then used to initialize vector `b` in the parallelized `for`-loop.
-   One might think the single construct could be ommited in this case, since every thread would write the same value of 10 to the same variable `a`. **It is not true!**.
    -   multiple threads could cause memory consistency problem.
    -   multiple stores to the memory address could cause bad performance.
-   **A barries is enssential before the `#paragma omp for` loop**

### 4.4.4 Workshare Construct<a id="sec-1-4-4" name="sec-1-4-4"></a>

(only supported in Fortran)

### 4.4.5 Combined Parallel Work-sharing Construct<a id="sec-1-4-5" name="sec-1-4-5"></a>

-   Combined parallel work-sharing constructs are shortcuts that can be used when a parallel region comprises precisely one work-sharing construct, that is, the work- sharing region includes all the code in the parallel region.

## 4.5 Clauses to Control Parallel and Work-sharing Construct<a id="sec-1-5" name="sec-1-5"></a>

-   The OpenMP directive support a number of *clauses*, optional additions that control the behavior of the construct they apply to.
-   Since the caluses are evaluated in external context, thus any variables that appear in them must be defined there.

### 4.5.1 Shared Clause<a id="sec-1-5-1" name="sec-1-5-1"></a>

-   specify which data will be shared among the threads, so there is one unique instance of these variables, and each thread can freely read or modify the values.
-   be careful about memory consistency.

### 4.5.2 Private Clause<a id="sec-1-5-2" name="sec-1-5-2"></a>

    #pragma omp parallel for private(i,a)
    for (i=0; i<n; i++)
      {
        a = i+1;
        printf("Thread %d has a value of a = %d for i = %d\n",
               omp_get_thread_num(),a,i);
      } /*-- End of parallel for --*/

-   The values of private data are *undefined* upon entry to and exit from the specific construct, even if the corresponding variable was defined prior to the region. **Be careful!**

### 4.5.3 Lastprivate Clause<a id="sec-1-5-3" name="sec-1-5-3"></a>

-   It ensures that the last value of a data object listed is accessible after the corresponding construct has completed execution.

    #pragma omp parallel for private(i) lastprivate(a)
    for (i=0; i<n; i++)
      {
        a = i+1;
        printf("Thread %d has a value of a = %d for i = %d\n",
               omp_get_thread_num(),a,i);
      } /*-- End of parallel for --*/
    printf("Value of a after parallel for: a = %d\n",a);

-   The same functionality can be implemented by using an additional shared variable.

    #pragma omp parallel for private(i) private(a) shared(a_shared)
    for (i=0; i<n; i++)
      {
        a = i+1;
        printf("Thread %d has a value of a = %d for i = %d\n",
               omp_get_thread_num(),a,i);
        if ( i == n-1 ) a_shared = a;
      } /*-- End of parallel for --*/

### 4.5.4 Fristprivate Clause<a id="sec-1-5-4" name="sec-1-5-4"></a>

The private data is also undefined on entry to the construct where it is specified. This could be a problem if we need to pre-initialize private variable with values that are available prior to the region in which they will be used.
-   use `firstprivate` construct to help out.
-   `firstprivate` cluase is supported on `parallel` construct, plus the work-sharing `loop`, `sections`, and `single` constructs.

    for(i=0; i<vlen; i++) a[i] = -i-1;
    
    indx = 4;
    #pragma omp parallel default(none) firstprivate(indx)   \
      private(i,TID) shared(n,a)
    TID = omp_get_thread_num();
    
    indx += n*TID;
    for(i=indx; i<indx+n; i++)
      a[i] = TID + 1;
    } /*-- End of parallel region --*/
    
    printf("After the parallel region:\n");
    for (i=0; i<vlen; i++)
      printf("a[%d] = %d\n",i,a[i]);

-   The code block shows each thread in a parallel region needs access to a thread-specific section of a vector but access start at a offset.
-   The offset = 4.
-   The length of each thread's section of the array is ginven by `n`.
-   This example could be implemented by using a shared variable, `offset` which contains the initial offset into vector `a`.

    #pragma omp parallel default(none) private(i,TID,indx)  \
      shared(n,offset,a)
       {
         TID = omp_get_thread_num();
         indx = offset + n*TID;
         for(i=indx; i<indx+n; i++)
           a[i] = TID + 1;
       } /*-- End of parallel region --*/

-   In general, *read-only* variables can be passed in as `shared` variables instead of `firstprivate`.

### 4.5.5 Default Clause<a id="sec-1-5-5" name="sec-1-5-5"></a>

-   It is used to give variables a default data-sharing attribute: `default(shared)` assigns the shared attribute to all variables referenced in the construct.

### 4.5.6 Nowait Clause<a id="sec-1-5-6" name="sec-1-5-6"></a>

It allows the programmer to fine-tune a program's performance.
-   It could suppress the implicit barrier at the end of work-sharing constructs. So when threads reach the end of the construct, they will immediately proceed to perform other work.
-   **Note**, the barrier at the end of a parallel region cannot be suppressed.
-   Once a parallel program runs correctly, one can try to identify places where a barrier is not needed and insert the `nowait` clause.

    #pragma omp for nowait
    for (i=0; i<n; i++)
      {
        ............
      }

### 4.5.7 Schedule Clause<a id="sec-1-5-7" name="sec-1-5-7"></a>

The `schedule` clause is supported on the loop construct only. It is used to control the manner in which loop iterations are distributed over the threads.

    #pragma omp parallel for default(none) schedule(runtime)        \
      private(i,j) shared(n)
    for (i=0; i<n; i++)
      {
        printf("Iteration %d executed by thread %d\n",
               i, omp_get_thread_num());
        for (j=0; j<i; j++)
          system("sleep 1");
      } /*-- End of parallel for --*/

-   Syntax is `schedule(kind [, chunk_size])`.

1.  schedule kinds could be:

    -   static
        Iterations are divided into chunks of size `chunk_size`, the `chunk_size` need not be a constant. It is the most efficient from a performance point of view, others have high overheads.
    -   dynamic
        The iterations are assigned to threads as the threads request them. The thread executes the chunk of iterations (controlled through the `chunk_size` parameter), then requests another chunk until there are no more chunks to work on.
    -   guided
        Similar to dynamic, see OpenMP standard for detail. (a little confusing)
    -   runtime, very convenient.
    -   The schedule is make at run times.
    -   Both the `dynamic` and `guided` schedules are useful for handling poorly balanced and unpredictable workloads. The difference between them is that with the `guided` schedule, the size of the chunk (of iterations) decreases over time.

## 4.6 OpenMP Synchronization Construct<a id="sec-1-6" name="sec-1-6"></a>

### 4.6.1 Barrier Construct<a id="sec-1-6-1" name="sec-1-6-1"></a>

A barrier is a point in the execution of a program where threads wiat for each other.

    #pragma omp parallel private(TID)
      {
        TID = omp_get_thread_num();
        if (TID < omp_get_num_threads()/2 ) system("sleep 3");
        (void) print_time(TID,"before");
    #pragma omp barrier
        (void) print_time(TID,"after ");
      } /*-- End of parallel region --*/

### 4.6.2 Ordered Construct<a id="sec-1-6-2" name="sec-1-6-2"></a>

It allows the execution of a structured block within a parallel loop in sequential order. Such as, to enforce an ordering on the printing of data computed by different threads.

It could als be used to help determine wheter there are any data races in the associated code.

### 4.6.3 Critical Construct<a id="sec-1-6-3" name="sec-1-6-3"></a>

It ensures that multiple threads do not attempt to update the same shared data simultaneously.
The associated code is referred to as a critical region/section.

    sum = 0;
    for (i=0; i<n; i++)
      sum += a[i];

The code loop sums up the elements of a vector a. A parallel version could be:
1.  let each thread independently add up a subset of the elements of the vector.
2.  store the result in a private variable.
3.  When all threads are doen, they add up their private contributions to get the sum.

    /*--pseudo parallel code for the summation--*/
    /*-- Executed by thread 0 --*/   
    sumLocal = 0;
    for (i=0; i<n/2; i++)
      sumLocal += a[i];
    sum += sumLocal;
    
    /*-- Executed by thread 1 --*/
    sumLocal = 0;
    for (i=n/2-1; i<n; i++)
      sumLocal += a[i];
    sum += sumLocal;

We must ensure that only one update of sum could take place at any time. Otherwise, it could cause **data race** problem. 

The below shows a simple implementation of a reduction operation:

    sum = 0;
    #pragma omp parallel shared(n,a,sum) private(TID,sumLocal)
    {
      TID = omp_get_thread_num();
      sumLocal = 0;
    #pragma omp for
      for (i=0; i<n; i++)
        sumLocal += a[i];
    #pragma omp critical (update_sum)
      {
        sum += sumLocal;
        printf("TID=%d: sumLocal=%d sum = %d\n",TID,sumLocal,sum);
      }
    } /*-- End of parallel region --*/
    printf("Value of sum after parallel region: %d\n",sum);

When the threads are finished with their part of the `for`-loop, they enter the critical region. By definition, only one thread at a time updates `sum`.

Another common situation where this construct is useful is when minima and maxima are formed:

    #pragma omp parallel private(ix, LScale, lssq, Temp) shared(Scale, ssq, x)
      {
    #pragma omp for
        for(ix = 1, ix<N, ix++)
          {
            LScale = ....;
    
    #pragma omp critical
            {
              if(Scale < LScale){
                ssq = (Scale/LScale) *ssq + lssq;
                Scale = LScale;
              }else
                ssq = ssq + (LScale / Scale) * Lssq
                  } /* End of critical region --*/
          }
      } /*-- End of parallel region --*/

### 4.6.4 Atomic Construct<a id="sec-1-6-4" name="sec-1-6-4"></a>

The atomic construct, which also enables multiple threads to update shared data without interference, can be an efficient alternative to the critical region.

In contrast to other constructs, it is applied only to the (single) assignment statement that immediately follows it; this statement must have a certain form in order for the construct to be valid, and thus its range of applicability is strictly limited.

    int ic, i, n;
    ic = 0;
    #pragma omp parallel shared(n,ic) private(i)
    for (i=0; i++, i<n)
      {
    #pragma omp atomic
        ic = ic + 1;
      }
    printf("counter = %d\n", ic);

The supported operations are: `+, *, -, /, &, ^, |, <<, >>`

Another example of using atomic:

    int ic, i, n;
    ic = 0;
    #pragma omp parallel shared(n,ic) private(i)
    for (i=0; i++, i<n)
      {
    #pragma omp atomic
        ic = ic + bigfunc();
      }
    printf("counter = %d\n", ic);

**Note** the `atomic` construct does not protect the execution of function `bigfunc`. It is only the update to the memory location of the variable `ic` that will occur atomically. If you do not want the function `bigfunc` to be executed in parallem among threads, use `critical` construct.

### 4.6.5 Locks<a id="sec-1-6-5" name="sec-1-6-5"></a>

-   *simple locks* which may not be locked if already  in a locked state, with type `omp_lock_t`.
-   *nestable locks* which may be locked multiple times by the same thread, with type `omp_nest_lock_t`.

The general procedure to use locks is:
1.  Define the lock variable.
2.  Initialize the lock via a call to `omp_init_lock`.
3.  Set the lock using `omp_set_lock` or `omp_test_lock`.
4.  Unset a lock after the work is done via a call to `omp_unset_lock`.
5.  Remove the lock associated via a call to `omp_destory_lock`.

### 4.6.6 Master Construct<a id="sec-1-6-6" name="sec-1-6-6"></a>

The `master` construct defines a block of code that is guaranteed to be executed by the master thread only.

It lacks the implied barrier on entry or exit.

    #pragma omp parallel shared(a,b) private(i)
     {
    #pragma omp master
       {
         a = 10;
         printf("Master construct is executed by thread %d\n",
                omp_get_thread_num());
       }
    #pragma omp barrier
    #pragma omp for
       for (i=0; i<n; i++)
         b[i] = a;
     } /*-- End of parallel region --*/
    printf("After the parallel region:\n");
    for (i=0; i<n; i++)
      printf("b[%d] = %d\n",i,b[i]);

Comparing with single construct:
The initailization of variable a is now guaranteed to be performed by the master thread.
And `barrier` needs to be inserted for correctness.

## 4.7 Interaction with the Execuation Environment<a id="sec-1-7" name="sec-1-7"></a>

There are variables that could be queried or modified in the OpenMP environment.

1.  Internal control variables. They could not be accessed or modified directly at application level, but could be queried and modified through OpenMP functions and environment variables.

    -   nthreads-var
    -   dyn-var
    -   nest-var
    -   run-sched-var
    -   def-sched-var
    
    To control those variable, `need` include `omp.h` header file:

2.  Control the number of threads in a parallel region by setting value `nthread-var`.

    -   `OMP_NUM_THREADS`, used at command-line
    -   `omp_set_num_threads`, used as `omp_set_num_threads(scalar-integer-expression)`.
    -   `num-threads`, used together with a prallel construct.
    -   `omp_get_max_threads()` routine, returns the largest number of threads available for the next parallel region.

3.  Optimize the use of system resources for thoughout by controling the value of `dyn-var`:

    -   `OMP_DYNAMIC(flay)`, flag could be `true` or `false`.
    -   `omp_set_dynamic`, adjusts the value of `dyn-var` at run time. `omp_get_dynamic` can be used to retrieve the current setting at run time.

4.  Set if the execution of parallel is nested or not via variable `nest-var`

    -   `OMP_NESTED`
    -   `omp_set_nested`

5.  Control the default schedule to be applied to parallel loops in a program

    -   its value decide the manner when schedule type is `runtime`.

6.  Other useful library routines:

    -   `omp_get_num_threads`, retrieve the number of threads in the current team
    -   `omp_get_thread_num`, returns the number of the calling thread as an integer value.
    -   `omp_get_num_procs`, returns an integer as the total number of processors available to the program at the instant in which it is called.
    -   `omp_in_parallel`, returns true if it is called from within an active parallel region.

## 4.8 More OpenMP Clauses<a id="sec-1-8" name="sec-1-8"></a>

### 4.8.1 If Clause<a id="sec-1-8-1" name="sec-1-8-1"></a>

Since some overheads incurred with the creation and termination of a parallel region, sush it is worth to use `if` cluase to specify whether there is enough work worth of doing so.

    #pragma omp parallel if (n > 5) default(none)   \
      private(TID) shared(n)
       {
         TID = omp_get_thread_num();
    #pragma omp single
         {
           printf("Value of n = %d\n",n);
           printf("Number of threads in parallel region: %d\n",
                  omp_get_num_threads());
         }
         printf("Print statement executed by thread %d\n",TID);
       }  /*-- End of parallel region --*/

-   if n > 5, the parallel region is executed by the number of threads available. Otherwise, one thread executes the region, so the region is then an *inactive* parallel region.
-   A `#pragma omp single` is used to avoid executing the first two print statements multiple times.

### 4.8.2 Num<sub>threads</sub> Clause<a id="sec-1-8-2" name="sec-1-8-2"></a>

The `num-threads` clause is supported on the `parallel` construct only and can be used to specify how many threads should be in the team executing the parallel region. 
Code shows the usage of `if` and `num-threads` cluases:

    #pragma omp parallel if (n > 5) num_threads(n) default(none)    \
      private(TID) shared(n)
       {
         TID = omp_get_thread_num();
    #pragma omp single
         {
           printf("Value of n = %d\n",n);
           printf("Number of threads in parallel region: %d\n",
                  omp_get_num_threads());
         }
         printf("Print statement executed by thread %d\n",TID);
       }  /*-- End of parallel region --*/

### 4.8.3 Ordered Clause<a id="sec-1-8-3" name="sec-1-8-3"></a>

It does not take any arguments and is supported on the loop construct only.

    #pragma omp parallel for default(none) ordered schedule(runtime) private(i,TID) shared(n,a,b)
    for (i=0; i<n; i++)
      {
        TID = omp_get_thread_num();
        printf("Thread %d updates a[%d]\n",TID,i);
        a[i] += i;
    #pragma omp ordered
        {printf("Thread %d prints value of a[%d] = %d\n",TID,i,a[i]);}
      }  /*-- End of parallel for --*/

### 4.8.4 Reduction Clause<a id="sec-1-8-4" name="sec-1-8-4"></a>

OpenMP provides the `reduction` clause for specifying some forms of recurrence calculations (involving mathematically associative and commutative operators) so that they can be performed in parallel without code modification.

    #pragma omp parallel for default(none) shared(n,a) reduction(+:sum)
    for (i=0; i<n; i++)
      sum += a[i];
    /*-- End of parallel reduction --*/
    printf("Value of sum after parallel region: %d\n",sum);

-   specify that `sum` will hold the result of a Reduction
-   identified via the `+` operator.

1.  Things need to be noticed when using with C++

    -   Aggregate types (including arrays), pointer types, and reference types are not supported.
    -   A reduction variable must not be const-qualified.
    -   The operator specified on the clause can not be overloaded with respect to the variables that appear in the clause.

### 4.8.5 Copying Clause<a id="sec-1-8-5" name="sec-1-8-5"></a>

### 4.8.6 Copyprivate Cluase<a id="sec-1-8-6" name="sec-1-8-6"></a>

## 4.9 Advanced OpenMP Construct<a id="sec-1-9" name="sec-1-9"></a>

### 4.9.1 Nested Parallelism<a id="sec-1-9-1" name="sec-1-9-1"></a>

### 4.9.2 Flush Directive<a id="sec-1-9-2" name="sec-1-9-2"></a>

Each thread executing an OpenMP code potentially has its own *temporary view* of the values of shared data. (*relaxed consistancy model*)

Sometimes update values of shared values must become visible to other threads in-between synchronization points.

### 4.9.3 Threadprivate Directive<a id="sec-1-9-3" name="sec-1-9-3"></a>

By default, global data is shared, which is often appropriate. But in some situations we may need, or would prefer to have, private data that persists throughout the computation. This is where the `threadprivate` directive comes in handy.

By default, the threadprivate copies are not allocated or defined. The programmer must take care of this task in the parallel region.

## Summary<a id="sec-1-10" name="sec-1-10"></a>

# Chapter05 How to Get Good Performance by Using OpenMP<a id="sec-2" name="sec-2"></a>