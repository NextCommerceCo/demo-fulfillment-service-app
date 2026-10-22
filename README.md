# Fulfillment Service App Demo

This app demonstrates the core concepts of a Next Commerce [Fulfillment Service App](https://developers.nextcommerce.com/docs/apps/guides/fulfillment-service) that processes order fulfillments for a store using the [Admin API](https://developers.nextcommerce.com/docs/admin-api) fulfillment endpoints. It targets Admin API version `2024-04-01`, set by `STORE_API_VERSION` in `app/app/settings.py`.

### Features

* [Creating Fulfillment Locations](https://developers.nextcommerce.com/docs/apps/guides/fulfillment-service#fulfillment-locations) - See the example [location create API request wrapper](https://github.com/NextCommerceCo/demo-fulfillment-service-app/blob/54e5a17b1d74ed0bd06f548f8446366f2b501536/app/stores/api/admin.py#L43).
* [Accepting Fulfillment Requests](https://developers.nextcommerce.com/docs/apps/guides/fulfillment-service#accepting-fulfillment-requests) - See the example [fulfillment request receiver](https://github.com/NextCommerceCo/demo-fulfillment-service-app/blob/54e5a17b1d74ed0bd06f548f8446366f2b501536/app/fulfillments/views.py#L25).
* [Rejecting Fulfillment Requests](https://developers.nextcommerce.com/docs/apps/guides/fulfillment-service#rejecting-fulfillment-requests) - See the example [fulfillment request receiver](https://github.com/NextCommerceCo/demo-fulfillment-service-app/blob/54e5a17b1d74ed0bd06f548f8446366f2b501536/app/fulfillments/views.py#L25).
* [Accepting Cancellation Requests](https://developers.nextcommerce.com/docs/apps/guides/fulfillment-service#accepting-cancellation-requests) - See the example [fulfillment request receiver](https://github.com/NextCommerceCo/demo-fulfillment-service-app/blob/54e5a17b1d74ed0bd06f548f8446366f2b501536/app/fulfillments/views.py#L25).
* [Rejecting Cancellation Requests](https://developers.nextcommerce.com/docs/apps/guides/fulfillment-service#rejecting-cancellation-requests) - See the example [fulfillment request receiver](https://github.com/NextCommerceCo/demo-fulfillment-service-app/blob/54e5a17b1d74ed0bd06f548f8446366f2b501536/app/fulfillments/views.py#L25).
* [Creating Fulfillments](https://developers.nextcommerce.com/docs/apps/guides/fulfillment-service#creating-fulfillments) - See the example [create fulfillment API wrapper](https://github.com/NextCommerceCo/demo-fulfillment-service-app/blob/54e5a17b1d74ed0bd06f548f8446366f2b501536/app/stores/api/admin.py#L96).
* [App OAuth Install Flow](https://developers.nextcommerce.com/docs/apps/oauth) - See the [store auth setup flow handler](https://github.com/NextCommerceCo/demo-fulfillment-service-app/blob/54e5a17b1d74ed0bd06f548f8446366f2b501536/app/stores/views.py#L20).

### Screenshots
|![image](https://github.com/NextCommerceCo/demo-fulfillment-service-app/assets/674282/f3bc695e-6947-4aea-91dc-de28b5351e67)|![image](https://github.com/NextCommerceCo/demo-fulfillment-service-app/assets/674282/085ed071-a124-4da8-b850-1b2e25d269da)|
|:-------------------------:|:-------------------------:|
|![image](https://github.com/NextCommerceCo/demo-fulfillment-service-app/assets/674282/6cb4c679-1991-4211-9069-056177dd7bba)|![image](https://github.com/NextCommerceCo/demo-fulfillment-service-app/assets/674282/0ddf1252-f211-41fa-8675-f37d5c8846a8)|


### How to Setup

#### Create App In Partner Account
The first step is to create an [App](https://developers.nextcommerce.com/docs/apps) in your [Next Commerce Partner Account](https://accounts.nextcommerce.com/partners/).

Take note of your App `Client ID` and `Client Secret`. The [OAuth install flow](https://developers.nextcommerce.com/docs/apps/oauth) needs them, and you'll enter them when configuring the app.

#### Install Docker & Docker Compose

The demo app uses Docker and Docker Compose to set up a full development environment with Django, Postgres, Celery, and RabbitMQ.

##### Option 1 - Install Docker Desktop

- Install [Docker Desktop](https://www.docker.com/products/docker-desktop/)

##### Option 2 - Manual Install

- Install [Docker](https://docs.docker.com/engine/)
- Install [Docker Compose](https://docs.docker.com/compose/)


#### Setup Public Tunnel

To access your localhost for app development, you can use [Ngrok](https://ngrok.com/) or [LocalTunnel](https://localtunnel.github.io/www/) to create and open a public tunnel to your local machine.


Your tunnel should expose port `3000`, take note of your public domain for the next step.


#### Update Your App OAuth Settings

Update your App OAuth **Redirect URLs** to match your local tunnel domain.

```
https://<YOUR LOCAL TUNNEL DOMAIN>/stores/auth/setup/
```


### Configure Your Application

To run the app, we need to configure it to use your local tunnel domain and OAuth credentials.

```bash
source setup.sh
```

This script writes the environment variables to the `app.env` file.


#### Run Application

To run the application on your local, use the following command.

In a new terminal

```bash
docker compose up
```

You should now be able to access the app on your tunnel domain and locally at `localhost:3000`.

#### Migrate Database & Create Super User

With your application now running, you need to migrate the database and create a super user.

In a new terminal, shell into your running django container

```bash
docker compose exec app bash
```

Once inside the container, run this command to migrate the database and create a superuser.

```bash
python manage.py migrate && python manage.py createsuperuser
```

The full Django admin is available at `http://localhost:3000/admin/`

#### Install on Development Store

You can now connect your app to your development store which starts the OAuth setup flow and configures Admin API access. :tada:

