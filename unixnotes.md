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

top will show processes top ones first and can be done by user

"nohup command &" will not only run a command in the background but keep running it even if the terminal is closed!

on mac: sudo port -f deactivate will deactivate an active port

to rename files in bulk:
rename .sorted.bam.bai .sorted.markdup.bam.bai *.bai

to uniq by column:
for the first 3 columns to make a key field...
sort -u -k1,3

if you want to copy all files except a few (and if the transfer gets interrupted it can be easily restarted) use rsync like so (a is recursive P is show progress):
rsync -aP --exclude==AUTHORS --exclude==COPYING --exclude==README * /usr/local/bin

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
18. can also do git commit -a if too lazy to remember all changes
19. git reset --hard HEAD^ to undo last commit or HEAD~2 to go back 2 commits, etc.
20. if you want to auto-pull from remote origin and master branch do git branch --set-upstream master origin/master
21. if you add some stuff to your git repo and don't want to always specify where you want to push and pull if you're just using remote origin and branch master...

Under [branch "master"], try adding the following to the repo's Git config file (.git/config):

[branch "master"]

        remote = origin
        merge = refs/heads/master

This tells Git 2 things:

    When you're on the master branch, the default remote is origin.
    When using git pull on the master branch, with no remote and branch specified, use the default remote (origin) and merge in the changes from the master branch.

grep ^pattern file (beginning of line); grep pattern$ file (end of line) grep 'exactmatch' file; grep -v 'ignorepattern' file; grep -h 'suppressfromfileoutputfrommultiplefiles' filelist

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

if you want to move a word around a string in python:
def move_word(s, word, pos):
   split = s.split()
   split.insert(pos, split.pop(split.index(word)))
   return ' '.join(split)

to use awk to recognize separators and output them (and swap values):
awk -F$'\t' '{OFS="\t"} {t=$2;$2=$4;$4=t;t=$3;$3=$5;$5=t;print;}'
awk -F$'\t' '{OFS="\t"} {if (($7 == "-") && ($2 > $3)) {t=$2; $2=$3; $3=t;} print $0}'

in bedtools with awk (average of reads per chromosome and the average of averages)
cat SRR1006449_500kb_SORT.cov | bedtools groupby -g 1 -c 6 -o mean | awk '{ sum += $2; count +=1 } END { print sum/count}' | less

cat -et to show special characters

sort -c can check if input is properly sorted

can use tophat or tophat2 with bowtie2 indexes to do mapping and alignment including splices for RNA sequences; tophat2 can specify cores used and memory used

use awk to print to certain decimal places
e.g. cat less.txt | awk '{printf "%s %.2f %s\n", $1, $2, $3}'

sudo port search or install for macports

ssh -p port user@mysite.com or user@127.0.0.1 if you need to use a port in ssh