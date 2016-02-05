# JHipster registry

This is the [JHipster](http://jhipster.github.io/) registry service, based on [Eureka](https://github.com/Netflix/eureka).

## Architecture

The JHipster microservices architecture is supposed to work in the following way:

- The gateway is a JHipster-generated application (using application type `gateway` when you generate it) that handles all Web traffic, and serves the AngularJS application
- The registry is a runtime application, using the usual JHipster structure, on which all applications registers
- Microservices are JHipster-generated application (using application type `microservice` when you generate them), that handle REST requests

When the gateway and the microservices are launched, they will register themselves in the registry (using the `eureka.client.serviceUrl.defaultZone` key in the `src/main/resources/config/application.yml` file.
The gateway will automatically proxy all requests to the microservices, using their application name: for example, when microservices `app1` is registered, it is available on the gateway on the `/app1` URL.

For example, if your gateway is running on `localhost:8080`, you could point to `http://localhost:8080/app1/rest/foos` to 
get the `foos` resource served by microservice `app1`. Of course, if you're trying to do it with your Web browser, don't forget REST resources are secured 
by default in JHipster, so you need to send the correct JWT header (see next point).

### Security considerations

For security to work, you need to exchange the JWT secret token between all your applications.

- The default token is unique, and generated by JHipster. It is stored in your `.yo-rc.json` file.
- Tokens are configured with the `jhipster.security.authentication.jwt.secret` key in your `src/main/resources/config/application.yml` file.
- To share this key between all your applications, copy the key from your gateway to all the microservices.
- A good practice is to have a different key in development and production: you could have different keys in your `application-dev.yml` and `application-prod.yml` files, or use a specific environment variable in production.

## Running the registry

To run the application, like any JHipster application, just launch:

    mvn

The registry Web console should be available at [http://127.0.0.1:8761/](http://127.0.0.1:8761/).

## Tips

### Avoid port conflicts

If you run several JHipster microservices on your machine, don't forget to change their default port (8081) by modifying the
`server.port` key in their `src/main/resources/config/application.yml` file.


curl 'http://127.0.0.1:8080/api/foos?cacheBuster=1454682734916' -H 'Pragma: no-cache' -H 'Accept-Encoding: gzip, deflate, sdch' -H 'Accept-Language: fr-FR,fr;q=0.8,en-US;q=0.6,en;q=0.4' -H 'Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImF1dGgiOiJST0xFX0FETUlOLFJPTEVfVVNFUiIsImV4cCI6MTQ1NDc2ODY3OH0.JUp-v33vFPrf0Lb2R6ja8UuEc1fkfz8WG7f4CunUCGIxwPWHXwyG1XcR_0FzqjeTrjcROJIiFQSPGclehfHyWw' -H 'Accept: application/json, text/plain, */*' -H 'User-Agent: Mozilla/5.0' -H 'Connection: keep-alive' -H 'Cache-Control: no-cache' --compressed

## More information

[JHipster]: https://jhipster.github.io/
