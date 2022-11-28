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
https://github.com/ucsd-cse15l-f22/list-methods-corrected which has the methods corrected (I would expect this to get full or near-to-full credit)
