# Lilypad Module Boiler Template

This is the basic template needed to create a module for the Lilypad network. 

For more information on building modules, see the docs on [build a job module](https://docs.lilypad.tech/lilypad/lilypad-milky-way-reference/build-a-job-module)

## `lilypad_module.json.tmpl`

```
{
  "machine": {
    "gpu": 0,
    "cpu": 1000,
    "ram": 100
  },
  "job": {
    "APIVersion": "V1beta1",
    "Spec": {
      "Deal": {
        "Concurrency": 1
      },
      "Docker": {
        "Entrypoint": [
          "/bin/sh",
          "-c",
          "/usr/app/run \"$MESSAGE\""
        ],
        "EnvironmentVariables": [
          {{ if .Message }}"{{ subst "MESSAGE=%s" .Message }}"{{else}}"Message=Hello World"{{ end }}
        ],
        "Image": "dockerhubuser/image:tagversion@sha256:hash"
      },
      "Engine": "Docker",
      "Network": {
        "Type": "None"
      },
      "PublisherSpec": {
        "Type": "IPFS"
      },
      "Resources": {
        "GPU": ""
      },
      "Timeout": 1800,
      "Verifier": "Noop"
    }
  }
}
```

## Components 

- **APIVersion:** Specifies the API version for the job.
- **Spec:** Contains the detailed job specifications:
- **Deal:** Sets the concurrency to 1, ensuring only one job instance runs at a time.
- **Docker:** Configures the Docker container for the job
- **Entrypoint:** Defines the command to be executed in the container, which sets up the `cowsay` command.
- **EnvironmentVariables:** This can be utilised to set env vars for the containers runtime, in the example above we use Go templating to set the MESSAGE variable dynamically from the CLI.
- **Image:** Specifies the image to be used (lilypadnetwork/lilysay:0.0.6), Its important to note to reference the SHA256 hash if you are using Docker. 
- **Engine:** In our example we are using Docker as the container runtime.
- **Network:** Specifies that the container does not require networking (Type: "None").
- **PublisherSpec:** Sets the method for publishing job results to IPFS.
- **Resources:** Indicates no additional GPU resources are needed (GPU: ""). As this is a very light job so does not need intensive processing. 
- **Timeout:** Sets the maximum duration for the job to 1800 seconds (30 minutes).
- **Verifier:** Specifies "Noop" as the verification method, meaning no verification is performed.
