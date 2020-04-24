# Alto API Documentation with Docker

Docker image to generate static API documentation using [Slate.](https://github.com/lord/slate)

If you just want to edit the documentation, just install your preferred text editor, and edit `index.html.md` file in `docs` folder 

Our Recommendation for text editor is [Visual Studio Code](https://code.visualstudio.com/download) with [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one) plugin.

If you want to live running the documentation in your local machine, please follow steps below.

## Install Docker

Go to [docker web site](https://www.docker.com/get-started), download docker installer and install it on your local machine.

## Getting Started

The idea is to use this Docker image like an application. Running the docker image will generate a folder with compiled html, javascipt and css for the documentation.

### Building the image

```
docker build -t biller-api-docs .
```

### Developing the doc

When writing the markdown documentation we would want to edit our markdown and then live preview the result. Assuming we have `docs` folder in current directory (example docs from Slate is attached in this repo):

```
docker run --rm -v $PWD/docs:/slate/source -p 4567:4567 -it biller-api-docs dev
```

Then, the preview of the published html can be viewed at [127.0.0.1:5467](http://127.0.0.1:5467)

### Publishing the doc

```
docker run --rm -v $PWD/docs:/slate/source biller-api-docs
```

The generated static html will appear in `docs/build/` folder in the host machine  
(Because of the volume mounting, you may need to reset the owner/group of `docs/build/`)