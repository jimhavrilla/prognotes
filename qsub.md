#In qsub:

##**_Always provide full paths especially for executables_**

####To import your current user environment use the -V option to import specific variables use -v

####-l is used to wrote most options for the queue itself like mem=20gb, walltime=48:00:00 and ncpus=8 but you must write -l in front of each parameter individually

<span style="font-family:Georgia;">-I</span> makes the queue interactive, -q indicates which priority queue you want to use standard or otherwise

####-W can be used to set numerous options like group\_list=whatevergroup or to submit multiple queues while making each subsequent dependent queue wait:

    afterany:jobid[:jobid...] implies that job may be scheduled for execution after jobs indicated by jobid have terminated, with or without errors.
    afterok:jobid[:jobid...] implies that job may be scheduled for execution only after jobs indicated by jobid have terminated with no errors.
    afternotok:jobid[:jobid...] implies that job may be scheduled for execution only after jobs indicated by jobid have terminated with errors.

> qsub yourscript.sh > id

> qsub -W depend=afterok:$(cat id) yournextscript.sh > id

> et cetera...



####-m indicates which things you would like the queue to mail to you b is before, e is error, a is after, and -M should be followed by your email address so it knows where to send it

####-N is used to give the job a name

####qstat -u username to show user's jobs

####qhold jobid to hold a job

####qdel jobid to kill a job

##[Great link for commands](http://hpc.sissa.it/pbs/pbs-2.html)

##[An amazing tutorial](https://wikis.nyu.edu/display/NYUHPC/Tutorial+-+Submitting+a+job+using+qsub)
