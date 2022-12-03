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
https://adventofcode.com/2022/day/1
Here we have a groups of number separated with newlines and an empty line as a separator.
We need to find the sum of there groups of number and find the bigger.
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
