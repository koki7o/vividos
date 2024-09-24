# Installing an image based on VividOS

## Fresh install from an ISO

You can use JasonN3’s [build-container-installer](https://github.com/JasonN3/build-container-installer) to generate an offline ISO of your image locally. See the project’s README for more information it and its configuration. This procedure only requires either ```podman``` or ```docker``` be installed.

Here's how to generate the ISO:

- With ```podman```:
  ```
  mkdir ./iso-output
  sudo podman run --rm --privileged --volume ./iso-output:/build-container-installer/build --security-opt label=disable --pull=newer \
  ghcr.io/jasonn3/build-container-installer:latest \
  IMAGE_REPO=ghcr.io/koki7o \
  IMAGE_NAME=vividos \
  IMAGE_TAG=latest \
  VARIANT=Silverblue # should match the variant your image is based on
  ```

- With ```docker```:
  ```
  mkdir ./iso-output
  sudo docker run --rm --privileged --volume ./iso-output:/build-container-installer/build --pull=always \
  ghcr.io/jasonn3/build-container-installer:latest \
  IMAGE_REPO=ghcr.io/koki7o \
  IMAGE_NAME=vividos \
  IMAGE_TAG=latest \
  VARIANT=Silverblue # should match the variant your image is based on
  ```
   
## By rebasing from an existing installation of Fedora atomic (or a derivative)

If you have already installed an atomic Fedora version or something derivative such as an Universal Blue image, it is possible to switch to another image such as your custom image by just running the commands below:

1. Rebase to an unsigned image to get the proper signing keys and policies installed:

```
IMAGE_PATH=ghcr.io/koki7o/vividos
IMAGE_TAG=latest
rpm-ostree rebase ostree-unverified-registry:$IMAGE_PATH:$IMAGE_TAG
systemctl reboot
```

2. Rebase to a signed image to complete the installation:

```
IMAGE_PATH=ghcr.io/koki7o/vividos
IMAGE_TAG=latest
rpm-ostree rebase ostree-image-signed:docker://$IMAGE_PATH:$IMAGE_TAG
systemctl reboot
```
