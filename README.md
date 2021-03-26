# Sellout Public General Documentation

## General Tech Stack and Tools
- React
  - React is a frontend library that Sellout uses to help build UI's and components in both the Backstage and Purchase-portal packages. Sellout uses React with Typescript to creat TSX components with typed props, state, variables, etc.
- Create-react-app
  - Create-react-app is a toolchain created by Facebook that assists Sellout in bootstrapping react apps to more quickly and easily get to the app development. It handles webpack configurations and provide optimizations for production builds. Create-react-app is used by both Backstage and Purchase-portal, however, purchase-portal was ejected from create-react-app to gain access to the webpack config file and make changes to inject the iframe it exists in into other websites.
- React-Native
  - React Native is a JavaScript framework similar to React that is used for writing real, natively rendered mobile applications for both iOS and Android. Sellout uses React Native with Javascript. The plan was to migrate to Typescript in the future but that goal was never reached. React Native is only used by Sellout in the Ticket-Scanning app that allows event staff to scan QR codes and admit event attendants.
- Styled Components
  - Sellout uses Styled Components to write CSS in .tsx files in a CSS-In-TS manner to more succinctly create beautiful components.
- MJML
  - MJML is a Node library used for creating stylized emails. It handles a lot of the variability in different screen sizes and is much easier to write than just plain css and html. When it is built, the MJML code is transpiled into html and css which is what is rendered in an email client. We also use a package called handlebars to inject variables into each email when they are created and sent.
- Sendgrid
  - Sendgrid is Sellout's email delivery service. Sellout sends the email html to Sendgrid and Sendgrid handles the distribution. Their are several other more minor toosl Sellouts uses in conjunction with MJML and Sendgrid to handle emails, all of which can be seen in the `email` microservice.
- Full stack Typescript
  - Sellout uses Typescript on both the front and back end. This enables code to be shared by all packages and is easier to write when switching between the front and back end frequently. The shared code exists in the `common` directory and consists of enums, utility functions, graphql queries, components, interfaces, types, models, etc.
- Apollo GraphQL
  - Apollo Graphql is used to efficiently write and handle queries and mutations for CRUD operations in the Sellout platform. Apollo caching is used on the front end to prevent unneccessary data reloads and the backend supports querying different data models all at once. Graphql was chosen as it avoids a lot of the common pitfalls with regular REST APIS.
- ExpressJS
  - ExpressJS (but in Typescript) is the web framework used by the Sellout platform to handle http requests. It is initialized on the backend in the `server.ts` file located in `graphql/src/`.
- Stripe
  - Stripe is the payment processer used by Sellout to handle payment functions such as card charges, refunds, client bank transfers, etc. When a promoter creates a Sellout Backstage organization. They need to connect their Stripe account so that they can accept payment from the people buying tickets to their events. This is done with Stripe Connect. Sellout previously used the `Stripe Charges API` to accept payment but has since moved to the `Stripe Payment Intents API`. To keep up with newer regulations. The refunds integration still needs to be written to support both a charge refund and payment intent refund. Stripe terminal is also used to accept payment from a physical card at the box office and send the data to the Sellout platform.
- Twilio/Plivo
  - Twilio is the current texting integration for authentication purposes in the Sellout v2 platform. However, Sellout switched to Plivo in the new version as it was cheaper and allowed for more flexibility in what was sent. The plan now, is to use Plivo for marketing texts, QR code texts, event reminders, etc, and switch to Firebase texts for authentication in Purchase-portal checkout and Backstage account creation. Firebase allows for 10,000 free texts a month, adds a recaptcha for added security, and does not rate limit as bad. The Plivo integration should be completed for the upcoming release but the Firebase authentication is not fully finished. A working, but not cleaned up, version is present within the `feature/firebase-phone-auth` branch. It ws not built in the traditional Firebase way, so pay attention to what API's are being called and how things are being stored.
- Mapbox
  - Mapbox is used in backstage to provide a cool visualization heat map for where ticket buyers are purchasing their tickets based off of IP addresses fed into GeoJSON data.
- IPstack
  - IPStack is used in the Purchase-portal to record the IP addresses of where people are buying tickets from. This data is then used to populate the heatmap and is stored for other analytics.
- D3
  - D3 is an SVG rendering library that Sellout uses to create complex and interactive custom analytics graphs for ticket sale metrics in the Backstage frontend.
