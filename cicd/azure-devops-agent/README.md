

# Builder Image

Create a builder image. This will be the image that runs the agent and waits for work. The OCP manifest below will build the image and create an image stream to store the newly minted image. While you don't need to change any parameters to get this working you can explore the parameters for customizations.

```console
oc process -f https://raw.githubusercontent.com/jleach/openshift-components/master/cicd/azure-devops-agent/openshift/build.yaml | oc apply -f -
```

# Run

Its best to create an deployment config with the params you want so that the image will restart properly and be managed by OCP. To do a quick test run you can use the command below. Replace `blabla-tools` with your tools namespace; and use `oc project` to make sure you're in the correct namespace before you run the command. It will use the Jenkins service account so that it has the proper permissions to to act as a builder.

```console
oc run az-image --image=docker-registry.default.svc:5000/blabla-tools/azure-develop-agent:v2.153.2  --replicas=1 --restart=Never --env="AZ_DEVOPS_ORG_URL=$AZ_DEVOPS_ORG_URL" --env="AZ_DEVOPS_TOKEN=$AZ_DEVOPS_TOKEN" --requests="cpu=1,memory=1Gi" --limits="cpu=1,memory=1Gi" --serviceaccount=jenkins
```

Once run the agent will come on-line and appear in the `default` pool with the name `az-image`.


# Build

There are other options you can pass as environment variables. See the [startup script](https://raw.githubusercontent.com/jleach/openshift-components/master/cicd/azure-devops-agent/scripts/start.sh) for more details.

