# MapReduce and Spark

## Examples

Assume that we have a file PersonInfo.txt which contains info about name, age and salary in the following format:

Kari 45 450000\
Ola 30 200000\
Kate 30 500000\
Pål 45 550000

We wish to find average income for each age, like this (the result does not have to be sorted):

45 500000\
30 350000

**a) Show with pseudo code for mapper and reducer how this can be done in MapReduce. Use the following as a starting point:**

**public void map(key(name), value(age,salary))\
public void reduce(key, Iterable values)**

```java
public void map(key(name), value(age, salary)) {
  // We don't need name.
  // We write age as the first value in the tuple to get it as a key
  // Salary is the value which will be reduced based on the key, so we place it
  // as the second value in the tuple.
  write(age, salary)
}
```

```java
public void reduce(key, Iterable values) {
  valueSum = 0;
  values.forEach(value => {
    valueSum += value;
  });
  averageValue = valueSum / values.length;
  write(key, value);
}
```

**b) Next, we wish to find maximum salary for each age using Spark. Assume that we already have the file read into an RDD of with pair (key,value)=(age,salary), i.e. RDD[(int, int)]. Show what transformations are needed to get a resulting RDD where (key, value)=(age, maxSalary).**

Let's assume that the RDD is stored in the variable `tuples`.

```python
tuples.reduceByKey(lambda a, b: max(a, b))
```
