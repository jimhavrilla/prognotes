In qsub:

Always provide full paths especially for executables

to import your current user environment use the -V option to import specific variables use -v

most options for the queue itself like mem=20gb, walltime=48:00:00 and ncpus=8 are set after writing -l in front of each parameter

-I makes the queue interactive, -q indicates which priority queue you want to use standard or other

-W can be used to set numerous options like group_list=whatevergroup or to submit multiple queues while making each subsequent dependent queue wait:
qsub yourscript.sh > id
qsub -W depend=afterok:$(cat id) yournextscript.sh > id
et cetera...

-m indicates which things you would like the queue to mail to you b is before e is error a is after and -M should be followed by your email address so it knows where to send it

-N can give the job a name and -W indi

qstat -u username to show user's jobs

qdel jobid to kill a job

good link for commands http://hpc.sissa.it/pbs/pbs-2.html