# Blue Green Deployments

The master branch is the blue deployment, the branch you create will be the green deployment.

Launch the terminal in the OpenShift Console sandbox. It will use the default project created during the setup of your sandbox. After launching the terminal, follow the steps given below. Alternatively, you can do the same using the console interface. Refer to this [tutorial](https://developers.redhat.com/courses/openshift/getting-started).

## Blue app from master

    oc new-app https://github.com/{username}/blue-green-openshift#master --name=blue --strategy=source

## Expose bluegreen service (using blue)

    oc expose service blue --name=bluegreen

## Green app deploy

    oc new-app https://github.com/{username}/blue-green-openshift#green --name=green

## Switch services to green

    oc get route/bluegreen -o yaml | sed -e 's/name: blue$/name: green/' | oc replace -f -
    
Breakdown of the above command:
1. `oc get route/bluegreen -o yaml` : Returns the route configuration in YAML format 
 ![image](https://user-images.githubusercontent.com/51695690/201993082-2c8bb129-4791-408d-8990-ff59732a7284.png)
2. `sed -e 's/name: blue$/name: green/'` : The `sed` command replaces `name: blue` with `name: green` in the output above.
3. `oc replace -f -` : Replaces the route with the new config from stdin (first command).


## Switch services back to blue

    oc get route/bluegreen -o yaml | sed -e 's/name: green$/name: blue/' | oc replace -f -
