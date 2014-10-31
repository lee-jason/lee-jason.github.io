---
layout: post
title: "Problems: Halloween Edition"
date: 2014-10-31 05:53:42 -0700
comments: true
categories: ['problems', 'algorithms']
---
<strong>Problem:</strong> You're a kid, and tonight's the greatest night a kid could have next to Christmas. Halloween night is coming soon and you're prepping your bags for the trick or treating.  The problem is, is that your parents are tired and are only willing to walk a certain number of feet before they call it a night.  Given a map detailing the amount of feet between each house and the amount of candy given per house, how do you maximize the amount of candy gained before having to return back home?

PICTURE!

<!-- more -->
Of course, graphing optimization problems.  Isn't this what every kid is thinking on Halloween?  I know it was definitely keeping <i>me</i> up last night, even though I don't trick or treat anymore.  This is slightly different from the <a href="http://en.wikipedia.org/wiki/Travelling_salesman_problem">traveling salesman problem</a> in that each location now has a weight, and now the salesman doesn't need to travel to every location.  I guess its a variant of the traveling salesman problem if the salesman's car had n amount of miles of fuel and the each location had variable amounts of money to be made and he needs to return home by the end of the trip. In this problem, we can assume that you house is accessible through a loop.  This algorithm also covers nodes that require paths to be traversed through twice, such as a house at the end of the street.  If we assume that all nodes have more than one connecting path then we can save search time by considering not rechecking already visited nodes.

<h2>Proposed Solution</h2>
So after starting to really think about this problem at 6am and it now being 11am, the problem is not as easy as I thought.  I've given it some thought and this is what I propose.  In order to get the true most optimal path a ton of paths have to be evaluated. A backtrack algorithm would be used to trace down paths starting from the house node and see if a branch could end up back at the house node before it we reach the step quota.  This algorithm would be pretty terrible due to there being potentially O(N^E) paths to traverse through given that N is nodes and E is edges but since backtracking is used we can save ourselves from checking dead end paths (paths that extend past the quota).

<h3>A Solution</h3>
Here's some pseudocode about
{% codeblock lang:java halloween path optimization pseudocode %}
    Class Node{
        Node class contains...
        int candyValue
        NodesNPaths[] connectingNodes;
        int totalCandyValue
        int totalSteps
        Node prevNode
        boolean visited
    }

    //bestPath modifies rootNode to have history of 
    Node bestPath(Node rootNode, Node currNode, int pathLength, Node prevNode, int stepQuota, boolean initial){
        //route is a failure when totalsteps is over the stepquota,
        //when the node has already been visited AND its not the root node
        //its fine to go back to the root node to update its values if its under the step quota and its already been visited.
        if(currNode.totalSteps > stepQuota || currNode.visited && currNode != rootNode){
            return null;
        }
        
        //replace tally of path of most candy
        currNode.totalCandyValue = currNode.candyValue + prevNode.totalCandyValue;
        currNode.totalSteps += pathLength + prevNode.totalSteps;
        currNode.prevNode = prevNode;
        currNode.visited = true;

        for(Node connectingNode : currNode.connectingNodes){
            Node nextNode = bestPath(rootNode, connectingNode.node, connectingNode.pathLength, Node.currNode, stepQuota, false);
            if(nextNode == null){
                //reset node status, so next branch can access this point again
                currNode.visited = false;
                return null
            }
        }
    }
{% endcodeblock %}

The algorithm above will update the rootNode and will eventually find the best path.  So in plain english here's what's going on.
The method above will recursively dig into the graph in a depth first search.  the function will break when it discovers that the path its on exceeds the amount of allotted steps.  Each traversed node keeps track of the previous node that it came from as well as a running tally of how many steps it took to get there as well as the amount of candy gained.  Each nodes running tally is checked against whether the addition of the current node offers more candy, and if it does then 

There's a lot of solutions I jotted down but these only give good enough optimal paths, you can read those below



<h3>Compromising Good Enough Solutions</h3>
So lets start with the trivial.  Given that we have <code>n</code> amount of feet before we get tired we can pick a set of points where the total distance is less than <code>n/2</code>.  This will ensure that we will have enough energy to walk back along the same path that we took. This will guarantee a working solution, problem is, it sucks!  We're missing out on extra candy by wasting half of our available distance by backtracking on houses already visited.

PICTURE!

<h2>Okay, I'm kind of thinking now Solution</h2>
So first of all, we would want to make another graph marking the shortest distance it would take to get home from any point.  Fortunately for us Djikstra has lead the way and we can use a modified version of his algorithm to figure out the shortest path back home.

PICTURE!

This means we can initially start picking high value targets that give out the king sized candy bars and from any point we end up on, we evaluate whether we have enough steps to visit another house and still have enough energy to go home or we just head home at that point following the shortest path.  Of course even this solution has issues.  High value houses may be very widely spread apart, and the path back home may overlap houses that we've already visited.  Of course there has to be a better solution

<h2>I'm thinking a little harder now</h2>
Using the shortest distance with history graph, we can backtrack from our starting home base to find a pretty good solution.  We start on our home base node, find the highest distance 