# BFS-1
# Problem 1
Binary Tree Level Order Traversal (https://leetcode.com/problems/binary-tree-level-order-traversal/)
//TC : O(n)
//SC : O(n)

//Approach:-
1. We will check the level and the length of the list
2. If the length of the list and level are same then we will add another list in the ans list
3. We will iterate through the left node and then add the value of the root to the level it is in represented by the index of the ans list
4. then call dfs for the right side of the Binary TRee
class Solution {

    List<List<Integer>> ans;

    public List<List<Integer>> levelOrder(TreeNode root) {

        ans = new ArrayList<>();

        if(root == null){
            return ans;
        }

        dfs(root, 0);

        return ans;
    }

    private void dfs(TreeNode root, int level){

        if(root == null){
            return;
        }

        if(level == ans.size()){
            ans.add(new ArrayList<>());
        }

        dfs(root.left, level+1);

        ans.get(level).add(root.val);

        dfs(root.right, level+1);
    }    
}

# Problem 2
Course Schedule (https://leetcode.com/problems/course-schedule/)

TC = O(V + E)
SC = O(V)

Approach:-
1. we will make a hashmap finding which course is dependent on what courses 
2. We will make an indegree array and count that each course is dependent on how many other courses
3. We will make intialize a queue and put the courses in the queue which does not require any prerequisites
4. We will run a loop and process the courses in the queue which either had 0 prerequisites or their prerequisites were completed 
5. WE will decrease the prerequisite of all the courses which have take the course we are currently processing and if any of the dependent course becomes 0 then we will add that course to our queue
6. if any of the indegree index is not 0 then we will return false as there is a cycle else true;
class Solution {
    public boolean canFinish(int numCourses, int[][] prerequisites) {
        if (numCourses == 0 || prerequisites.length == 0) {
            return true;
        }

        HashMap<Integer, List<Integer>> hm = new HashMap<>();

        for (int[] pre : prerequisites) {
            hm.putIfAbsent(pre[1], new ArrayList<>());
            hm.get(pre[1]).add(pre[0]);
        }

        int[] indegree = new int[numCourses];
        Queue<Integer> queue = new LinkedList<>();

        for (int i = 0; i < numCourses; i++) {
            if (hm.containsKey(i)) {
                List<Integer> temp = hm.get(i);
                for (int val : temp) {
                    indegree[val]++;
                }
            }
        }

        int count = 0;
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) {
                queue.offer(i);
                count++;
            }
        }

        while (!queue.isEmpty()) {
            int course = queue.poll();
            List<Integer> temp = hm.get(course);

            if (temp != null) {
                for (int depend : temp) {
                    indegree[depend]--;

                    if (indegree[depend] == 0) {
                        queue.offer(depend);
                        count++;
                    }
                }
            }
        }

        return count == numCourses;
    }
}