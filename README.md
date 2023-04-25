To push this to Openshift, install kompose: https://kompose.io/ and openshift CLI tools and Docker Desktop.
Clone the repository or download this as a zip.
Copy .env.example into .env and add the database information. No quotation marks needed.
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

If you want to specify the subdomain, you can do so by adding subdomain in the route file as such:

    spec:
      subdomain: this-is-my-subdomain
This allows you to specify the subdomain without specifying the whole domain name.

To communicate between 2 containers, the container name in the Docker compose yml file can be used, similar to how it is done in Docker compose. This can be seen in the Nginx configuration file in ./webUI/default.conf, where Nginx is setup to proxy forward all ./api requests to http://django:8000.