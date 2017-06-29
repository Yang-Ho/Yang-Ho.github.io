---
layout: post
title:  "Lessons learned from Spring 17"
categories: Post 
---

With the spring semester long behind me, I had time to reflect on what I had achieved and my mistakes.
I had always considered myself an ok programmer that had a lot to learn. 
Turns out I was right.

Of all the programming languages I've used, Python was the most frequently used while C++ was second.
However, my knowledge of C++ felt pretty basic while my Python knowledge was more advanced (not by much).
One of the reasons I decided to work with Dr. Stallmann was so I had a chance to improve my C++ knowledge.
Over the course of the semester although things didn't work out entirely the way I had hoped, I still learned a great deal about C++ and programming in general.

Ultimately, the entire semester could be summerized by the following:

### Theory $$\not =$$ Practice


# Major points:
0) C++ is complicated and I now understand why people hate it
    * I started reading effective C++ (the 1st? edition so a bit outdated but still good to my understanding)
        * The way he describes C++ really made it seem less complicated:
            * C++ is really 4(7 if you include C++11, C++14, C++17) languages put into 1:
                * C part (like 99% of it)
                * C++ part (Mostly OOP)
                * Stl part
                * Templates

1) sets in c++ are not what you think
    * Sets traditionally have O(1) complexity for access
        * In Python sets are basically dicts with dummy values
    * c++ sets guarentee O(lgn)
        * why? they use redblack/binary search trees
        * why not a hash map?
            * The stl has certain requirements for its' data structures/containers
    * What should I do:
        * depends
            * if you know all possible elements, then a simple array of bool's or 0/1's will suffice
            * a sorted vector can be just as fast
            * a hashmap with dummy values (note: maps are like sets in that they aren't hashmaps! unordered_maps are though)

2) Understanding algorithms is important, but knowing key details of the programming languages you use is also important
    * In python, sets are dicts (i.e. hashmaps) so they are fast
    * I assumed that the algorithm we implemented in vcsolver would be fine for vc-framework
        * I knew the algorithm, but I didn't know about stl::set's requirements/guarentees
            * So when I went on to implement the matching algorithm, I used set because I thought it behaved similar to set()
            * I was wrong (see above)
    * If I had a better understanding of C++ I could have saved A LOT of time from debugging/profiling
