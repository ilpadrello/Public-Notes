The goal is to automate deployment using Github (or others), and making sure the code build and work before running it. If a step fail go back to latest stable version, without downtime.

### Typical Setup

1. CI/CD trigger -> GitHub Actions or other
2. Deployment Strategy -> Blue-Green or Rolling Deploymennt
3. Rollback mechanism -> Keep previous builds ready

### USING DOCKER
Using docker is the best way since you can isolate and have builds that don't break your server, easy rollback with restarting previous container, zero downtime with a traffick switch.

### WORKFLOW

- Push to `master` → GitHub Actions builds a Docker image → push to Docker Registry (could be Docker Hub or GitHub Packages).
- Server pulls the new image.
- Start container `myapp-v2` alongside `myapp-v1`.
- Run health check.
    - ✅ If healthy → switch proxy (NGINX / Traefik) to `v2`.
    - ❌ If fail → keep serving `v1`.
- Optionally stop/remove old container after a grace period