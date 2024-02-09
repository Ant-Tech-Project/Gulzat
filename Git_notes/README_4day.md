Cholpon I love  you. Good luck to you!!! I love you too!!
What can a cause of conflict in Git?
When working in Git, users can combine commits from two different branches through an action known as merging. Files are automatically merged unless there are conflicting sets of changes (i.e. the commits update the same line of code differently).
A merge conflict is an event that occurs when Git is unable to automatically resolve differences in code between two commits.
When all the changes in the code occur on different lines or in different files, Git will successfully merge commits without your help.
However, when there are conflicting changes on the same lines, a “merge conflict” occurs because Git doesn’t know which code to keep and which to discard.
Here is a good article, with good explanation when conflict come up and how to resolve it: https://www.freecodecamp.org/news/how-to-fix-merge-conflicts-in-git/
A conflict in Git arises when there are conflicting changes made to the same lines of a file in different branches or commits that Git is attempting to merge. Conflicts occur when Git is unable to automatically determine how to merge the changes. Some common scenarios that can cause conflicts include:

1. **Parallel Changes:**
   - Different branches or commits modify the same lines of a file independently.

2. **File Renaming or Moving:**
   - If a file is renamed or moved in one branch and modified in another, Git might struggle to reconcile the changes.

3. **Deletion Conflicts:**
   - If one branch deletes a file while another branch modifies or renames the same file, a conflict can occur.

4. **Branch Divergence:**
   - When two branches have diverged significantly, and changes in one branch conflict with changes in another upon merging.

5. **Whitespace Changes:**
   - Conflicts can occur if one branch modifies the content of a line, and another branch changes only the whitespace in the same line.

6. **Merge Commits:**
   - Conflicts can also arise when merging if there are changes in both the branch being merged and the target branch since their last common ancestor.

7. **Manual Edits to Common Ancestor:**
   - If there are manual edits to the common ancestor of two branches (e.g., changes made directly on the remote repository), conflicts may occur during a pull or merge.

When a conflict occurs, Git marks the conflicted sections in the affected files and pauses the merging process, leaving it to the user to resolve the conflict manually. Resolving conflicts typically involves reviewing the conflicting changes, deciding how to merge them, and then marking the conflict as resolved.

Conflicts are a natural part of collaborative development, especially when working on different features or bug fixes concurrently. Proper conflict resolution ensures that the final merged code incorporates the intended changes without introducing errors or inconsistencies.

Additional useful link regarding solving conflicts and merging commands: 
1. https://unstop.com/blog/merge-in-git
2. https://www.freecodecamp.org/news/resolve-merge-conflicts-in-git-a-practical-guide/