##Random Unix Notes

ctrl+a to go to beginning of line, ctrl+e to end of line, alt then click to go anywhere

"screen" use Ctrl+a " to access screens, Ctrl+a c to create a new screen and Ctrl+a A to name it Ctrl+a Ctrl+a to cycle
screen recovery: screen -d -r
"ln -s" (symbolic link function creates pointer to file that acts like readonly file)
"source .bashrc" to set the new bashrc or edit the bashrc to change startup settings
"ssh -Y or -X" is X11 forwarding for remote displays (use firefox or chrome or xeyes to test display)
"^" to substitute/edit last command (e.g. ls -la; ^-ls^-l)
"cd -" to go to last directory
history to get full history
~ is user root
/ is root
ls -d gives you file path (in echo form with file name), -h gives biggest byte size like GB or MB, can also use --block-size=M etc.

to uniq by column:
for the first 3 columns to make a key field...
sort -u -k1,3

find . -type f -name "*1*" -exec grep Volume {} \;  # cool way to look for a certain group of files, then execute a command on the results; the {} can be used to execute the command on a specific file or if empty all files.  the \; terminates output because otherwise you could keep typing commands

###git
1. git init to create a new local repository
2. git config -- global color.ui "auto" #adds colors
3. git config -- global core.editor "nano" # changes the editor to nano
4. git config -- global core.autocrlf "input" # for windows just write true
5. git add blahfile.txt adds file to "staging area" (-A adds all files); then do git commit -m "message" to finalize the
change locally
6. git status to check the state
7. git log shows you what has been done
8. git branch shows branches, git branchname creates a new one; 
9. git diff shows you what is different between what has already been committed and what hasn't been even line by line...awesome
10. git checkout -- specificfile.txt will allow you to recover your unedited version or previous versions of files also git checkout branch can be used to switch branches which ACTUALLY have DIFFERENT local files
11. git merge branch will merge that branch to the current branch; if the two files are the same name but different it will create a conflict unless it is simply adding a line or something like that.  so you edit the file and then add and commit it and then merge in the other branch and you're done 
12. git log --oneline puts changes on oneline --graph makes a little line graph that shows the connection between commits and branches
13. git ls-tree -r master shows you all files in that branch
14. if you fork the git repository online from someone else to yours you can edit a copy separately without worrying about conflict
15. git remote add origin https://github.com/jimhavrilla/general.git to add a remote repository origin; git remote to see what remote repositories are linked, like origin (and there can be a different one for fetch/pull and push); git push origin master will make the changes to the master branch remotely
16. if you go on github you can add collaborators to your repository so they can help you with the coding and push changes
17. git pull will update stuff from github to your local copy of the repository

** find . -type f -exec mv {}.bam {}.txt # to change all bams to .txt files...awesome

** tr is a great command...e.g. tr -s " " will collapse all extra spaces into one space

** fuser file tells you a PID associated with that file.  if you do fuser -k file you can kill that process!  awesome!  (you can also do kill -signal number PID)

****parallel computing
for example, for the sra archive unpackaging:
find *.sra | parallel -j 7 --dry-run "fastq-dump {}" (dry run just echos commands so remove -dry-run later)

to parallelize an already parallel command or any command:
for example say you want to parallelize parallel alignments.  find the list you want to parallelize, pipe into xargs -i then echo "command {} rest of command" (the curly brackets is where you pipe the members of the list through x args) and then pipe THAT through qsub with say 8 cores to run 8 of them at a time

in python, to run unix use:
import sys
call("string with unix code",shell=True)
in python, to make argument options with no particular order:
import argparse
parser = argparse.ArgumentParser(description="blah")
parser.add_argument('-o','--option',help='this option does this') (if you type the argument action="store_true" and no values then it means if you put the option do this otherwise don't)
args=parser.parse_args()
optionarg = args.option

in bedtools with awk (average of reads per chromosome and the average of averages)
cat SRR1006449_500kb_SORT.cov | bedtools groupby -g 1 -c 6 -o mean | awk '{ sum += $2; count +=1 } END { print sum/count}' | less

use awk to print to certain decimal places
e.g. cat less.txt | awk '{printf "%s %.2f %s\n", $1, $2, $3}'

port search or install for macports

\** is math.pow in python...it does 2 ** 3 = 8 like so

numpy.arange(start,end,increment) can make a range of values (end not inclusive)
numpy.linspace(start,end,num=numofsteps) can make a range of values too, even n-dimensionally, end not inclusive

also in python: define functions like ---
def stupidfunc(x,y):
	"""this is dumb""" # a docstring which you can call and learn about your function using function.\_\_doc\_\_
	return x+y

** in ipython pressing Tab after object. gives you a list of fields or methods to choose from
*** a python package called pandas utilizes quick data frame operations R-style but faster...can use data.columns, and a read_csv function to get the data already formatted...can also call column like data.column or data[["column","column2"]]; 
*** you can run methods on columns in pandas like s.td() standard deviation or .plot() or .hist() (histogram and you can modify the graph too) or .head() or .tail() to get the first or last n items exactly like in unix and you can combine them or filter like data = data[(data.blahvalue <= some crap) & (data.crapvalue <= some blah)], which you can already do in python, or .describe() to get basic descriptive stats
*** you can also change column names as long as you replace it with a list like data.columns = ['new', 'names', 'in', 'order']
*****there is a datetime module in python and a function strptime that can strip the characters from a date/time type object so you can assign the dates to the data.index values and then use data.ix[datetime(blah,blah,blah)] and access information by date; then you can drop the date column by typing data.drop("date",axis=1) 0 for rows
*** in pandas can access data rows at once like data.clouds[datetime(2012,1,1):datetime(2012,1,31)].str.contains("whatever")...pretty cool, also has a function isnull() to detect NaN and Nonetype data and pandas will shove in NaN automatically in blank cells and you can use fillna("") to instead fill columns with empty strings or the like
*** accessing individual rows is possible in pandas with irow(1) or whatever number; data.iterrows() will give you idx and row which is cool because you can do for idx,row in data.iterrows() (iterrows takes away the dot syntax and requires the dictionary syntax though): and you can go further embedding an if statement like if "blah" in row["this stuff"]:
*** can use data.apply(function) to apply a function to a DataFrame in pandas
*** in pandas you can use data.groupby("column") to group your data by a value, to let you iterate through a group and pass data to excel sheets or csvs or make new columns in your data frame

*** pandas has str functions like contains() or isin() that looks for strings in string columns

also in python: subprocess can call unix commands and store output in python. if 
find -name finds a name can use * same as in unix ls stuff.  also can do type -f to only look for files and d for directories
also in python:
	ct=0
	while True:
		try:
			code
		except Exception:
			ct=ct+1
			continue
		else:
			store variable[ct]
			break

touch to "change date modified in unix"

for x in $(seq 1 to n) unix for loops
