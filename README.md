# Secure LLM with Pomerium

This project is based off the instructions in Pomerium's [Self-Hosted LLM Behind Pomerium](https://bit.ly/4376Wki) guide.

## Prerequisites

- [Pomerium Zero account](https://bit.ly/4k7RIm5)
- [Docker](https://docs.docker.com/get-docker/) and[Docker Compose](https://docs.docker.com/compose/install/) or [Orbstack](https://orbstack.dev/)
- Port 443 open on your router or wherever you host this project.

## Configure Pomerium Zero

When you create a Pomerium Zero account, you will be taken to the Pomerium Zero console. Here you can create a new route for Open WebUI. There will already be one for the verify route which is just a page to show your claims in your JWT when logged in.

You'll also have one policy out of the box which will allow access to only you via the email you used to sign up with.

[Follow the steps in the docs](https://bit.ly/4376Wki#configure-pomerium-zero) to add a route for Open WebUI and a policy to allow access to it.


## Environment Configuration

To configure the environment for this project, you need to set up the `.env` file with the following variables:

```env
POMERIUM_ZERO_TOKEN=replace-with-your-pomerium-zero-token
WEBUI_URL=https://llm.yourdomain.pomerium.app # or whatever you called your route for Open WebUI in the Pomerium Zero console.
VERIFY_URL=https://verify.yourdomain.pomerium.app
GPU_DEVICE=/dev/dri/your-device # e.g. /dev/dri/renderD128
```

Make sure to replace the placeholder values with your actual configuration details. This setup is crucial for the proper functioning of the Pomerium Zero integration and GPU acceleration if applicable.

### GPU Acceleration (Optional)

If your machine has a GPU (e.g. AMD, Intel, or NVIDIA), you can enable GPU access for Open WebUI to accelerate model inference.To do so, uncomment the `devices` and `group_add` lines in the `docker-compose.yaml` and configure your GPU device in your `.env` file.

For example, on an AMD GPU (using `/dev/dri/renderD128`):

```env
GPU_DEVICE=/dev/dri/renderD128
```

Make sure your user has access to the GPU device (usually by being in the `video` group):

```bash
getent group video
```

**Note:** GPU acceleration requires that Open WebUI and Ollama support your GPU type. Currently, support for AMD and Intel GPUs may be limited or experimental.