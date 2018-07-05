# Leetcode: Cheapest Flights Within K Stops     :BLOG:Medium:


---

Cheapest Flights Within K Stops  

---

Similar Problems:  
-   [Bus Routes](https://code.dennyzhang.com/bus-routes)
-   [Review: BFS Problems](https://code.dennyzhang.com/review-bfs)
-   Tag: [#bfs](https://code.dennyzhang.com/tag/bfs)

---

There are n cities connected by m flights. Each fight starts from city u and arrives at v with a price w.  

Now given all the cities and fights, together with starting city src and the destination dst, your task is to find the cheapest price from src to dst with up to k stops. If there is no such route, output -1.  

    Example 1:
    Input: 
    n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]]
    src = 0, dst = 2, k = 1
    Output: 200
    Explanation: 
    The graph looks like this:

![img](//raw.githubusercontent.com/DennyZhang/images/master/code/cheapest-flights-within-k-stops1.png)  

    The cheapest price from city 0 to city 2 with at most 1 stop costs 200, as marked red in the picture.

    Example 2:
    Input: 
    n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]]
    src = 0, dst = 2, k = 0
    Output: 500
    Explanation: 
    The graph looks like this:

![img](//raw.githubusercontent.com/DennyZhang/images/master/code/cheapest-flights-within-k-stops2.png)  

    The cheapest price from city 0 to city 2 with at most 0 stop costs 500, as marked blue in the picture.

Note:  

-   The number of nodes n will be in range [1, 100], with nodes labeled from 0 to n - 1.
-   The size of flights will be in range [0, n \* (n - 1) / 2].
-   The format of each flight will be (src, dst, price).
-   The price of each flight will be in the range [1, 10000].
-   k is in the range of [0, n - 1].
-   There will not be any duplicated flights or self cycles.

Github: [challenges-leetcode-interesting](https://github.com/DennyZhang/challenges-leetcode-interesting/tree/master/cheapest-flights-within-k-stops)  

Credits To: [leetcode.com](https://leetcode.com/problems/cheapest-flights-within-k-stops/description/)  

Leave me comments, if you have better ways to solve.  

---

    ## Blog link: https://code.dennyzhang.com/cheapest-flights-within-k-stops
    ## Basic Ideas: BFS
    ##
    ## Complexity: Time O(n), Space O(n)
    class Solution:
        def findCheapestPrice(self, n, flights, src, dst, K):
            """
            :type n: int
            :type flights: List[List[int]]
            :type src: int
            :type dst: int
            :type K: int
            :rtype: int
            """
            import collections
            import sys
            d = collections.defaultdict(lambda: {})
            for [start, end, price] in flights:
                m = d[start]
                m[end] = price
            price_map = {}
    
            queue = collections.deque()
            seen = set([])
    
            queue.append(src)
            seen.add(src)
            price_map[src] = 0
            level = 0
            min_price = sys.maxsize
            while len(queue) != 0:
                level += 1
                # print(level, K, src, dst, queue)
                if level>K+1: break
                for k in range(len(queue)):
                    p = queue.popleft()
                    # find the neighbors
                    if p in d:
                        m = d[p]
                        for q in m:
                            if q == dst:
                                min_price = min(min_price, price_map[p]+m[q])
                                continue
                            # not visited
                            if (q not in seen):
                                # print(price_map, p, q, m)
                                price_map[q] = price_map[p]+m[q]
                                queue.append(q)
                                seen.add(q)
                            # we have a better solution
                            elif price_map[q] > price_map[p]+m[q]:
                                queue.append(q)
                                price_map[q] = price_map[p]+m[q]
    
            return min_price if min_price != sys.maxsize else -1