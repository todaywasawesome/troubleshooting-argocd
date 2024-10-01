This example shows how resoruces can be orphaned when changing the name of an application. 

Create the parent app: `kubectl apply -f lost-parent-app.yaml`

Then change the name of the child app in git, this will not update the child app, instead it will create a new child app. 


