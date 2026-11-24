# QuickBite Spring Boot / Docker-outside-of-Docker

## Runner setup (Linux host)

The Runner container uses the host Docker daemon through
`/var/run/docker.sock`. This is Docker-outside-of-Docker (DooD): Docker CLI
commands run in the Runner container, while the daemon runs on the Linux host.

1. Make sure Docker Engine and the Docker Compose plugin are installed and
   running on the Linux host.
2. Copy `.env.example` to `.env`. Set `GITHUB_REPOSITORY_URL` to this repo's
   URL and `GITHUB_RUNNER_PAT` to a GitHub token that can register a runner
   for this repository. Do not commit `.env`.
3. Start the Runner from this directory:

   ```sh
   docker compose up -d
   docker compose logs -f github-runner
   ```

4. Confirm the Runner appears as online in the repository's
   **Settings > Actions > Runners** page. Push a commit to trigger
   `.github/workflows/docker-dood.yml`; it runs `docker info` and builds this
   Spring Boot project.

The socket mount grants the Runner container broad control over the host's
Docker daemon. Use it only for trusted repositories and workflows; do not
allow untrusted pull requests to execute on this Runner.
