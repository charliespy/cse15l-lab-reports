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






2>&1 : I/O Redirection
