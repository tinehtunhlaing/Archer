# Archer

Archer is a web-first freelance marketplace for clients and freelancers. The workspace contains three separately maintained projects: a web app, an API, and a planned mobile app. Product behavior and the current MVP scope are described in [SPEC.md](SPEC.md).

## Projects

| Project | Path | Purpose | Developer guide |
|---|---|---|---|
| Web app | [`app/`](app/) | React client for marketplace workflows | [app/README.md](app/README.md) |
| API | [`api/`](api/) | Express service, authentication, and persistence | [api/README.md](api/README.md) |
| Mobile | [`mobile/`](mobile/) | Expo client for auth, job discovery and posting, proposals, and messaging | [mobile/README.md](mobile/README.md) |

Each project is intended to have its own repository, dependencies, setup, and release lifecycle. The root workspace is for project-level documentation and coordination; it is not a shared application package.

## Local development

Start the API first, then run the web app in a second terminal:

```sh
cd api
npm install
cp .env.example .env
# Set a local JWT_SECRET in api/.env before starting the API.
npm run db:generate
npm run db:migrate
npm run db:seed
npm run dev
```

```sh
cd app
npm install
cp .env.example .env
npm run dev
```

The API serves at `http://localhost:4000`; the web app serves at `http://localhost:5173`. See each project's README for environment variables, sample accounts, commands, and implementation notes. Demo credentials are for local seeded data only.

Start the mobile app in another terminal with `cd mobile && npm install && npm start`. On iOS, run `npm run ios` from `mobile/` to open the simulator.

## Documentation ownership

- [SPEC.md](SPEC.md) describes product features, confirmed scope, and open product decisions.
- [AGENTS.md](AGENTS.md) sets the durable project boundaries and implementation guidance for coding agents.
- [CLAUDE.md](CLAUDE.md) imports the shared guidance in `AGENTS.md` for Claude Code.
- Project READMEs are the user/developer guides for setup, operation, and project-specific commands.
