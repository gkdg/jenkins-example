## This example uses official jenkins docker image.

Run the command:
```make run```

Read the logs of the newly created container in order to find initial admin password by running below commands.

Find the container id:
```docker ps```

Check the logs of the conainer to find the initial password:
```docker logs <container_id>```

There are three credentials scope.
1-Global scope
2-System scope (only available on jenkins server not for jenkins job)
3-Project scope(only comes with multi-pipeline branch option and secrets are naturally has project scope)

If you wanna build only a particular branch go to on console 
your-pipeline => branch sources => Behaviours => discover branches; with java regular expression. 

For instance below regular expression will only accept branches main, dev or feature
```^main|dev|feature.*$```