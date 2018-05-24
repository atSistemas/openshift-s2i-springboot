# openshift-s2i-springboot
Spring Boot Demo - Minishift

Provision minishift, setup it and deploy microservice springboot 
from buildConfig execution.

## Requirements
- [minishift](https://docs.openshift.org/latest/minishift/getting-started/installing.html) 1.5+
- Make sure minishift is available in your PATH

## Build

### All-in bootstrap
* bootstrap
```
./minishift-start && \
./minishift-create-projects && \
./minishift-create-app && \
./minishift-bc-run
```

### Manual step by step
* Start with clean installation
```
minishift delete --clear-cache
```
* minishift-start
```
minishift start --vm-driver=virtualbox && \
minishift oc-env
```
This will create a new project with name example
* minishift-create-projects
```
eval $(minishift oc-env)
oc login -u developer
# Create Projects
echo "Creating example namespace ..."
oc new-project example --display-name="example SprintBoot"
```
This will create a new app from polarisOpenShift.yaml template
*  minishift-create-app
```
eval $(minishift oc-env)
oc login -u developer
# Create Projects
echo "Creating example namespace ..."
oc new-app -f ../templates/polarisOpenShift.yaml -n example
```
This will execute the buildConfig object generate from the polarisOpenShift template
* minishift-bc-run
```
eval $(minishift oc-env)
oc login -u developer
# Start-build Build Config
oc start-build polaris
```
## Note
The last steps generate a imageStream from the build strategy s2i (source to image) from 
source code of a example git repository (SpringBoot). Once the image is generate, 
the infraestructure will be setup fot the springboot service. 