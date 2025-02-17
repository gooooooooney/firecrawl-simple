# Contributors guide:

Welcome to [Firecrawl](https://firecrawl.dev) 🔥! Here are some instructions on how to get the project locally, so you can run it on your own (and contribute)

If you're contributing, note that the process is similar to other open source repos i.e. (fork firecrawl, make changes, run tests, PR). If you have any questions, and would like help gettin on board, reach out to hello@mendable.ai for more or submit an issue!

## Running the project locally

First, start by installing dependencies

1. node.js [instructions](https://nodejs.org/en/learn/getting-started/how-to-install-nodejs)
2. pnpm [instructions](https://pnpm.io/installation)
3. redis [instructions](https://redis.io/docs/latest/operate/oss_and_stack/install/install-redis/)

Set environment variables in a .env in the /apps/api/ directory you can copy over the template in .env.example.

To start, we wont set up authentication, or any optional sub services (pdf parsing, JS blocking support, AI features )

.env:

```
# ===== Required ENVS ======
NUM_WORKERS_PER_QUEUE=8
PORT=3002
HOST=0.0.0.0
REDIS_URL=redis://localhost:6379
REDIS_RATE_LIMIT_URL=redis://localhost:6379

# ===== Optional ENVS ======

# Other Optionals
TEST_API_KEY= # use if you've set up authentication and want to test with a real API key
BULL_AUTH_KEY= @
PLAYWRIGHT_MICROSERVICE_URL=  # set if you'd like to run a playwright fallback
```

### Installing dependencies

First, install the dependencies using pnpm.

```bash
pnpm install
```

### Running the project

You're going to need to open 3 terminals.

### Terminal 1 - setting up redis

Run the command anywhere within your project

```bash
redis-server
```

### Terminal 2 - setting up workers

Now, navigate to the apps/api/ directory and run:

```bash
pnpm run workers
```

This will start the workers who are responsible for processing crawl jobs.

### Terminal 3 - setting up the main server

To do this, navigate to the apps/api/ directory and run if you don’t have this already, install pnpm here: https://pnpm.io/installation
Next, run your server with:

```bash
pnpm run start
```

### Terminal 3 - sending our first request.

Alright: now let’s send our first request.

```curl
curl -X GET http://localhost:3002/test
```

This should return the response Hello, world!

If you’d like to test the crawl endpoint, you can run this

```curl
curl -X POST http://localhost:3002/v1/crawl \
    -H 'Content-Type: application/json' \
    -d '{
      "url": "https://mendable.ai"
    }'
```

## Tests:

The best way to do this is run the test with `npm run test:local-no-auth` if you'd like to run the tests without authentication.

If you'd like to run the tests with authentication, run `npm run test:prod`
