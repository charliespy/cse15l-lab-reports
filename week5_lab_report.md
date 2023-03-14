# Welcome to Bash Grading Script Tutorial! 
Hi! In today's tutorial, we will go over how to create your own grading script using Bash and Junit! The full script is posted at the end, so feel free to jump there if you want, but we're going to start by breaking it down bit by bit. 

## 1. Importing and Cloning the Repository
The code for this section is the following: 
```bash
CPATH='".;../lib/hamcrest-core-1.3.jar;../lib/junit-4.13.2.jar"'
rm -rf student-submission
git clone $1 student-submission
echo 'Finished cloning'
```

The first line that starts with "CPATH" is where we import the Junit library necessary to run the grader. 

The second line, `rm -rf student-submission`, is removing the current student-submission directory so we can start fresh. This step is important so that other students' work don't interfere with the student we're currently grading. 

The third line, `git clone $1 student-submission`, means that we are cloning the url at the first command-line argument (`$1`) and naming the resulting repository as `student-submission`. Note that we are assuming that the first command-line argument is a valid GitHub url, because otherwise, the `git clone` command will fail and exit the script. 

The last line is a status update, printing a messsage to the terminal that we've finished cloning. 

## 2. Locating the student submission
The code for this section is the following: 
```bash
cd student-submission
if [[ -f ListExamples.java ]]
then
  echo "student submission found"
else
  echo "wrong file submitted"
  echo "You're grade is 0%, try again!"
  exit 1
fi
```

In this section, we are going to first `cd` to the student-submission directory, then try to find a file called `ListExamples.java`.

We will next use an if-statement. If we have found that `ListExamples.java` is a valid filepath, then we will print to the terminal that this step is completed. 

However, if we do not find `ListExamples.java`, then we will print to the terminal, notifing the student that they ahve submitted the wrong file, and their grade is 0%. Then, by using `exit 1`, we will exit the program so the rest of the script will *not* be executed. 

## 3. 

```bash
cp ../TestListExamples.java ./
javac -cp ".;../lib/hamcrest-core-1.3.jar;../lib/junit-4.13.2.jar" *.java 2>compiler-error.txt

if [[ $? == 0 ]]
then
  echo "successfully compiled"
else
  echo "compiler error"
  echo "You're grade is 0%, try again!"
  exit 1
fi

```









2>&1 : I/O Redirection
