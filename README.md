# Programming Fundamentals 2 - Lab Session 11

This time you'll learn how to sort objects using different sorting criteria. To sort objects it's necessary to answer the question: given two objects, which one is smaller. In Swift terminology, one must implement a function "<(object1, object2)" that takes two objects and outputs true if the first entry is "smaller" then the second. 

You will create a new class Song. There is no obvious way to define when one song is smaller than the other. You can compare them by duration, alphabetically, etc. You will define one "default" way to compare two songs and will learn how to create and use other ways of comparison.

Revisiting these examples will help you to complete the assignment
https://github.com/coast-uni-lu/pf2-2020-course/blob/master/07-Operators-and-Standard-Protocols/

## Exercise 1 - Equality and Comparison with a Music Player

### Music Player (Playlist)
A music player is a playlist with songs. A playlist is a countable entity that **can not** be shared among different users. The playlist has: name, list of songs, play counter, total duration. No one can access the list of songs. But. It is possible to add songs to the list and play them one after another. Each time the playlist is played, the counter of the playlist is increased. The counter describes the number of times a playlist has been played (playlist is a countable entity). The duration of the playlist the sum of the songs' durations it contains.

### Duration
The duration is a comparable entity and represents a time duration in seconds. However, in some cases duration must be formatted [MM:SS]. Each pair of duration objects can be compared in an obvious way (by duration in seconds).

### Song
The song is a countable and comparable entity. The song has: title, artist, duration, counter. The counter indicates how many times a song has been played: the counter is increased each time song is played. User sees currently played song in the format: TITLE by ARTIST [DURATION], e.g. 'Chosen Family' by Rina Sawayama [04:09]'.

Songs are also comparable entities. Two songs are equal if their title, artist, and duration are the same. Songs also can be sorted. The natural order of sorting is defined below in decreased priority.
1. title (alphabetically)
2. artist (alphabetically)
3. duration (numerically)

### Tasks
- complete UML diagram
- define Countable protocol
- implement the Song and song's ```play``` method
- implement methods to compare two songs and to tell whether songs are the same (see **Song** description above)
- implement the Playlist
- implement add(song: Song) method that adds a song to the list. Playlist can't have the same song twice. If the song is already on the list (use ```.contains``` to check for that), the user gets a notification. 
- implement play() method that plays songs in the playlist one by one. Increase the counter each time the playlist is played. Output in the console the song currently playing and its duration (nicely formatted, see below)
Write the following test cases:
- in your **main.swift** file Create 2 objects of type Duration and compare them.
- create the array with 3 different songs. Sort it and print the sorted array. Check that ```.contains``` function works properly for your array.
- add some songs to the playlist. Continue by sorting the list of songs of the playlist. Print out the songs and analyze the outputs. Are the songs in the right order?

### Hints :
- you may use the following code to sort and print the songs of the array.
```swift
 songs.sorted().forEach {print($0)}
 ```
- google how to use String(format: ...) to format the duration (you could also be fine without it)
- use ```print(’\(self)’)``` for printing the description of a class. See the section on CustomStringConvertible below.
CustomStringConvertible protocol to create a String representation for a Song. CustomStringConvertible is the protocol from the Swift library, it forces you to implement description field.

```swift
class MyClass: CustomStringConvertible {
var description: String {
	return "sTrInG_RePrEsEnTaTi0n"
	}	
//your class implementation
} 
var x = MyClass()
print("str representation of my object is \(x)")
//outputs 
//str representation of my object is sTrInG_RePrEsEnTaTi0n
```

## Exercise 2 - Other Sortingtype for Songs

Continue with the project of the previous exercise, use the same **main.swift** you already have.

### Tasks

- In your **main.swift** create the function to compare songs by counters. Then use this function to sort the playlist you've created in the previous exercise. 

Rules for this order are:
1. Sorted by count in descending order.
2. If we have songs with the same counter, use the natural/default comparison.

- Write another function to compare two songs by its duration in ascending order and sort the playlist again. Rules are (in desc order)
1. duration (numerically)
2. title (alphabetically)
3. artist (alphabetically)

### Hints
- You can sort of the songs of the playlist by using the following code.
```swift
 myplaylist.songs.sorted(by: compareByPlayCount).forEach {print(’ \($0) [ \($0.count)]’)}
 ```

 ## Console Output
```
My first duration is smaller!
Song 4 already exists in list
'Don't You' by Simple Minds [04:21]
'Life Is A Rollercoaster' by Ronan Keating [03:57]
'Music' by Kelly [05:52]
The song is already in your playlist!
'Music' by John Miles is contained in the playlist
playing list 'Night of the Proms 2016' (total duration: 33:47)
now playing 'Music' by Kelly [05:52]
now playing 'Don't You' by Simple Minds [04:21]
now playing 'Life Is A Rollercoaster' by Ronan Keating [03:57]
now playing 'Unwritten' by Natasha Bedingfield [04:19]
now playing 'The Unforgiven' by Stefanie Heinzmann [03:32]
now playing 'Music' by Kelly [05:54]
now playing 'Music' by John Miles [05:52]
songs sorted by natural order:
'Don't You' by Simple Minds [04:21]
'Life Is A Rollercoaster' by Ronan Keating [03:57]
'Music' by John Miles [05:52]
'Music' by Kelly [05:52]
'Music' by Kelly [05:54]
'The Unforgiven' by Stefanie Heinzmann [03:32]
'Unwritten' by Natasha Bedingfield [04:19]
songs sorted by play count:
'Music' by John Miles [05:52] [11]
'Music' by Kelly [05:52] [11]
'Music' by Kelly [05:54] [11]
'Unwritten' by Natasha Bedingfield [04:19] [5]
'Don't You' by Simple Minds [04:21] [1]
'Life Is A Rollercoaster' by Ronan Keating [03:57] [1]
'The Unforgiven' by Stefanie Heinzmann [03:32] [1]
songs sorted by duration:
'The Unforgiven' by Stefanie Heinzmann [03:32]
'Life Is A Rollercoaster' by Ronan Keating [03:57]
'Unwritten' by Natasha Bedingfield [04:19]
'Don't You' by Simple Minds [04:21]
'Music' by John Miles [05:52]
'Music' by Kelly [05:52]
'Music' by Kelly [05:54]
```
