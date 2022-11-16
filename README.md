# unison-hello-world
trying to make a unison server deployable via docker image (until someone gives me access to unison cloud).

The hello world unison app lives here: https://share.unison-lang.org/@tapegram/code/latest/namespaces/public/project/helloworldserver/;/terms/README

Able to build and run a container locally with this command:
`docker run -p80:80 <imageId>`

## Current Status

When you build the dockerfile, we run UCM with `compile-transcript.md`. This will

1) Fetch the code from the above project link

2) compile a `server.uc` file which will be used to run the container later.

3) Take 8+ minutes (!)

We are deploying the container to ECS, with help from ECR, by way of a github action that builds and pushes the image to ECR and then updates the task in ECS.

In order to actually trigger a deploy, something has to change in this repo. So for now I made a CHANGELOG.md file so you can trigger a deployment by adding to the changelog.

This repo is essentially an infrustructure repo managing the remote unison share code.

## Some painpoints

- It takes 8+ minutes to build the new docker images
- Doing this entire docker/aws nonsense defeats the whole points of unison (lol, unison cloud pls?)
- Using transcripts in the Dockerfile (and basically everything else im doing) feels very hacky
- Trigger deploys in this repo instead of via changes to the actual unison code is non-ideal.
- It looks like the actual hello world endpoint is kind of slow before it is cached by the browser. Maybe this is because I'm not putting enough resources into the task definition, but it will be interesting to figure out how to optimize performance of the web server.

## Other thoughts

- How would we want to include CI? Would I add transcripts for calling `test` and `io.test`?
- When running integration tests with unison, should I stick to trying to use test containers to provide dependencies?

## Steps

- ✅ Created a hello world http server with the [httpserver lib](https://share.unison-lang.org/@stew/code/latest/namespaces/public/projects/httpserver/latest/;/terms/README). Put a link to my implementation living on unison share

- ✅ Create a docker image that pulls the above branch and runs it. Probably using the unison base image created [here](https://hub.docker.com/layers/tavishpegram/unison/latest/images/sha256-45fabec64886ee28e819ef9910a7e615408f1f68861f9d6374d955e725b3c6f6?context=repo&tab=layers)

- ✅ Confirm I can talk to a unison web server living in a locally running docker container

- ✅ Deploy this container somewhere, maybe with something fancy like k8s/helm, and confirm that I can talk to the server.

- ✅ Figure out how CD works with this. Something lazy for now might be adding a trigger to replace pods with new instances whenever server.uc changes?
