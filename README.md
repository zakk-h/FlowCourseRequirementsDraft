# FlowCourseRequirementsDraft

idea: flow network to do bipartite matching to determine if a set of classes satisfies the trinity requirements. trivial. generate a set of classes that satisfy it and no more - trivial. slightly tricky to deal with overflow of 3 NS codes, etc because capacity of edge is reached. harder - idk how to do - find the minimum size set that satisfies this. harder - find the minimum weighted set that satisfies this with a weighting being hours per week or interest. another tough thing - from a set of classes you have already taken, carry on to find a cheap max flow - it needs to use the existing classes, but one issue is you can't get all the codes attached to a class - you may have to choose between CZ and SS, etc. one way is to build all possible graphs of where you are at the present and carry on computing the max flow.

so we have a source that goes to each class in the first layer with capacity infinity, and then a class has edges to a mode of inquiry and area of knowledge and seminar nodes, so three different connections, but these are specific versions for each of them, like ModesofInquiryNodeForClass1, for Class2 it is different and so on. the key is going to Modes of Inquiry, it would have capacity 3 and areas of knowledge have capacity 1. then, from there, they can flow into SS, CCI, or whatever with capacity 1, which are the same nodes for all of the courses. then, those flow into a sink node with capacity 2 for most things or whatever, and if it sums to whatever it should be, they satisfy everything, or if it doesn't, you could look at the layer before and see which nodes there (SS, CCI, etc) weren't at their max capacity

to tackle generating a schedule that satisfies all graduation requirements while using the minimum number of courses possible, you need to solve an optimization problem on top of the max flow structure. this can be done with integer linear programming.

greedy approach: pick the course that has the most codes that you need right now. not ideal because you might take ALP EI W instead of ALP CCI, which leaves you with NS CCI instead of NS EI W, and maybe courses don't have NS CCI. suppose there is always a course with the maximum number possible of the codes you need, then the greedy approach would work because that is the only concern. or, it is more obvious why it wouldn't work if it favored the codes you could get 3 of instead of the ones you can only get 1 of, because those are the limiting ingredient in any schedule. 

 I guess for the weights, one way to do it is to take a class, but divide the capacities by the weight, and then duplicate the node weight times so you'd have to choose it that way. that assumes weights are integers but if not you could just scale everything up to the lcm of the weights. however, if we don't have integer capacities, we can't guarantee an integer flow solution, and even if we scale it up, we might get a flow of 1 somewhere, but that represents half the class on the new scale, so we can't quite solve it as nicely.

but, if they're duplicates, then wouldn't they if they chose 1 node with a certain weight, then they'd have to choose the rest of them because if it chose it, that is the best way to get the codes it needs, so then it would choose all the rest of the same class.

the only catch is if a class has say the same codes and the same weight, and then they could mix and match, so we'd have to enforce that it stays with the same class, and perhaps the best way to do that is do something like in MSTS when we add small amounts to each weight to make it unique

to be clear, if we have weights, we'd have to make the same number of copies of each class, like LCM whatever because our algorithm that we are assuming to exist with the integer linear programming optimization is just counting nodes.

or we could minimize straightout: \text{minimize } \sum_{i=1}^n w_i \cdot x_i

other disguised applications: transportation networks: suppliers or warehouses to customer demand points, with intermediate layers being distribution centers or other constraints. you want to get the max flow, but you also want to minimize the cost, and edge weights might be the cost to transport whatever you want to transport there. this is a harder problem because the cost would differ if you shipped things in bulk.

another problem - minimize bandwidth usage in routing data between multiple points. data centers go through routers to get to end users or other data centers.

other interesting algorithms: binary search on the number of classes needed 1 to all_classes and brute force within each number. really, we know that it is lower bounded by 10 because 5x2 and we expect it isn't much higher, but we can't get insights like this with weights

