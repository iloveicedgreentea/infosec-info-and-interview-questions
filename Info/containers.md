# Container Security

## General

### Multi-stage builds
Use one stage to build, another stage to run. For example (pseudocode):

```
FROM: go as builder
RUN make

FROM: scratch
RUN mkdir /app
COPY --from=builder /app /app
CMD ["/app/main"]
```

## Hardening

### Run updates
Always run updates as your first step. Well known images are filled with vulnerabilities. Throw python3.7 into Clair and see how many issues there are.

### Don't run as root
Guides on Docker ALWAYS get this wrong. If you follow them, and almost everyone does, your stuff will run as root. This is a security issue because of potential container escapes, volumes, filesystem permissions inside the container, etc. Would you run a web server as root?

Create a user as you would with a VM.

### Run inside your own directory
Create a app directory and chown it, and copy your files there (COPY --chown xyz:xyz)

### Run scratch if its a binary
If you compile a static binary, you don't need a full image. Combined with a multistage build, you can run the binary alone in a scratch image

`FROM: scratch`

You can also use [distroless](https://github.com/GoogleContainerTools/distroless) if your app needs some other libraries.

