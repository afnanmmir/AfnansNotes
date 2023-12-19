---
title: "Design Browser History"
date: 2022-10-25
lastmod: 2022-10-25
tags:
- leetcode
- stack
---
The problem can be found [here](https://leetcode.com/problems/design-browser-history/)

# The Problem
You have a browser of one tap where you start at `homepage` and can visit another `url`, get back in the history some number of `steps` or move forward in the history some number of `steps`.

Implement a `BrowserHistory` class:
- `BrowserHistory(string homepage)` initializes the object with the homepage of the browser
- `void visit(string url)` visits `url` from the current page. The current page will be put in the backwards history. This will also clear up the forward history.
- `string back(int steps)` moves `steps` back in history. If you can only go back `x` steps such that `x < steps`, then go back `x` steps. Return the url that you end up at.
- `string forward(int steps)` moves `steps` forward in history. If you can only go forward `x` steps such that `x < steps`, then go forward `x` steps. Return the url that you end up at.

# The Approach
This class we will create will require 3 fields. We will need a field that will keep track of the current page, a field to keep track of the history going backwards, and a field that will keep track of the history going forwards. We can see that the history data structures must be LIFO data structures, because the last element to be put inside the history will be need to be the first one to be popped off when we go back or forwards. Because of this, we will use a stack structure for the backward history and the forward history. 
When we perform a visit operation, we will first clear the forward history. We will then add the current page to the backwards history. We will then set the current page to the url passed into visit.
When we perform a back operation with a given amount of steps, we will iteratively pop elements off of the backwards history stack. These elements we pop off will then also be pushed onto the forward history stack. We will continue to pop off elements from the backwards history until we have popped `steps` amount of times or when the stack becomes empty (whichever comes first). The last element we pop will be the current page we are on.
Performing the forward operation will be very similar, except we will be popping from the forward history and pushing to the backward history. Again, the final element we pop off will be the current page.

# The Code
```python
class BrowserHistory:

    def __init__(self, homepage: str):
        self.homepage = homepage
        self.curr_page = homepage
        self.backwardHistory = collections.deque()
        self.forwardHistory = collections.deque()

    def visit(self, url: str) -> None:
        self.forwardHistory.clear()
        self.backwardHistory.appendleft(self.curr_page)
        self.curr_page = url

    def back(self, steps: int) -> str:
        num_steps = 0
        while(len(self.backwardHistory) > 0 and num_steps < steps):
            self.forwardHistory.appendleft(self.curr_page)
            previous = self.backwardHistory.popleft()
            self.curr_page = previous
            num_steps+=1
        return self.curr_page
        

    def forward(self, steps: int) -> str:
        num_steps = 0
        while(len(self.forwardHistory) > 0 and num_steps < steps):
            self.backwardHistory.appendleft(self.curr_page)
            nxt = self.forwardHistory.popleft()
            self.curr_page = nxt
            num_steps += 1
        return self.curr_page


# Your BrowserHistory object will be instantiated and called as such:
# obj = BrowserHistory(homepage)
# obj.visit(url)
# param_2 = obj.back(steps)
# param_3 = obj.forward(steps)
```
