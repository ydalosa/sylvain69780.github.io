---
layout: default
title: "Advent of Code 2022"
tags: coding
---
# Starting
I love the minimalist design of this event.
I know about this annual event for coders through my nephew Cyrille. Not very confident at first, I had finished last year's puzzles a few months late.
Let try to do it using C# and Python
# Day 1

[Here](https://adventofcode.com/2022/day/1) we have a groups of number separated with newlines and an empty line as a separator.
We need to find the sum of these groups of number and find the bigger.
```csharp
void Day1()
{
    string[] lines = File.ReadAllLines("day1.txt");
    var l = new List<int>() { 0 };
    foreach (string line in lines)
        if (line == "")
            l.Add(0);
        else
            l[l.Count - 1] += int.Parse(line);
    Console.WriteLine(l.Max());
}

```
After the remark of my mentor Johann that this algorithm is "not terrible"
```csharp
static void Day1()
{
    var c = 0;
    var m = 0;
    foreach (string line in System.IO.File.ReadLines(@"day1.txt"))
        if (line == "")
        {
            if (c > m) m = c;
            c = 0;
        }
        else
            c += int.Parse(line);
    // last group of values
    if (c > m) m = c;
    Console.WriteLine(m);
}
```
To avoid redondant condition
```csharp
static void Day1()
{
    var c = 0;
    var m = 0;
    foreach (string line in System.IO.File.ReadLines(@"day1.txt"))
    {
        if (line == "") c = 0;
        else c += int.Parse(line);
        if (c > m) m = c;
    }
    Console.WriteLine(m);
}
```
Using enumerator
```csharp
static void Day1()
{
    var e = File.ReadLines(@"day1.txt").GetEnumerator();
    var last = !e.MoveNext();
    string line;
    var c = 0;
    var m = 0;
    while (!last)
    {
        line = e.Current;
        if (line == "") c = 0;
        else c += int.Parse(line);
        last = !e.MoveNext();
        if (c > m && (line == "" || last)) m = c;
    }
    Console.WriteLine(m);
}
```
Part 2 need to have top 3
```csharp
static void Day1p2()
{
    var c = 0;
    var l = new List<int>();
    foreach (string line in System.IO.File.ReadLines(@"day1.txt"))
        if (line == "")
        {
            l.Add(c);
            c = 0;
        }
        else
            c += int.Parse(line);
    l.Add(c);
    Console.WriteLine(l.OrderByDescending(x=>x).Take(3).Sum());
}
```
[String.Empty and "" produces exactly the same code](https://stackoverflow.com/questions/2905378/string-empty-versus)
>I've had numerous cases while quickly prototyping code, I littered "" all over the place, which later introduced many bugs, because **intent** was not clear: "Did I mean to say: pass empty string here, intentionally" or "Did I forgot to pass a non-empty string here

# Day 2
[here](https://adventofcode.com/2022/day/2)
I must code the rules of the game and decode the inputs
First version not terrible, second version better
```csharp
static void Day2()
{
    var input = System.IO.File.ReadLines(@"day2.txt");
    var codes = new Dictionary<string, string>
    {
        {"A","Rock" },
        {"B" ,"Paper"},
        {"C" , "Scissors"},
        {"X","Rock" },
        {"Y" ,"Paper"},
        {"Z" , "Scissors"}
    };
    var values = new List<string> { "Rock", "Paper", "Scissors"};
    var defeats = new List<string> { "RockScissors", "ScissorsPaper", "PaperRock" };
    var score = 0;
    foreach (var (opponent, you) in input.Select(x => x.Split(" ")).Select(x => (codes[x[0]], codes[x[1]])))
    {
        score += 1 + values.IndexOf(you);
        if (!defeats.Contains(opponent + you))
        {
            score += you == opponent ? 3 : 6;
        }
    }
    Console.WriteLine(score);
}

static void Day2()
{
    var input = System.IO.File.ReadLines(@"day2.txt");
 //   var action = new List<string> { "Rock", "Paper", "Scissors" };
    var defeats = new List<(int,int)> { (1,3), (3,2), (2,1) };
    var score = 0;
    foreach (var (opponent, you) in input.Select(x => x.Split(" ")).Select(x => ("ABC".IndexOf(x[0][0])+1,"XYZ".IndexOf(x[1][0])+1)))
    {
        score += you;
        if (!defeats.Contains((opponent,you)))
        {
            score += you == opponent ? 3 : 6;
        }
    }
    Console.WriteLine(score);
}

static void Day2p2()
{
    var input = System.IO.File.ReadLines(@"day2.txt");
    //   var action = new List<string> { "Rock", "Paper", "Scissors" };
    var score = 0;
    var defeats = new List<(int, int)> { (1, 3), (3, 2), (2, 1) };
    foreach (var (opponent, ending) in input.Select(x => x.Split(" ")).Select(x => ("ABC".IndexOf(x[0][0]) + 1, "XYZ".IndexOf(x[1][0]) + 1)))
    {
        var you = opponent;
        if (ending == 1)
            you = defeats.Find(x => x.Item1 == opponent).Item2;
        else if (ending == 3)
            you = defeats.Find(x => x.Item2 == opponent).Item1;
        score += you;
        score += (ending-1) * 3;
    }
    Console.WriteLine(score);
}

```

# Day 3

```csharp
static void Day3()
{
    var input = System.IO.File.ReadLines(@"day3.txt");
    var score = 0;
    foreach (var line in input)
    {
        var l = line.Length;
        var (a, b) = (line.Substring(0, l / 2), line.Substring(l / 2, l / 2));
        var r = (int)a.First(x => b.Contains(x));
        var p = r >= (int)'a' ? r - (int)'a' + 1 : r - (int)'A' + 27;
        score += p;
    }
    Console.WriteLine(score);
}

static void Day3p2()
{
    var input = System.IO.File.ReadAllLines(@"day3.txt");
    var score = 0;
    for (var i = 0; i < input.Length / 3; i++)
    {
        var (a, b, c) = (input[i * 3], input[i * 3 + 1], input[i * 3 + 2]);
        var r = (int)a.First(x => b.Contains(x) && c.Contains(x));
        var p = r >= (int)'a' ? r - (int)'a' + 1 : r - (int)'A' + 27;
        score += p;
    }
    Console.WriteLine(score);
}
```

 # Day 4
Coding some intervals
```csharp
static void Day4()
{
    var input = System.IO.File.ReadAllLines(@"day4.txt");
    var r = (string x) => { var r = x.Split("-"); return (start: int.Parse(r[0]), end: int.Parse(r[1])); };
    var score = 0;
    foreach ( var (a,b) in input.Select(x => x.Split(",")).Select(x => (r(x[0]), r(x[1]))) )
    {
        if ((b.start >= a.start && b.end <= a.end) || (a.start >= b.start && a.end <= b.end))
            score++;
    }
    Console.WriteLine(score);
}
static void Day4p2()
{
    var input = System.IO.File.ReadAllLines(@"day4.txt");
    var r = (string x) => { var r = x.Split("-"); return (start: int.Parse(r[0]), end: int.Parse(r[1])); };
    var score = 0;
    foreach (var (a, b) in input.Select(x => x.Split(",")).Select(x => (r(x[0]), r(x[1]))))
    {
        if ( ( a.start <= b.start && b.start <= a.end) || (b.start <= a.start && a.start <= b.end))
            score++;
    }
    Console.WriteLine(score);
}
```
# Day 5
Stacks 
But better if I just used lists instead ?
```csharp
static void Day5()
{
    var input = System.IO.File.ReadLines(@"day5.txt").GetEnumerator();
    input.MoveNext();
    var w = 1 + (System.IO.File.ReadLines(@"day5.txt").First().Length) / 4;
    var stacks = new Stack<char>[w];
    foreach (var i in Enumerable.Range(0, w))
        stacks[i] = new Stack<char>();
    while (input.Current[1] != '1' )
    {
        foreach (var i in Enumerable.Range(0, w))
            if (input.Current[i*4+1] != ' ') 
                stacks[i].Push(input.Current[i * 4 + 1]);
        input.MoveNext();
    }
    input.MoveNext();
    // reversing the stacks because of read order of the input :-(
    foreach (var i in Enumerable.Range(0, w))
    {
        var l = new Stack<char>();
        foreach (var c in stacks[i])
            l.Push(c);
        stacks[i] = l;
    }
    while (input.MoveNext())
    {
        Console.WriteLine(input.Current);
        var i = input.Current.Replace("move ", "").Replace(" from ", ",").Replace(" to ", ",").Split(',').Select(x => int.Parse(x)).ToList();
        var (amount, origin, destination) = (i[0], i[1], i[2]);
        foreach (var c in Enumerable.Range(1,amount))
        {
            stacks[destination-1].Push(stacks[origin-1].Pop());
            Console.WriteLine(string.Join("", stacks.Select(x => x.FirstOrDefault(' '))));
        }
    }
}
static void Day5p2()
{
    var input = System.IO.File.ReadLines(@"day5.txt").GetEnumerator();
    input.MoveNext();
    var w = 1 + (System.IO.File.ReadLines(@"day5.txt").First().Length) / 4;
    var stacks = new Stack<char>[w];
    foreach (var i in Enumerable.Range(0, w))
        stacks[i] = new Stack<char>();
    while (input.Current[1] != '1')
    {
        foreach (var i in Enumerable.Range(0, w))
            if (input.Current[i * 4 + 1] != ' ')
                stacks[i].Push(input.Current[i * 4 + 1]);
        input.MoveNext();
    }
    input.MoveNext();
    // reversing the stacks because of read order of the input :-(
    foreach (var i in Enumerable.Range(0, w))
    {
        var l = new Stack<char>();
        foreach (var c in stacks[i])
            l.Push(c);
        stacks[i] = l;
    }
    while (input.MoveNext())
    {
        Console.WriteLine(input.Current);
        var i = input.Current.Replace("move ", "").Replace(" from ", ",").Replace(" to ", ",").Split(',').Select(x => int.Parse(x)).ToList();
        var (amount, origin, destination) = (i[0], i[1], i[2]);
        var mover = new Stack<char>();
        foreach (var c in Enumerable.Range(1, amount))
            mover.Push(stacks[origin - 1].Pop());
        foreach (var c in mover)
            stacks[destination - 1].Push(c);
        Console.WriteLine(string.Join("", stacks.Select(x => x.FirstOrDefault(' '))));
    }
    Console.WriteLine("END "+string.Join("", stacks.Select(x => x.FirstOrDefault(' '))));
}
```
# Day 6
Spend less time on this one to process the input, so less pain !
```csharp
static void Day6()
{
    var input = System.IO.File.ReadLines(@"day6.txt").First();
    var marker = new Queue<char>();
    var pos = 1;
    foreach (var c in input)
    {
        if (marker.Count == 4) marker.Dequeue();
        marker.Enqueue(c);
        if (marker.Count == 4 && marker.GroupBy(x => x).Select(y => y.Count()).Max() == 1) break;
        pos++;
    }
    Console.WriteLine("Restult = " + pos);
}
static void Day6p2()
{
    var input = System.IO.File.ReadLines(@"day6.txt").First();
    var marker = new Queue<char>();
    var pos = 1;
    foreach (var c in input)
    {
        if (marker.Count == 14) marker.Dequeue();
        marker.Enqueue(c);
        if (marker.Count == 14 && marker.GroupBy(x => x).Select(y => y.Count()).Max() == 1) break;
        pos++;
    }
    Console.WriteLine("Restult = " + pos);
}
```
# Day 7
```csharp

```