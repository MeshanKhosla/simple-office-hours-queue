## The real SOHQ repo is [here](https://github.com/Berkeley-CS61B/simple-office-hours-queue). This one was created since I initially couldn't deploy from an organization account. That has been resolved so this repo is not needed.

# Simple Office Hours Queue

## What?

SOHQ is an open-source office hours queue that allows students to sign up for office hours and instructors to manage the queue.

For a brief usage guide, click [here](https://docs.google.com/presentation/d/1nPpjBQkf2LSRuIlFPolcVz9_iE3x1sNHgx0nXM867e4/edit?usp=sharing).

## Installation

### Prerequisites
- Git
- Node.js >= 14.7

Clone the reposity and `cd` into it.

For the `.env` setup, Refer to the `.env.example` file for the format of the required environment variables. The setup is as follows:

Run `cp .env.example .env` to create the `.env` file.

<details>
<summary>Ably</summary>

Ably is used for real-time communication (i.e. queue updates)

1. Create an account on <a target="_blank" href="https://ably.com">Ably</a>.

2. Create a new app.

3. Go to the "API Keys" tab. You should have 2 API keys, one for the server (top) and one for the client (bottom). Copy the server API key and paste it into the `ABLY_SERVER_API_KEY` variable in the `.env` file. Copy the client API key and paste it into the `NEXT_PUBLIC_ABLY_CLIENT_API_KEY` variable in the `.env` file. **Your client API key should have the `Subscribe` and `Publish` permission enabled.**
![Ably API Keys](/readme-assets/ably-config.jpg)

</details>

<details>
<summary>Google OAuth</summary>

Google OAuth is used for authentication.

1. Create a new project on <a target="_blank" href="https://console.developers.google.com">Google Cloud Platform</a>.

2. Navigate to the Credentials tab. Press the "Create Credentials" button and select "OAuth client ID". Go throuth the process of filling out the form. Select `External` if you're given the option. Press the "Create Credentials" button again. This time select "Web application" as the application type.

3. Create a new OAuth client ID. Make sure to set the Authorized JavaScript origins to `http://localhost` and redirect URI to `http://localhost:3000/api/auth/callback/google`. When deployed, add new entries, replacing `localhost`/`localhost:3000` to the new URL.
![Google Auth setup](/readme-assets/google-config.jpg)

4. Copy the client ID and paste it into the `GOOGLE_CLIENT_ID` variable in the `.env` file.

5. Copy the client secret and paste it into the `GOOGLE_CLIENT_SECRET` variable in the `.env` file.

</details>

<details>
<summary>Database</summary>

#### Local Development 
For local development we run a docker container serving a mySQL database.

1. Install [Docker](https://docs.docker.com/get-docker/) on your machine.

2. Start the container by running:
```
docker compose up
```

Note that if you are using an M1 Mac, you may need to change the docker image in `docker-compose.yaml` to 'arm64v8/mysql'. This is because the official image for MySQl, as of writing, does not currently support the ARM architecture.


#### Production



For the production environment, the free tier of <a href="https://planetscale.com">Planetscale</a> is very nice.

1. Create a new database on <a target="_blank" href="https://planetscale.com">Planetscale</a>.

2. Copy the database URL and paste it into the `DATABASE_URL` variable in the `.env` file. You can find this link by clicking on the database name and then clicking on the `Connect` button and select `Connect with Prisma`.

</details>

<details>
<summary>NextAuth</summary>

Run `openssl rand -base64 32` (you may need to download openssl) and put the result inside of `NEXTAUTH_SECRET`.

</details>

<br />


After your `.env` file is set up, run the following commands to install dependencies populate the database with the schema:

```bash
npm install
npx prisma db push
```

We need to populate the database with the default settings and first user. Go to `prisma/seed.ts` and change the `YOUR_EMAIL_ADDRESS` variable to your email. Then run:

```bash
npx prisma db seed
``` 

If you want to run the server in development mode (to see live changes), run the following command:

```bash
npm run dev
```

If you want to create a production build, run the following commands:

```bash
npm run build
npm run start
```

If you'd like to view or edit your database locally at any point, you can run

```bash
npx prisma studio
```


## Tech Stack

This project is built using the [T3 stack](https://github.com/t3-oss/create-t3-app), specifically these aspects:

- [Next-Auth.js](https://next-auth.js.org) for authentication (Google)
- [Prisma](https://prisma.io) for database management
- [tRPC](https://trpc.io) for API management
- [Chakra UI](https://chakra-ui.com) for UI components
- [MySQL](https://mysql.com) for database
- [Ably](https://ably.com) for real-time updates

## Contributing

If you'd like to contribute, please make a pull request. If you have any questions, feel free to open an issue.

[![Powered by Vercel](https://images.ctfassets.net/e5382hct74si/78Olo8EZRdUlcDUFQvnzG7/fa4cdb6dc04c40fceac194134788a0e2/1618983297-powered-by-vercel.svg)](https://vercel.com?utm_source=cs61b&utm_campaign=oss)
