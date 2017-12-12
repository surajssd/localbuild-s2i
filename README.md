# Build container images

### Create a new OpenShift project

```console
oc new-project ticker
```

### Build image

```console
$ kedge build --s2i --image ticker -b centos/python-35-centos7:3.5
imagestream "ticker" created
buildconfig "ticker" created
INFO[0000] Starting build for "ticker"                  
build "ticker-1" started
Receiving source from STDIN as archive ...
---> Installing application source ...
---> Installing dependencies ...
Collecting flask (from -r requirements.txt (line 1))

...

Pushed 8/10 layers, 80% complete
Pushed 9/10 layers, 90% complete
Pushed 10/10 layers, 100% complete
Push successful
```

Here since the code base is python based we are using the builder image that is meant to build python code base which is `centos/python-35-centos7:3.5`. Also the output imagestream will be named as `ticker`.

### Deploying application

Deploy the kedge configs in `configs` directory:

```console
$ kedge create -f configs/
service "ticker" created
route "ticker" created
deploymentconfig "ticker" created
service "redis" created
deploymentconfig "redis" created
```

### Doing curl on endpoint

Get the route

```console
$ oc get route
NAME      HOST/PORT                            PATH      SERVICES   PORT      TERMINATION   WILDCARD
ticker    ticker-ticker.192.168.39.62.nip.io             ticker     <all>                   None
```

Verify that the application is running fine:

```console
$ curl ticker-ticker.192.168.39.62.nip.io
<h3>Hello Kubernauts</h3> <br/><h2>Number of Hits:</2> 1<br/>
```

**Note**: Above URL might be different for you.
