# The Agent Containerizes It

The agent gets to work. First it looks around the project — the same files you'd
see:

```bash
ls
```

It reads the Dockerfile it's going to build. Notice the base image:

```bash
cat Dockerfile
```

`FROM node:20` — a full operating system and **431 packages**, most of which the
app never uses. We'll come back to that in **Choice**. The line to watch for now
is `COPY . .`: it copies *everything* in the directory into the image. Hold that
thought.

Now build it:

```bash
docker build -t catalog-service:baseline .
```

And bring the whole stack up with Compose — Postgres, Kafka, LocalStack (S3),
WireMock, the API, and the frontend:

```bash
docker compose up -d
```

It works. The API is live, the tests would pass, and from the outside the agent
did exactly what you asked. **This is the part everyone sees and loves.**

> The image builds, the app runs, the goal is met. If you stopped here, you'd call
> it a success — and ship it.

But the agent did more than build an image. It read everything in the project to
do it, and `COPY . .` put everything into the image. Let's look at what "everything"
included. Continue to **The Leak: Keys & Credentials**.
