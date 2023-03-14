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


## 3. Compiling the Submission
The code for this section is the following: 
```bash
cp ../TestListExamples.java ./
javac -cp ".;../lib/hamcrest-core-1.3.jar;../lib/junit-4.13.2.jar" *.java 2 > compiler-error.txt

if [[ $? == 0 ]]
then
  echo "successfully compiled"
else
  echo "compiler error!!!"
  cat compiler-error.txt
  echo "You're grade is 0%, try again!"
  exit 1
fi
```

In the first line, by saying `cp ../TestListExamples.java ./`, we are copying the student code file and our test files in the same directory. 

Then, by saying `javac -cp ".;../lib/hamcrest-core-1.3.jar;../lib/junit-4.13.2.jar" *.java`, we are compiling the test file. 

In the same line right after this, by saying `2 > compiler-error.txt`, we are using a technique called "I/O redirection" to store any potential compile errors into a file called `compiler-error.txt`. The number 2 here is a file descriptor representing `stderr`. This allows us to stop the error, which would stop the file directly, and print our own corresponding messages. 

Then, we are going to use an if-statement to print corresponding messages if there is or isn't an error during the compiling process. 


## 4. Running the Tests and Reporting the Grade
After we've compiled the code, we will now run the tester! The code for this section is the following:
```bash
java -cp ".;../lib/hamcrest-core-1.3.jar;../lib/junit-4.13.2.jar" org.junit.runner.JUnitCore TestListExamples > results.txt 2>&1

if [[ $? == 0 ]]
then
  echo "tests passed"
  cat results.txt
  echo "You're grade is 100%, congratulations!"

else
  echo "tests failed"
  cat results.txt

  num_tests=$(grep 'Tests run:' results.txt | sed 's/^.*: //')
  failures=$(grep 'Failures:' results.txt | sed 's/^.*: //')
  num_tests=$(($num_tests))
  failures=$(($failures))
  successes=$(($num_tests - $failures))
  
  echo "You're grade is:" $(($successes / $num_tests))"% Try again!"
  exit 1
fi
```

In the first line, we are using Junit to test the student implementations. At the end of the line, by typing `>results.txt 2>&1`, we are storing the output (successfull or error) to a file called `results.txt`. 

Then, we are using an if-statement to say that if there's no error, it means that all the tests have passed. In this case, we will print the result and say that the student has received a grade of 100% (yay!).

However, if there is an error (meaning that one or more tests have failed), then we will first print the error message results, then calculate the grade for the student. 

We will use the commands `num_tests=$(grep 'Tests run:' results.txt | sed 's/^.*: //')` and `failures=$(grep 'Failures:' results.txt | sed 's/^.*: //')` to extract the information we need to calculate students' grade. This `grep` command will return the test numbers in `results.txt`. 









