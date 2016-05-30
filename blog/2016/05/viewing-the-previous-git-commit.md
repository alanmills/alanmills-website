Viewing the previous Git commit
===============================
**Author**: Alan Mills  
**Date**: [30 May 2016 09:45](/blog/history/2016-05.md)
**Tags**: [Git](/blog/categories/git.md)
**Status**: Publish

To get the last git commit with the commit message and a list of the files that were changed, run the command `git log --name-status HEAD^..HEAD`.
``` bash
$ git log --name-status HEAD^..HEAD
commit 1d043e864f9237e4d8fe0bdf9d80ee6974fdb488
Author: Alan Mills <alan@alanmills.info>
Date:   Sat May 29 18:00:47 2016 +0100

    Using a ViewModel to bring back the Expense Group and Expense Group Status in a single view.  Related Work Items: #1896

M       ExpenseTracker/ExpenseTracker.WebClient/Controllers/ExpenseGroupsController.cs
M       ExpenseTracker/ExpenseTracker.WebClient/ExpenseTracker.WebClient.csproj
A       ExpenseTracker/ExpenseTracker.WebClient/Models/ExpenseGroupViewModel.cs
M       ExpenseTracker/ExpenseTracker.WebClient/Views/ExpenseGroups/Index.cshtml
```

If you want to view the differences in the files of the last commit, run the command `git diff HEAD^..HEAD`.

``` bash
$ git diff HEAD^..HEAD
diff --git a/ExpenseTracker/ExpenseTracker.WebClient/Controllers/ExpenseGroupsController.cs b/ExpenseTracker/ExpenseTracker.WebClient/Controllers/ExpenseGroupsController.cs
index 9a82a78..3faf00f 100644
--- a/ExpenseTracker/ExpenseTracker.WebClient/Controllers/ExpenseGroupsController.cs
+++ b/ExpenseTracker/ExpenseTracker.WebClient/Controllers/ExpenseGroupsController.cs
@@ -4,6 +4,7 @@ using System.Net.Http;
 using System.Threading.Tasks;
 using System.Web.Mvc;
 using ExpenseTracker.WebClient.Helpers;
+using ExpenseTracker.WebClient.Models;
 using Newtonsoft.Json;

...
```
