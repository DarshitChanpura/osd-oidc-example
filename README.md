# OpenSearch using OpenID Connect backend with Keycloak

## Run and configure client in Keycloak

Before running OpenSearch you must run and configure a client in Keycloak.

There are 2 `docker-compose` files in this repository, for running keycloak navigate to the `keycloak/` folder and run:

```
docker-compose -f docker-compose-keycloak.yml up
```

This will run a PostgreSQL database on port 5432 and keycloak on localhost:8080.

Once the containers have started, navigate to `http://localhost:8080/auth` in a web browser then click on `Administration Console` and login with:

```
Username: admin
Password: Pa55w0rd
```

### Creating a client

Once logged in you need to create a `Client`. 

1. Click on `Clients` on the left menu
2. Click on `Create client`
3. Type in `Client ID: opensearch-dashboards-sso` with name `OpenSearch Dashboards SSO`
4. `Next`
5. `Save`
6. In `Valid redirect URIs` enter: `http://localhost:5601/auth/openid/login`
7. In `Valid post logout redirect URIs` enter: `http://localhost:5601`
8. `Save`

### Create a user

1. Click on `Users` on the left menu
2. Click on `Add user`
3. Fill out the form to create a new user (`user1` for an example username)
4. Click on `Create`
5. On the next screen (`User details` in the breadcrumbs) click on the `Credentials` tab and give the user a password. 
6. Go back to the `Details` tab and click `Save`

## Start OpenSearch and OpenSearch-Dashboards

In the root level of this repository run:

`docker-compose up`

Wait for startup and then navigate to `https://localhost:5601`

Login with the user created above

## Mapping user to backend roles

After completing the steps above the user is able to login/logout, but only has access to `own_index`.

In order to extract backend roles from keycloak, there needs to be additional configuration on the client to ensure that keycloak is populating a field that matches the `roles_key` that is configured in the `authc` section of the security plugin's `config.yml` file.

1. Click on `Users` on the left menu
2. Click on desired user
3. Navigate to `Role mapping` tab
4. Click on `Assign role` and map the user to the desired backend role
5. You can create new roles in the `Realm roles` section of the left menu


