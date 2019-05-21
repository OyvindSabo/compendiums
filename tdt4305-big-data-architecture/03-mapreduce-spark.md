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

## More examples
Assume a file countries.tsv (tsv = tab separated) which contains information about countries and
characteristic terms for these countries (in this case, you can assume one tab "\t" per line,
between country and the term list):

France    wine,art,trains,wine,trains
Canada    glaciers,lakes,hockey,maple,grizzly
Norway    fjords,fjords,glaciers,trolls
Japan     trains,sushi,origami,sushi,fuji
Argentina wine,glaciers,football,glaciers

You are now, for each of the problems below, to show how they can be solved using Spark
transformations /actions (Scala, Python, or Java).
Hint: Split a text string x based on "\t" (tab): val y = x.split("\t")

**a) Create an RDD with name ”data” based on the file, where each record/object is a text string (String) containing one line from the file.**
```python
data = sc.textFile("countries.tsv")
```

**b) Create a new RDD ”data2” where each object in ”data” is an ”array/list of strings”, where the
first text string is country and text string number two contains the terms. Example result (Py-
thon):
[['France', 'wine,art,trains,wine,trains'],
['Canada', 'glaciers,lakes,hockey,maple,grizzly'],
['Norway', 'fjords,fjords,glaciers,trolls'],
['Japan', 'trains,sushi,origami,sushi,fuji'],
['Argentina', 'wine,glaciers,football,glaciers']]**
```python
data2 = sc.textFile("countries.tsv").map(lambda line: line.split("\t"))
```


**c) Create a new pair RDD ”data3” where (key is country, and value is an “array/list of strings”
of characteristic terms, example result:
[('France', ['wine', 'art', 'trains', 'wine', 'trains']),
('Canada', ['glaciers', 'lakes', 'hockey', 'maple', 'grizzly']),
('Norway', ['fjords', 'fjords', 'glaciers', 'trolls']),
('Japan', ['trains', 'sushi', 'origami', 'sushi', 'fuji']),
('Argentina', ['wine', 'glaciers', 'football', 'glaciers'])]**
```python
data3 = sc.textFile("countries.tsv").map(lambda line: line.split("\t")).map(lambda line: (line[0], line[1].split(",")))
```


**d) Find number of characteristic terms in the dataset (not including name of the countries).**
```python
data4 = sc.textFile("countries.tsv).map(lambda line: line.split("\t")).map(lambda line: (line[0], line[1].split(","))).flatMapValues(lambda value: value).count()
```

**e) Find the number of distinct characteristic terms in the dataset.**
```python
data5 = sc.textFile("countries.tsv).map(lambda line: line.split("\t")).map(lambda line: (line[0], line[1].split(","))).flatMapValues(lambda value: value).distinct().count()
```

**f) Based on ”data3”, create an RDD that contains number of distinct terms for each country.
Example result:
[('Argentina', 3), ('Norway', 3), ('France', 3), ('Canada', 5), ('Ja
pan', 4)]**
```python
data3.map(lambda line: (line[0], len(list(dict.fromkeys(line[1])))))
```

## Even more examples
For each song streamed, a line will be generated in the log, in the following format (comma separated): TimeStamp,UserName,Artist,SongName

Assume the following example dataset stored in the file streamed.csv:\
1,u1,a1,s1\
2,u2,a1,s2\
3,u1,a1,s2\
4,u3,a1,s1\
5,u1,a2,s3\
6,u2,a1,s2\
7,u2,a1,s2\
8,u2,a1,s2\

This dataset has already been loaded into an RDD named s:
```python
val s = sc.textFile("streamed.csv").map(_.split(","))
```

**Create an RDD containing number of songs streamed for each artist. Example results:**\
(a2,1)\
(a1,7)

```python
s.map(lambda line: line[2]).reduceByKey(lambda a, b: a + b)
```

**Create an RDD that for each line has name of user and number of song he/she has streamed. Example results:**\
u3 1\
u2 4\
u1 3
```python
s.map(lambda line: line[1]).reduceByKey(lambda a, b: a + b)
```

**Find number of distinct songs that have been played. Example results:**\
3
```
s.flatMapValues(lambda line: line[3]).distinct().count()
```
