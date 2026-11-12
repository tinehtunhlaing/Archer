# Archer

Archer is a web-first freelance marketplace for clients and freelancers. Product behavior and the current MVP scope are described in [SPEC.md](SPEC.md).

## Repositories

| Project | Repository | Purpose |
|---|---|---|
| Web app | [`app`](app/) · [Archer-app](https://github.com/tinehtunhlaing/Archer-app) | React client for marketplace workflows |
| API | [`api`](api/) · [Archer-api](https://github.com/tinehtunhlaing/Archer-api) | Express service, authentication, and persistence |
| Mobile | [`mobile`](mobile/) · [Archer-mobile](https://github.com/tinehtunhlaing/Archer-mobile) | Expo client for the Archer API |

Each project has its own dependencies, setup, and release lifecycle. This repository coordinates the projects and contains the product and contributor documentation; it is not a shared application package.

Clone with the project repositories initialized:

```sh
git clone --recurse-submodules https://github.com/tinehtunhlaing/Archer.git
```

For an existing clone, initialize or refresh the project checkouts with:

```sh
git submodule update --init --recursive
```

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
- [AGENTS.md](AGENTS.md) sets durable project boundaries and implementation guidance.
- [CLAUDE.md](CLAUDE.md) imports the shared guidance in `AGENTS.md`.
- Project READMEs own setup, operation, and project-specific commands.

## License

Archer and each standalone project are licensed under the MIT License. See the `LICENSE` file in this repository and the corresponding file in each project repository.
