# Bash Scripts

## 📌 Script List (Click to Jump)

1. [Greatest of Three Numbers](#greatest-of-three-numbers)
2. [Even or Odd](#even-or-odd)
3. [Prime Check](#prime-check)
4. [Number or String](#number-or-string)
5. [Count Words & Characters in File](#count-words--characters-in-file)
6. [Average of n Numbers](#average-of-n-numbers)
7. [Fibonacci Series](#fibonacci-series)
8. [Factorial of a Number](#factorial-of-a-number)
9. [Sum of Digits](#sum-of-digits)
10. [Palindrome Check](#palindrome-check)

---

# CODE SCRIPTS (ALL IN ONE)

```bash
# ---------------------------------------------------------
# Greatest of Three Numbers
# ---------------------------------------------------------
echo "Enter first number:"
read a
echo "Enter second number:"
read b
echo "Enter third number:"
read c

if [ $a -ge $b ] && [ $a -ge $c ]; then
  echo "Greatest: $a"
elif [ $b -ge $a ] && [ $b -ge $c ]; then
  echo "Greatest: $b"
else
  echo "Greatest: $c"
fi

# ---------------------------------------------------------
# Even or Odd
# ---------------------------------------------------------
echo "Enter a number:"
read n

if [ $((n % 2)) -eq 0 ]; then
  echo "$n is Even"
else
  echo "$n is Odd"
fi

# ---------------------------------------------------------
# Prime Check
# ---------------------------------------------------------
echo "Enter a number:"
read n

if [ $n -le 1 ]; then
  echo "$n is not prime"
  exit
fi

flag=0
for ((i=2; i<n; i++))
do
  if [ $((n % i)) -eq 0 ]; then
    flag=1
    break
  fi
done

if [ $flag -eq 0 ]; then
  echo "$n is Prime"
else
  echo "$n is Not Prime"
fi

# ---------------------------------------------------------
# Number or String
# ---------------------------------------------------------
echo "Enter input:"
read input

if [[ $input =~ ^[0-9]+$ ]]; then
  echo "$input is a Number"
else
  echo "$input is a String"
fi

# ---------------------------------------------------------
# Count Words & Characters in File
# ---------------------------------------------------------
echo "Enter filename:"
read filename

if [[ -f $filename ]]; then
  while IFS= read -r line; do
    words=$(echo "$line" | wc -w)
    chars=$(echo "$line" | wc -c)
    echo "Line: $line"
    echo "Words: $words | Characters: $chars"
    echo "-----------------------------"
  done < "$filename"
else
  echo "File not found!"
fi

# ---------------------------------------------------------
# Average of n Numbers
# ---------------------------------------------------------
echo "Enter the count of numbers (n):"
read n

sum=0
echo "Enter $n numbers:"
for (( i=1; i<=n; i++ ))
do
  read num
  sum=$((sum + num))
done

average=$((sum / n))

echo "Sum = $sum"
echo "Average = $average"

# ---------------------------------------------------------
# Fibonacci Series
# ---------------------------------------------------------
echo "Enter the number of terms (n):"
read n

a=0
b=1

echo "Fibonacci series up to $n terms:"

for (( i=1; i<=n; i++ ))
do
  echo -n "$a "
  fn=$((a + b))
  a=$b
  b=$fn
done
echo

# ---------------------------------------------------------
# Factorial
# ---------------------------------------------------------
echo "Enter a number:"
read n

fact=1
for (( i=1; i<=n; i++ ))
do
  fact=$((fact * i))
done

echo "Factorial of $n is $fact"

# ---------------------------------------------------------
# Sum of Digits
# ---------------------------------------------------------
echo "Enter a number:"
read n

sum=0

while [ $n -gt 0 ]
do
  digit=$((n % 10))
  sum=$((sum + digit))
  n=$((n / 10))
done

echo "Sum of digits = $sum"

# ---------------------------------------------------------
# Palindrome Check
# ---------------------------------------------------------
echo "Enter a string:"
read str

rev=$(echo "$str" | rev)

if [ "$str" == "$rev" ]; then
  echo "$str is a Palindrome"
else
  echo "$str is NOT a Palindrome"
fi
