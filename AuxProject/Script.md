#!/bin/bash

# Task 1: Ask the user for their name and age, and output a message with their name and the year they were born.

echo "Please enter your name:"
read name
echo "Please enter your age:"
read age
year=$(($(date +%Y) - $age))
echo "Hello, $name! You were born in $year."

# Task 2: Create a new directory with a name provided by the user, and navigate into it.
echo "Please enter the name of the directory you want to create:"
read dir_name
mkdir $dir_name
cd $dir_name

# Task 3: List all files in the current directory, sorted by file size.
ls -lS

# Task 4: Count the number of files in the current directory and output the result.
file_count=$(ls | wc -l)
echo "There are $file_count files in this directory."

# Task 5: Take a list of numbers as input from the user and output the sum of those numbers.
echo "Please enter a list of numbers separated by spaces:"
read numbers
sum=0
for num in $numbers
do
  sum=$(($sum + $num))
done
echo "The sum of the numbers is: $sum"

# Task 6: Output a random number between 1 and 100.
echo "Your random number is: $(( ( RANDOM % 100 ) + 1 ))"

# Task 7: Create a backup of a specified file by copying it to a backup directory with a timestamp in the filename

echo "Please enter the full path of the file you want to backup:"
read file_path
timestamp=$(date +%Y-%m-%d_%H-%M-%S)
mkdir -p /home/ubuntu/Shell/backup
touch /home/ubuntu/Shell/backup/"$(basename $file_path)"_"$timestamp"
cp $file_path /home/ubuntu/Shell/backup/"$(basename $file_path)"_"$timestamp"

# Task 8: Check if a website is online and output a message indicating whether it is up or down.
echo "Please enter the URL of the website you want to check:"
read url
if curl --output /dev/null --silent --head --fail "$url"; then
  echo "$url is up."
else
  echo "$url is down."
fi

# Task 9: Convert a temperature in Celsius to Fahrenheit, using input from the user.
echo "Please enter a temperature in Celsius:"
read celsius
fahrenheit=$(echo "scale=2; ((9/5) * $celsius) + 32" | bc)
echo "$celsius degrees Celsius is equal to $fahrenheit degrees Fahrenheit."

# Task 10: Ask the user for a sentence, then output the sentence in reverse order.
echo "Please enter a sentence:"
read sentence
reverse=""
for (( i=${#sentence}-1; i>=0; i-- ))
do
  reverse="$reverse${sentence:$i:1}"
done
echo "Your sentence in reverse is: $reverse"

