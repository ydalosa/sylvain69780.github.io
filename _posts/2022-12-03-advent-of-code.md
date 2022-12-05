---
layout: default
title: "Advent of Code 2022"
tags: coding
---
# Starting
I love the minimalist design of this event.
I know about this annual event for coders through my nephew Cyrille. Not very confident at first, I had finished last year's puzzles a few months late.
Let try to do it using C# and Python
# Day 2
[here](https://adventofcode.com/2022/day/2)
I must code the rules of the game and decode the inputs
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
```

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
