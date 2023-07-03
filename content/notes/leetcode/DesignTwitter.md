---
title: "Design Twitter"
tags:
- coding
- software
- heaps
---
The problem can be found [here](https://leetcode.com/problems/design-twitter/)

# The Problem
You are to design a simplified version of Twitter, where users can post tweets, follow/unfollow other users, and is able to see the 10 most recent tweets in the user's news feed.

You should implement the Twitter class:
- `Twitter()` Initializes your twitter object.
- `void postTweet(int userId, int tweetId)` Composes a new tweet with ID `tweetId` by the user `userId`. Each call to this function will be made with a unique `tweetId`.
- `List<Integer> getNewsFeed(int userId)` Retrieves the 10 most recent tweet IDs in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user themself. Tweets must be ordered from most recent to least recent.
- `void follow(int followerId, int followeeId)` The user with ID `followerId` started following the user with ID `followeeId`.
- `void unfollow(int followerId, int followeeId)` The user with ID `followerId` started unfollowing the user with ID `followeeId`.

# The Approach
We can start with the easier part of this problem, which is posting a tweet and handling user's followers. We can use a dictionary for each of these: one to map a user to their tweets and one to map a user to their followers. For the list of tweets, we use a dictionary with an integer key and a list of integers as the value (list of tweet id's). Each time we invoke the postTweet method, we will add the tweet id to the list of tweets mapped to the user id. For the followers, we will use a dictionary that maps an integer key to a set of integers. We use a set because we do not require any form of ordering for the list of followers. Each time we invoke the follow method, we will add the followee id to the set of followers mapped to the follower id. Each time we invoke the unfollow method, we will remove the followee id from the set of followers mapped to the follower id.

Now, we can move on to the more difficult part of this problem, which is retrieving the 10 most recent tweets in the user's news feed. To do this, first, we have to keep track of the time a tweet is posted. In this simplified version of Twitter, only one tweet can be posted at a time. Therefore, we can keep track of the time each tweet is posted by keeping a class variable that keeps track of the time, and in the `postTweet` method, we store the tweet as well as the time it was posted. Now, each user will have a list of tweets from least recent to most recent, each stored with their time posted. Now, this method boils down to another well-known problem: [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/). Given a user id, we will have a collection of sorted list, and we will want to sort at least the 10 most recent tweets out of these sorted lists. We can use a heap to do this. We first store the most recent tweet from each of the user's followers (including) their own. At each iteration, we pop the most recent tweet from the heap and add it to the feed. Then, whichever user this tweet came from, we add the next most recent tweet from that user to the heap. We repeat this process until the heap is empty, or we have 10 tweets in the feed. To perform this, for each item we store in the heap, we need to keep track of the time it was posted, the user it was posted by, and the index of the tweet in the user's list of tweets. After this process, we will have the 10 most recent tweets in the user's news feed.

# The Code

```py
class Twitter:

    def __init__(self):
        self.user_to_followers = defaultdict(set)
        self.user_to_tweets = defaultdict(list)
        self.count = 0

    def postTweet(self, userId: int, tweetId: int) -> None:
        tweet_tuple = (self.count, tweetId)
        self.count -= 1
        self.user_to_tweets[userId].append(tweet_tuple)

    def getNewsFeed(self, userId: int) -> List[int]:
        list_of_tweets = []
        res = []
        follows = self.user_to_followers[userId]
        follows.add(userId)
        heap = []
        for follow in follows:
            tweets = self.user_to_tweets[follow]
            index = len(tweets) - 1
            if(index >= 0):
                count, tweetId = tweets[index]
                heap.append([count, tweetId, follow, index])
        heapq.heapify(heap)
        while(len(res) < 10 and len(heap) > 0):
            popped = heapq.heappop(heap)
            tweetId = popped[1]
            index = popped[3]
            userId = popped[2]
            res.append(tweetId)
            if(index > 0):
                count, tweetId = self.user_to_tweets[userId][index - 1]
                heapq.heappush(heap, [count, tweetId, userId, index - 1])
        return res
        

    def follow(self, followerId: int, followeeId: int) -> None:
        self.user_to_followers[followerId].add(followeeId)

    def unfollow(self, followerId: int, followeeId: int) -> None:
        if(followeeId in self.user_to_followers[followerId]):
            self.user_to_followers[followerId].remove(followeeId)


# Your Twitter object will be instantiated and called as such:
# obj = Twitter()
# obj.postTweet(userId,tweetId)
# param_2 = obj.getNewsFeed(userId)
# obj.follow(followerId,followeeId)
# obj.unfollow(followerId,followeeId)
```