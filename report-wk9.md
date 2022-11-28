# Lab Report Week 9

## grade.sh
```

set -e

echo "Grading the submitted github URL: $1"
echo ""

rm -rf student-submission
git clone $1 student-submission 

file = "ListExamples.java"
class = "class ListExamples"

if [[ $? -eq 0 ]]
then
  echo "Cloned successfully."
else
  echo "Clone failed."
  exit 1
fi

cd student-submission

if [[ -f $file ]]
then
  echo "Check file successfully! ($file)"
else
  echo "Fail to find ($file)"
  exit 1
fi

grep -q "$class" ./$file

if [[ $? -eq 0 ]]
then
  echo "Your file has the right class"
else
  echo "Fail to find the right class!"
  exit 1
fi

cd ..
cp TestListExamples.java student-submission/TestListExamples.java
cp -r lib/ student-submission/lib
cd student-submission

set +e

javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java 2> error.txt

if [[ $? -eq 0 ]]
then
  echo "Compile Successfully!"
else
  echo "Compile error!"
  exit 1
fi

java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore TestListExamples > test_result.txt

if grep "OK" test_result.txt &>/dev/null
    then
        echo "All Tests Passed!"
    else
      grep "Tests run" test_result.txt
    echo "Failed : "
    grep ") " test_result.txt
fi
```
## Three student submissions
Example 1: https://github.com/ucsd-cse15l-f22/list-methods-corrected which has the methods corrected (I would expect this to get full or near-to-full credit)
![image](https://github.com/YuxuanIsL/lab-report-week-9/blob/main/corrected.png)

Example 2: https://github.com/ucsd-cse15l-f22/list-methods-compile-error, which has a syntax error of a missing semicolon.
![image](https://github.com/YuxuanIsL/lab-report-week-9/blob/main/compile-error.png)

Example 3: https://github.com/ucsd-cse15l-f22/list-methods-filename, which has a great implementation saved in a file with the wrong name.
![image](https://github.com/YuxuanIsL/lab-report-week-9/blob/main/filename.png)

## Trace script in Example 3

`set -e` stops the execution of a script if a command or pipeline has an error - which is the opposite of the default shell behaviour, which is to ignore errors in scripts. No stdout or stderr. Exit code 0.

`echo "Grading the submitted github URL: $1"` `echo""` No stdout or stderr. Exit code 0.

`rm -rf student-submission` removes student-submission directory from the current directory. No stdout or stderr. Exit code 0.

`git clone $1 student-submission ` clones student-submission reporsitory to local computer. No stdout or stderr. Exit code 0.

`file = "ListExamples.java"` `class = "class ListExamples"` create variables. No stdout or stderr. Exit code 0.
 
`if [[ $? -eq 0 ]]` checks if the reporsitory is successfully cloned. In this case, the URL gives correct git reporsitory. The statement is true. It continues on the first `then`. Exit code 0.

`echo "Cloned successfully."` returns the statement. Exit code 0.

`else
  echo "Clone failed."
  exit 1
fi` not executed. 

`cd student-submission` No stdout or stderr. Exit code 0.

`if [[ -f $file ]]` checks if there is `file` which is `ListExamples.java` in the reporsitory. In this case, there is no ListExample.java since the student stores it to a file with wrong name. The statement is false. It gose to the first `else`.

`echo "Fail to find ($file)"` No stdout or stderr. Exit code 0.

 `exit 1` No stdout or stderr. Exit code 0.

`fi` ends `if`. No stdout or stderr. Exit code 0.
