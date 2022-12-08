# dev_Zeppelin
Zeppelin Spark Notes and Workspace


#### Load External Dependencies/Jars
```
%dep z.load("/<path>/packages.jar")
```

or

```
%spark.dep
z.reset() // clean up previously added artifact and repository

// add maven repository
z.addRepo("RepoName").url("RepoURL")

// add maven snapshot repository
z.addRepo("RepoName").url("RepoURL").snapshot()

// add credentials for private maven repository
z.addRepo("RepoName").url("RepoURL").username("username").password("password")
```