- NPM
  - NPM is the package manager for Node used in every Sellout package to handle installations and versioning of open-source and third-party tools. Sellout also maintains its own NPM packages, the code in `common` to easily publish, install, and share in-house code with every Sellout package.
- Lerna
  - Lerna is monorepo management tools and enables Sellout to more effectively manage each package that the system consists of. It enables command to be run in every package and provides the ability to write scripts for things such as publishing the `common` code to NPM and updating the package version in every package that uses them.
- Intercom
  - Intercom is a tool that Sellout uses in Backstage and on the marketing site. It is a saas platform that allows customers to directly contact Sellout's customer support team through their web browser without leaving the Sellout app.
- Redux
  - Sellout uses Redux for client side caching and global state management in both Purchase-portal and Backstage.
- Redux-Saga
  - Redux-Saga is a library used in conjuction with Redux that aims to make application side effects (i.e. asynchronous things like data fetching and impure things like accessing the browser cache) easier to manage, more efficient to execute, easy to test, and better at handling failures. Sellout uses Redux-Saga in both Backstage and Purchase-portal.
- JWT
  - Sellout currently uses a simple JWT authentication for logins where the JWT's are stored in the client's local storage and are used to authenticate and identify each request to the backend. There is currently no expiration time for the tokens and we have an auto login feature set up if the JWT in local storage is legitimate. The userId and organizationId are also stored in the token if they are required.
- MongoDB
  - MongoDB is the NoSQL database used by Sellout to store all of the documents required for the Sellout platform. The main data models are:
    - Artist: Stored data for artists created in Backstage.
    - Event: Stored data for events created in the Backstage event creator.
    - Fee: Stored fee data set by event promoters and Sellout. Used to determine what to charge customers.
    - Order: Stored data for each order placed through the Sellout platform.
    - Organization: Stored info for organizations created in backstage
    - Role: The role and permission level data for members of a backstage organization.
    - Seating: Stored seating data for charts created with the SeatsIO integration.
    - User: Stored data for all users on the sellout platform, event promoter, event staff, or ticket buyer.
    - Venue: Stored data for venues created in Backstage.
    - WebFlow: Info for client's webflow site integrations.
    - UserProfile: A mix between the user and organization.
    - Task: Sellout has a task system, kind of like a cron job, that runs every 30 seconds and executes any tasks that needs to be executed at that time such as sending out QR codes. The data on task type and when to execute is stored here.
- Mongoose
  - Mongoose is the Node MongoDB client that Sellout uses to facilitate CRUD operations with the database as well as create and initialize the database schemas in the correct format.
- NodeJS
  - Sellout API's and infrastructure is written in NodeJS with Typescript.
- Protobuf
  - Protobuffers are extremely lightweight (smaller than JSON and XML) and perfect for transporting messages between services. Sellout uses them for internal communcation via NATS, a message-bus similar to Kafka, to allow the microservices in the platform to quickly and efficiently communicate.
- NATS
  - NATS is used to send messages between each microservice in the Sellout platform.
- Google Cloud
  - Google cloud is the cloud computing solution used for building and hosting the different Sellout environments. Things like the Cloud Build, Kubernetes engine, Analytics, etc. are all utilized.
- Docker
  - Each of the packages in the Sellout system runs its own Docker container. When changes are made to a package and pushed, the package is containerized and uploaded to Google Cloud Registry for storage. It is then deployed to Kubernetes via Helm. Each version of the containers is kept long-term as an artifact, allowing Sellout to rollback to specific versions of each service if necessary.
- Kubernetes
  - Kubernetes is production-grade container orchestration. Kubernetes facilitates the networking of multiple computers into a computer cluster, allowing Sellout to run many computers in parallel. Kubernetes defines a set of usable abstractions to make scaling software applications vertically and horizontally in the cloud easy. Kubernetes and Docker work hand in hand. Docker abstracts the runtime environment of the application and Kubernetes abstracts the physical compute power and networking protocols required to run the Docker container. Each service in the system uses Kubernetes application, networking, and storage abstractions to run and communicate with other parts of the system.
- Nginx
  - Nginx is used as the web server and handles things such as load balancing.
