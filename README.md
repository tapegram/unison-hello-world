# unison-hello-world
trying to make a unison server deployable via docker image (until someone gives me access to unison cloud).

The hello world unison app lives here: https://share.unison-lang.org/@tapegram/code/latest/namespaces/public/project/helloworldserver/;/terms/README

Compiling the server into `server.uc` -> can we deploy this via a dockerfile? 

Able to build and run a container locally with this command:
`docker run -p8081:8081 <imageId>`

## Current Status

When you build the dockerfile, we run UCM with `compile-transcript.md`. This will

1) Fetch the code from the above project link

2) compile a `server.uc` file which will be used to run the container later.

3) Take 8+ minutes (!)

## Steps
✅
⬜️
- ✅ Created a hello world http server with the [httpserver lib](https://share.unison-lang.org/@stew/code/latest/namespaces/public/projects/httpserver/latest/;/terms/README). Put a link to my implementation living on unison share

- ✅ Create a docker image that pulls the above branch and runs it. Probably using the unison base image created [here](https://hub.docker.com/layers/tavishpegram/unison/latest/images/sha256-45fabec64886ee28e819ef9910a7e615408f1f68861f9d6374d955e725b3c6f6?context=repo&tab=layers)

- ✅ Confirm I can talk to a unison web server living in a locally running docker container

- ⬜️ Deploy this container somewhere, maybe with something fancy like k8s/helm, and confirm that I can talk to the server.

- ⬜️ Figure out how CD works with this. Something lazy for now might be adding a trigger to replace pods with new instances whenever server.uc changes?
