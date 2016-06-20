# Introduction

This guide was originally conceived by [@k4kfh](http://github.com/k4kfh).

## Author's Note

I begin writing this in June 2016, after working on [ZephyrCab](http://github.com/k4kfh/ZephyrCab) for over a year. When I started ZephyrCab, I was not only new to JavaScript development, I was new to railroad physics. In a way, I was new to railroads. I knew some history, and I knew a bit of what made them tick, but I did not have any knowledge of how to drive a train, or the science at work on the railroad. In writing ZephyrCab, I spent days at a time reading about physics. I started with the classic ``f=ma`` and worked my way up from there. During this research and learning, I found myself battling a recurring problem: *there simply isn't enough good literature on these problems.* There are a few articles here and there, but the complete mathematical backend of a train simulator was, for the most part, closed-source and kept secret.

After spending over a year slowly piecing together the information I needed, with help from two men who generously gave time and expertise to teach me, I have inched forward in the development of ZephyrCab. I've learned a lot in this time, and as beneficial to me as the search for information has been, I don't want every person who's curious about the inner workings of trains to find themselves in my position. My goal in writing *The Idiot's Guide to Railroad Physics* is to make this specialized, hard-to-find information available to all.

With that in mind, I have included two types of content in this guide: concepts and numbers. What do I mean "numbers," you ask? For every hour I spent searching for a formula or other form of conceptual knowledge on railroading, I spent at least 30 minutes searching for the actual numbers to use with these formulas. Wheel diameters, pipe lengths, friction coefficients, reservoir volumes, air compressor flow rates; the list goes on. Some of this information can be found in locomotive data sheets, but some of it required much more digging than that. Therefore, in addition to information on how these things work, I felt it necessary to include the "actual numbers" to make these concepts function in practice.

I hope you enjoy this guide! If you'd like to contribute, please visit the GitHub project, [k4kfh/idiotsGuideToRailroadPhysics](http://github.com/k4kfh/idiotsGuideToRailroadPhysics).

Happy railroading!