- Helm
  - Helm is the Kubernetes package manager. Sellout uses Helm to facillitate the packaging of application components into Helm Charts, which can be deployed to Kubernetes with a single command. A Helm Chart contains everything Kubernetes needs to run a single service, including information on the Docker container to run, how it should be run, and what other services are relied on. Each service has it's own Helm Chart.
- Sentry
  - Sentry provides Sellout with a way to see crash reports in the production and staging environments as well as analytics on the crash such as IP address, browser type, error message, etc. Sellout also has a Slack bot that sends an alert when there are crashes in production
- Prometheus
  - Prometheus allows Sellout to collect metrics on CPU usage and memory utilization on a per container basis to ensure the hardware for an application is performing correctly.
- Grafana
  - Grafana is a time-series visualization tool that works with Prometheus to make the gathered metrics easy to interpret. 
- Jaegar
  - Tracing allows developers to track a request all the way through the system from start to finish and measure how each subcomponent of the system behaves. Distributed architectures add the complexity of tracing requests between processes boundaries. To solve this, Sellout uses [Jaeger](https://www.jaegertracing.io/). Jaeger facilitates transaction monitoring, the measurement of several types of latency, and the tracking of requests between services, all in real-time. Jaeger comes with a monitoring UI to easily interpret the gathered data. Each service implements Jaeger tracing protocols via the Jaeger client, which simply means that when services make or receive requests, they will include context information that will be reported to the Jaeger backend.
- ELK stack logging
  - ElasticSearch, Logstash, and Kibana make up the ELK stack, a solution to distributed log aggregation, storage, and search.

If you want to more specifically see what tools Sellout uses in a technical way, check out the `package.json` files in the different Sellout packages and see how they are used.

## Sellout Development Team Shake Up and Moving Forward
As of right now, the current development team at Sellout is moving on to other projects and the rest of the Sellout team is looking for people to finish off the new version of the platform and push it to production. The current master branch is what currently exists in production at [app.sellout.io](https://app.sellout.io/) and the new unreleased version exists on the branch `feature/lerna`. There are several things that need to happen before this branch can be pushed to production for Sellout users to use. They include but are not limited to:
- Publish the reskinned ticket scanning app in the IOS and Play store.
- Provide a frontend for refund metrics and functionality.
- Update the backend to support refunds for both Stripe Charges and Stripe Payment Intents.
- Redesign the emails
- Merge in the `feature/firebase-phone-auth` branch and clean up as well integrate front end code in the purhcase portal ui.
- Fully fix package versioning issue. There was a bug where the builds in google cloud were not building properly.
- Test app on various browsers with full funcitonality QA and fix issues.
- Fix seating bug where multiple seats were created for items such as a seated table.
- Issues with the 'no-permission' page flashing.
- QA the app with Joel Martin (Head of Product) and fix issues.
- Fix pagination issues.
- Create scanned in cards.
- Set up deployment stuff for the lerna branch to be ready for when it is pushed to production.
- Add QR code scanning functionality in backstage.

Other Items that may not be needed to release but will still be probably needed going forward are:
- Add global linting to the entire Sellout Platform as well as an auto formatter.
- Write Unit and integration tests.

We also maintain a [Notion](https://www.notion.so/) board with more detailed and specific items of what is left to do before this new platform can be released.

#### Differences between the current production app (V1) and the new version (V2)
  - V2 is a monorepo while V1 is not. V2 contains the frontend code in `backstage` and `purchase-portal` inside the repo `SelloutPlatform` while the V1 frontend code is in the separate repositories: `SelloutUserUI`, `SelloutAdminUI`, and `SelloutCheckoutUI`. `backtage` is the new equivalent of `SelloutAdminUI`, `purchase-portal` is the new equivalent of `SelloutCheckoutUI`, and `SelloutUserUI` currently does not have anything similar in the new version. It was a simple user web app that allowed people to view their tickets.
  - All of the frontend code in the above mentioned V1 repos is deprecated and not in use in the new platform. While there are a lot of changes in the API code, a lot of the same code is still in use in the new version.
  - The frontends for V1 were mostly written in class-component javascript React while in V2 everything is written with react-hooks, functional components, and Typescript.
  - The design in the new version is very different from the design in the old version.
  - `Stripe Payment-Intents` are used to process payment in V2 while `Stripe Charges`, a deprecated API, is used in V1.
  - V2 uses Redux and superior caching methods in `backstage` while `SelloutAdminUI` uses the Apollo Client Cache.

## Organization Repositories

The `SelloutPlatform` repository contains most of the front and backend code and uses [Lerna](https://github.com/lerna/lerna) to make the management of every package easier within this single repository. This is the repository that contains the `feature/lerna` branch.

The `SelloutPromoterMobile` repository contains the React-Native code that allows promoters to scan QR codes and admit customers into an event. This app has been published on both the IOS and Play stores for users to download onto their phone. There is a unreleased, reskinned version that should be published to the respective stores.

The `SelloutOps` repository contains the code responsible for observability and deployment. There is a build trigger that gets run when pushing to the SelloutPlatform that updates the live staging environment but to push to production, you will have to manually update the `image-tags.yaml` file with the correct commit hash.

The other repositories are either deprecated or serve some other function not directly tied to an application.


## Getting Started

You must have NPM, NodeJS, and Lerna installed to run the Sellout system. Follow the steps below to get started

1. Acquire all of the needed permissions and install the above dependencies.
  - You will need access to things such as google cloud, API keys, and secrets.
3. Create the required directory structure, install and link service dependencies with:
   ```
   $ cd $SELLOUT_SRC/SelloutPlatform && lerna bootstrap
   ```
3. Start the platform with:
   ```
   $ make start
   ```
   or, if you just want to run things locally and not use kubectl to port forward to the staging clusters, run
   
   ```
   $ make start-local
   ```

This last command will start the web clients, the microservices, and build the `common` packages as well as watch for changes when they are made. After a minute or so, the platform should be fully running. There are also several other commands in the `Makefile` that perform various tasks such as cluster scaling or connecting to a clustered Mongo instance. Everything should work on both MacOS and Ubuntu. There may be additonal configuration required if not using either of these OS's.

## Common

The platform shares a decent amount of code and this code is all located in the `common` folder which contains NPM packages that are installed and used by both frontend and backend platforms. Here you will find all the protobuf definitions, icons, component libraries, the base service defintion, TypeScript interfaces and models, and some utility code.

### Publishing common

When the files in the common directory are updated, you must publish these changes before you push your changes to GitHub. In order to do this, commit your existing changes, but do not push them. In the root the `SelloutPlatform` directory, run `lerna publish`. This will publish the changes made to common to NPM and will update all the services with the new version. This will auotmatically trigger a a commit and push the commits to github with the updated version.

## System Architecture

This system is built using Microservice Oriented Architecture (MSOA).

#### Internal Communication
Services communicate internally via [NATS](https://nats.io/) and [protobuffers](https://developers.google.com/protocol-buffers/). Each service subscribes to message topics they are interested in processing. Before publishing a message, the service encodes the request body as a protobuffer. Protobuffers are extremely lightweight (smaller than JSON and XML) and perfect for transporting messages between services. When a service publishes a message, NATS routes the message to the correct processor. The receiving service then processes the message and reports back to the requesting service the results of processing the request. This is an asynchronous [request/response pattern](https://en.wikipedia.org/wiki/Request%E2%80%93response) implemented via [publish/subscribe](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern) mechanisms. This implementation allows all services to provide the durability of the request/response paradigm while also allowing the flexibility to drop down to the publish/subscribe layer for activities that require it (such as broadcasting via [WebSockets](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API), [HTTP Server Push](https://en.wikipedia.org/wiki/HTTP/2_Server_Push), etc) with minimal changes to the system and without special casing.

#### External Communication
Services communicate externally via GraphQL. This is the client application's entrypoint into the system. GraphQL solves several common problems with REST API's including [overfetching](https://stackoverflow.com/questions/44564905/what-is-over-fetching-or-under-fetching) and underfetching, the [N+1 problem](https://restfulapi.net/rest-api-n-1-problem/), and also provides a declarative [type system](https://graphql.org/learn/schema/) which simplifies writing queries. GraphQL will run as a service in the system. When a request is received, GraphQL will attempt to resolve the request based on a set of rules that define the available queries and by which service they can be resolved. This involves dispatching a request to the specified service, modifying the results to match the requested format, and sending the results back to the client. An external broadcasting system will likely be implemented in the future to provide real-time updates to live data.

#### Scaling
Scaling is a first class citizen in this system. The two primary scaling techniques used are Pod Autoscaling and Cluster Autoscaling. Pod Autoscaling facilitates the automatic deployment of additional Kuberentes Pods when the existing pods are above maximum capacity. Cluster Autoscaling comes in once the entire system is under maximum capacity and there are no resources to allocate new Pods. Cluster Autoscaling solves this problem by adding new nodes (computers) to the cluster, thus raising the total amount of compute power available. Cluster Autoscaling can also save money by remove nodes from the cluster when they are no longer needed.


## Packages

#### Backstage
Backstage is the web client for promoters and event staff to create and manage events.

#### Purchase-portal
Purchase Portal is how customers can purchase tickets to an event. We use an Iframe to be able to embed this site into any other site so promoters can have a 'Buy Tickets' button that never redirects customers off of the promoter website.

#### Mobile Client
The mobile client is built using [React Native](https://facebook.github.io/react-native/) with Javascript and functions as a way to allow event staff to stan QR codes on tickets to admit people into the event.

#### Marketing site and event hosting
The marketing site and event hosting is done via a Webflow site. We have a Wwebflow integration built to be able to push a JSON object to webflow that can then be rendered accordingly for customers to click on and pop up the purchase portal.

## Packages in the `SelloutPlatform` repo on the on the `feature/lerna` branch

#### backstage
This folder contains the Reactjs code for the backstage application that event managers and promoters use to create events. It is our biggest front end application.

#### common
This directory contains the four packages `models`, `service`, `ui`, and `utils`. This is the code that is shared between all of our different applications and can be installed via NPM.

#### deploy
This folder is deprecated and can probably be deleted. DevOps stuff is now handled in the `SelloutOps` repository.

#### email
This directory handles all the logic around sending the different emails such as ticket confirmations or qr codes. It also contains the MJML templates which are the templates that are responsible for the design of the email itself.

#### file-upload
This module handles the file-upload logic for storing files on Google cloud. Currently, the only items uploaded are images such as QR codes, Event posters, venue posters, etc.

#### graphql
This folder contains the main `server.ts` file that starts the Expressjs server and applies the required middleware. It also contains the GraphQL schema in `schema.ts` which defines the types and behavior of API requests. The GraphQL resolvers for each microservice are also stored here where they provide a callable endpoint with security checks.

#### plivo
This directory is responsible for interacting with Plivo. Plivo is a company/tool that we planned on using for text message marketing and ticket sending.

#### purchase-portal
This directory contains the embeddable iframe that is used at the box office and on Promoter/Sellout websites to facilitate the purchasing of event tickets.

#### scripts
This folder is deprecated and can probably be deleted. Lerna handles most of our scripting requirements.

#### seating
This directory is responsible for interacting with SeatsIO as well as the seating logic and the storing of seating information for a venue. SeatsIO is a platform that provides a GUI for 'easily' creating sophisticated seating charts that users can select and reserve when purchasing tickets.

#### stripe
The Stripe module is used for processing payments from users into the bank accounts of Sellout and the promoters who promote on Sellout. It is also used for refunds, and PoS stuff.

#### task-queue
The task system in Sellout runs every 10 seconds and checks the stored task 'todos' to see if any of them need to be executed. This is stuff such as delayed ticket sending. The task system can be used to automatically execute any future processes and do not require user interaction.

#### tools
This folder is deprecated but contained some code needed to backfill the database as well as automatically run some load testing by creating thousands of orders a second via a script.

#### web-flow
The current Sellout event hosting site where people go to click on events and buy tickets was created with web-flow and is hosted on their platform. This is where the event JSON object is pushed to webflow in order to render it correctly on the website. The plan was to eventually get rid of the web-flow integration and build custom react sites for better SEO and customization.

#### event, fee, artist, order, organization, user-profile, user, venue, role
These directories contain the code that handles the interactions with the corresponding data models as well as the business logic based around each item. For example, `event` handles all CRUD operations revolving around events. `EventService.ts` is responsible for the bulk of the business logic and interactions with the rest of the app while `EventStore.ts` is used to directly interact with the database through mongoose.

## Other Notes
The Sellout production Backstage environment lives at https://app.sellout.io/ and Purchase-portal lives at https://embed.sellout.io/?eventId=theeventid

The Sellout staging Backstage environment lives at https://app.sellout.cool/ and Purchase-portal lives at https://embed.sellout.cool/?eventId=theeventid

It might be easier to deploy and observe the platform in another, more simple way, depending on what the next devs have experience with.

