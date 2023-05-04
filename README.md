
To push this to Openshift, install kompose: https://kompose.io/ and openshift CLI tools and Docker Desktop.
Clone the repository or download this as a zip.\
Note: Due to a current incompatibility between the latest release of kompose and Openshift CLI tools, doing these steps on the latest tools will not work. The buildconfig and deploymentconfig files have their `apiVersion` wrong. To fix this, do one of the following:

 1. Manually edit the files. Can refer to this kompose PR to check what needs to be changed: https://github.com/kubernetes/kompose/pull/1598 but essentially: change `apiVersion` in buildconfig to `image.openshift.io/v1` and deploymentconfig to `apps.openshift.io/v1`
 2. Use an older version of oc cli tool. Version 4.10.9 is tested working at at the time of writing can be retrieved by installing the Openshift extension in Docker Desktop and retrieving the oc tool used by them. This requires some digging so it isn't that easy.
 3. **Recommended:** Compile the latest kompose. This is quite easy, install [Go](https://go.dev/doc/install) then run `go build -o kompose.exe main.go`.

Copy .env.example into .env and add the database information. No quotation marks needed.
Edit the Django settings.py to the one in the project, there is a commented default Django database config, commented out so that this example works without an external database.
Then run the following commands:

    docker compose -f recipeUI.yml config > stack.yml
    kompose convert -f stack.yml --provider openshift --build build-config --build-repo https://github.com/hometue/openshift-test.git
    oc create -f django-imagestream.yaml
    oc create -f react-imagestream.yaml
    oc create -f django-buildconfig.yaml
    oc create -f django-deploymentconfig.yaml
    oc create -f django-service.yaml
    oc create -f react-buildconfig.yaml
    oc create -f react-deploymentconfig.yaml
    oc create -f react-route.yaml
    oc create -f react-service.yaml

Important: Imagestream files must be pushed before anything else, because every other file will refer to it and without the image there can be no container.

If you want to specify the subdomain, you can do so by adding subdomain in react-route.yaml file as such:

    spec:
      subdomain: this-is-my-subdomain
This allows you to specify the subdomain without specifying the whole domain name.

To communicate between 2 containers, the container name in the Docker compose yml file can be used, similar to how it is done in Docker compose. This can be seen in the Nginx configuration file in ./webUI/default.conf, where Nginx is setup to proxy forward all ./api requests to http://django:8000.

If accessing via https is desired, because of security or perhaps because browser default is https when we key in domain name, add this to react-route.yaml file:

    tls:
	    termination: edge
	    insecureEdgeTerminationPolicy: Allow
