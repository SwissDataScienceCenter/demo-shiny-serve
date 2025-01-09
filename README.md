# Shiny Serving Demo

This is a demo project that shows how to serve a Shiny app from a Renku V2 project. It uses the [rocker shiny](https://rocker-project.org/images/versioned/shiny.html) Docker base image and makes some modifications necessary for compatibility with Renku sessions.

You can see it in action at https://renkulab.io/v2/projects/cramakri/shiny-demo.

## Serving your own shiny app

This project serves a "hello-world" Shiny example app. To serve your own app, you can clone this repo, add your app, and then configure a Renku V2 session launcher.

### Adding your app

Once you have cloned this repo, you can replace the code in the `src/r/app` folder with your app.

After doing that, you should build the image and test that the app works. This requires Docker installed on your machine, but it will help you debug problems quickly.

For example, you can build and run with the following commands:

```
docker buildx build -t renku/shiny-session --platform linux/amd64 .
docker run --rm -ti -e RENKU_BASE_URL_PATH="/test-app" -p 3838:3838 renku/shiny-session
```

If everything worked correctly, you should be able to connect a web browser to

http://localhost:3838/test-app/

And you should see your app there.


### Renku session launcher

Once your app runs locally, you can serve it from Renku. To do this, you will need to create a session launcher with the following pieces of information:

- the container image URL
- the port
- UID and GID for the container

This repo is configured to build a Docker image. You should see a section called **Packages** in the right-hand side of the page for this repo. Click there to get the URL for the image (you will probably want to most recent one).

It will look something like: `ghcr.io/swissdatasciencecenter/demo-shiny-serve:sha-1478fc5`

The other pieces of information are fixed:

- Port: 3838
- UID: 997 (the `shiny` user in the Rocker image)
- GID: 997

With that, you should have a session launcher that serves your Shiny app from Renku!

## References

This project builds on The Rocker Shiny Docker image, which is designed to run the Shiny Server. Here are links to documentation for the Rocker image and Shiny Server. You may find them helpful for more advanced use cases.

- https://rocker-project.org/images/versioned/shiny.html
- https://docs.posit.co/shiny-server/
- https://github.com/rstudio/shiny-server/
