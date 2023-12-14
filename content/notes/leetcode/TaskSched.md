---
title: "Task Scheduler"
date: 2022-08-10
lastmod: 2022-08-10
tags:
- leetcode
- heaps
- greedy
---

The problem can be found [here](https://leetcode.com/problems/task-scheduler/)

# The Problem
You are given a character array `tasks` that represent tasks that a CPU needs to perform, where each letter represents a different task. Tasks can be done in any order, and they take one unit of time to execute.

However, there is a non-negative integer `n` that represents the cool down period between two same tasks for the CPU. This means there must be `n` units of time between two identical tasks for the CPU. In that time, the CPU can be doing other tasks or staying idle.

Return the least amount of units of time needed to complete all the tasks

# The Approach
When choosing which task to execute, we have to take into consideration two factors: the amount of times each distinct task needs to be executed (how often it appears in the array), and which tasks can we perform at the given time. The general principle will be, at any given time, we want to execute the task that:
- Can be executed according to the cooldown time
- Has the highest number of executions left among the tasks that can be executed.

This will require us to keep track of tasks that can be executed at any given times, how many times they need to be executed, and the tasks that are in cool down. Because we want to always execute the task that requires the most executions, we can use a heap data structure to store the tasks that can be executed. This will allow us to find the maximum execution times in `log(n)` time. We can then store cooling down tasks in any ordered data structure. A queue will work best for us in this case. The algorithm is as follows. We first create a heap that will store objects which contain the task character, the number of times it needs to be executed, and the next possible time it can be executed. Each object will be initialized with a distinct character, the frequency of that character in the array, and `0` for the next possible time. We will then initialize an empty queue for tasks that will be on cooldown. We will then iterate the following process until both data structures are empty and keep track of the time.

We will pop the first element off of the heap. If this task still has more executions to be performed, we will perform the following:
- Decrement the number of times execution needed by 1
- Set the time it can next be used to `time + n` where time is the current time.
- Add to the queue.

It is added to the queue now because it will be in cooldown. After this process is executed, we will check the cooldown queue. We will check the first element in the queue, and see if its cooldown period has expired yet. If it has, we will pop it off of the queue and add it back to the heap. These two processes will continue to go through, while also keeping track of time, until **both** data structures are empty. This ensures no tasks are in cool down and all have been executed. After this, we will have our minimum time we can return.

# Code
```java
class Solution {
    
    class Task implements Comparable<Task>{
        char task;
        int nextUseTime;
        int numTasks;
        
        public Task(char s, int n, int tasks){
            task = s;
            nextUseTime = n;
            numTasks = tasks;
        }
        
        public boolean equals(Task t){
            return (task == t.task) && (nextUseTime == t.nextUseTime) && (numTasks == t.numTasks);
        }
        
        public int compareTo(Task t){
            return t.numTasks - numTasks;
        }
        
        public String toString(){
            return task + " " + nextUseTime + " " + numTasks;
        }
    }
    
    public int leastInterval(char[] tasks, int n) {
        Map<Character, Integer> charToFreq = new HashMap<Character, Integer>();
        PriorityQueue<Task> minHeap = new PriorityQueue<Task>();
        for(char c: tasks){
            if(charToFreq.containsKey(c)){
                charToFreq.put(c, charToFreq.get(c) + 1);
            }else{
                charToFreq.put(c, 1);
            }
        }
        for(char c: charToFreq.keySet()){
            minHeap.offer(new Task(c, 0, charToFreq.get(c)));
        }
        Queue<Task> idle = new LinkedList<Task>();
        int time = 0;
        while(!minHeap.isEmpty() || !idle.isEmpty()){
            // System.out.println(minHeap);
            // System.out.println(idle);
            time += 1;
            Task polled;
            if(!minHeap.isEmpty()){
                polled = minHeap.poll();
                polled.numTasks -= 1;
                polled.nextUseTime = n + time;
                if(polled.numTasks > 0)
                    idle.add(polled);
            }
            if(!idle.isEmpty() && idle.peek().nextUseTime <= time){
                Task notIdle = idle.poll();
                minHeap.offer(notIdle);
            }
        }
        return time;
    }
}
```
