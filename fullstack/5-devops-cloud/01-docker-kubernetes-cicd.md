# Docker, Kubernetes and CI/CD

> **Time needed:** about 1.5 hours · **Questions:** 14 + 10 quick questions
> Say every "Say this" answer OUT LOUD once. Reading silently is not practice.

## Progress checklist
- [ ] 1. Containers vs VMs; image vs container
- [ ] 2. Dockerfile best practices for Node.js
- [ ] 3. Layers and build caching
- [ ] 4. The PID 1 problem
- [ ] 5. Volumes, networks, Docker Compose
- [ ] 6. Kubernetes core objects
- [ ] 7. ConfigMap and Secret
- [ ] 8. Liveness, readiness, startup probes
- [ ] 9. Requests, limits, autoscaling (HPA)
- [ ] 10. Rolling deployments and rollback
- [ ] 11. Blue-green vs canary vs feature flags
- [ ] 12. A good CI/CD pipeline
- [ ] 13. Debugging a misbehaving pod
- [ ] 14. Helm, Infrastructure as Code, GitOps

---

## 1. What's the difference between a container and a VM? And between an image and a container?

**In simple words**
A VM is a whole fake computer with its own operating system. A container is just your app running in an isolated box that shares the host's operating system. So containers are much lighter and faster.

**Picture it**
A VM is a separate house with its own water and power. A container is a flat in one building: its own locked door, but shared pipes. An image is like a class, a container is like an object made from it.

**🗣️ Say this in the interview**
> "A **virtual machine** emulates a whole computer with its own operating system **kernel** (the core of the OS). It's heavy, slow to boot, and uses GBs. A **container** is an **isolated process** that **shares the host's kernel**. It's light, starts in seconds, and is MBs.
> An **image** is the packaged, **read-only template**: the app, its dependencies and config. A **container** is a **running copy** of that image. Like a class and an object: one image, many containers."

**⚠️ Avoid saying**
"A container is a small VM." It shares the host kernel; a VM doesn't.

---

## 2. What are your Dockerfile best practices for Node.js?

**In simple words**
Keep the final image small, safe and quick to rebuild. Build in one stage, copy only what you need into a clean final stage, and don't run as root.

**🗣️ Say this in the interview**
> "My checklist: a **small official base image** with a pinned version. A **multi-stage build**, so build tools and dev dependencies never reach the final image. **Copy `package*.json` first** and install, *then* copy the code, so the install step stays cached when only code changes. **`npm ci --omit=dev`** for production dependencies. Run as a **non-root user**. A `.dockerignore` for `node_modules`, `.git` and `.env`. **No secrets in the image**. And start with `node server.js` directly, not `npm start`, so shutdown signals reach Node."

```dockerfile
# ---- build stage: has all tools and dev dependencies ----
FROM node:22-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
# build, then remove dev dependencies
RUN npm run build && npm prune --omit=dev

# ---- runtime stage: only what the app needs to run ----
FROM node:22-alpine
ENV NODE_ENV=production
WORKDIR /app
COPY --from=build --chown=node:node /app/node_modules ./node_modules
COPY --from=build --chown=node:node /app/dist ./dist
COPY --chown=node:node package.json ./
# don't run as root
USER node
EXPOSE 3000
# run node directly, not npm start, so it gets the stop signal
CMD ["node", "dist/server.js"]
```

(Note: in a Dockerfile, a comment must be on its own line. A `#` at the end of a line is not a comment.)

**They may also ask**
- *Why multi-stage?* The final image has no compilers or dev packages. It's smaller and has less to attack.

**⚠️ Avoid saying**
"I copy everything, then run `npm install`." Every code change would reinstall all packages.

---

## 3. How do layers and build caching work?

**In simple words**
Each line in a Dockerfile makes a **layer** (a saved step). Docker reuses saved layers until something changes. After a changed layer, every layer below it rebuilds.

**Picture it**
A stack of pancakes. If you change the third pancake, you have to remake every pancake on top of it.

**🗣️ Say this in the interview**
> "Each Dockerfile instruction creates a **layer**, and Docker **reuses cached layers** until something changes. Once a layer changes, every layer **after** it rebuilds. So I order steps from **least to most often changing**: base image, system packages, then `package.json` and install, then the source code. Get that wrong and every code change reinstalls all npm packages."

**⚠️ Avoid saying**
Putting `COPY . .` before `npm ci`. That breaks the cache on every change.

---

## 4. What is the PID 1 problem in Node containers?

**In simple words**
In a container, your app is usually the first process, **PID 1**. Linux treats PID 1 specially: it doesn't get the normal default handling of stop signals. So if Node doesn't handle SIGTERM, it won't shut down nicely. It just gets killed after a timeout.

**🗣️ Say this in the interview**
> "Inside a container the app usually runs as **PID 1**, and PID 1 **doesn't get default signal handling**. So if Node has no SIGTERM handler, `docker stop` or a Kubernetes shutdown doesn't stop it gracefully. It waits for the timeout and then gets killed. Also, with `npm start` as the command, npm is PID 1 and **may not pass the signal** on to Node.
> Fixes: run `node` directly in `CMD`, **handle SIGTERM** in the app (see the Node.js file, graceful shutdown), and optionally use a tiny init like `tini` (or `docker run --init`). It forwards signals and cleans up zombie processes."

**⚠️ Avoid saying**
"`npm start` is fine in Docker." npm may swallow the stop signal.

---

## 5. How do volumes, networks and Docker Compose work?

**In simple words**
A container's files are deleted with the container. Anything you must keep goes in a **volume**. Containers on the same network find each other by name. Compose starts several containers from one file.

**🗣️ Say this in the interview**
> "A container's filesystem is **thrown away** with the container. So anything that must survive goes in a **volume**, like database files or uploads in local dev. Containers on the same **Docker network** reach each other **by service name** through built-in DNS, like `postgres:5432`. **Docker Compose** describes a multi-container setup (API, Postgres, Redis) in one YAML file. It's perfect for local development. In production, app containers should be **stateless and disposable**, and data lives in managed databases and object storage."

**⚠️ Avoid saying**
"I store uploads inside the container." They're lost on the next deploy.

---

## 6. What are the core Kubernetes objects?

**In simple words**
Kubernetes runs your containers across many machines and keeps them healthy. You describe what you want ("3 copies of my API"), and it makes it happen.

**Picture it**
A Deployment is the manager saying "always keep 3 cashiers on shift". Pods are the cashiers. The Service is the one "Pay here" sign that sends you to whichever cashier is free.

**🗣️ Say this in the interview**
> "- **Pod**: the smallest unit, one or more containers sharing network and storage. Pods are **disposable**.
> - **Deployment**: keeps N copies of a pod running, and handles **rolling updates and rollbacks**.
> - **Service**: a **stable name and IP** that load-balances to the healthy pods, because pod IPs keep changing.
> - **Ingress**, or the newer **Gateway API**: routes outside HTTP traffic (domains, paths, TLS) to Services.
> - **ConfigMap / Secret**: config and sensitive values given to pods.
> - **HPA**: scales the number of pods automatically.
> - Also: **StatefulSet** (stable identity, for databases), **DaemonSet** (one pod on every node, like a log agent), and **Job / CronJob** (tasks that run to the end and stop)."

**⚠️ Avoid saying**
"I connect to the pod's IP." Pod IPs change. Always go through a Service.

---

## 7. How do ConfigMaps and Secrets work?

**In simple words**
ConfigMaps hold normal settings. Secrets hold passwords and keys. But a plain Kubernetes Secret is only **base64-encoded**, which anyone can decode. It is not encrypted.

**🗣️ Say this in the interview**
> "ConfigMaps hold **non-sensitive** config; Secrets hold sensitive values. But a plain Secret is only **base64-encoded**, not encrypted. So for real security I turn on encryption at rest, limit access with **RBAC** (role-based access control: who can read what), and preferably sync secrets from **AWS Secrets Manager or Azure Key Vault**, with the External Secrets Operator or the CSI driver. And config is never baked into the image. The same image goes to every environment."

**⚠️ Avoid saying**
"Secrets are encrypted, so they're safe." Base64 is not encryption.

---

## 8. What's the difference between liveness, readiness and startup probes?

**In simple words**
Probes are health checks Kubernetes runs on your pod. **Readiness** decides if the pod gets traffic. **Liveness** decides if the pod gets restarted. **Startup** gives slow apps time to boot.

**Picture it**
Readiness: "Is the cashier ready to serve?" If not, send customers to another counter. Liveness: "Has the cashier fainted?" If yes, replace them.

**🗣️ Say this in the interview**
> "**Readiness** asks *should this pod get traffic now?* If it fails, the pod is **removed from the Service** but not restarted. Useful during startup, during shutdown, or when overloaded.
> **Liveness** asks *is this process stuck?* If it fails, Kubernetes **restarts** the container.
> **Startup** protects slow-starting apps: liveness isn't checked until startup succeeds.
> The classic mistake is a **liveness probe that checks the database**. If the database has a short blip, Kubernetes restarts **every** pod at once, and a small problem becomes a full outage. Liveness should only check the process itself."

**⚠️ Avoid saying**
Using one endpoint for liveness and readiness, with database checks in both.

---

## 9. How do requests, limits and autoscaling (HPA) work?

**In simple words**
**Requests** are the CPU and memory a pod is promised. **Limits** are the most it can use. The **HPA** (Horizontal Pod Autoscaler) adds or removes pods based on load.

**🗣️ Say this in the interview**
> "**Requests** are what the pod is **guaranteed**. The scheduler uses them to place pods on nodes. **Limits** are the **maximum**. Go over the memory limit and the container is **OOMKilled** (killed for using too much memory, exit code 137). Go over the CPU limit and it's **throttled** (slowed down), which in Node shows up as random latency spikes.
> The **HPA** adds or removes pods based on CPU, memory, or custom metrics like requests per second or **queue length**. But more pods only help if the pods are the bottleneck. If the **database** is the bottleneck, more pods just put more pressure on it. And new pods take time to start, so for a known spike I **pre-scale**."

**They may also ask**
- *Node and memory limits?* Make V8's heap limit fit inside the container limit with `--max-old-space-size`, about 75% of the limit. Otherwise V8 can grow past it and get OOMKilled.

**⚠️ Avoid saying**
"Just add more pods." Not if the database is the bottleneck.

---

## 10. How do rolling deployments and rollback work?

**In simple words**
A rolling update swaps old pods for new ones a few at a time, so the site never goes down. Rollback means going back to the previous version.

**🗣️ Say this in the interview**
> "A rolling update replaces pods **gradually**: start a new pod, wait until it's **ready**, then remove an old one. `maxSurge` and `maxUnavailable` control the speed. **Readiness probes** make it safe, because traffic only goes to pods that are really ready.
> Rollback is `kubectl rollout undo`, or redeploying the previous image tag. But it only works if **database changes are backward compatible**, using expand and contract (see the Databases file). Old and new versions run side by side during the rollout."

**⚠️ Avoid saying**
"Rollback is always one command." Not if the new version already changed the database in a breaking way.

---

## 11. Blue-green vs canary vs feature flags?

**In simple words**
Three ways to release safely. **Blue-green** switches all traffic at once between two copies. **Canary** sends a small share of users to the new version first. **Feature flags** ship the code switched off and turn it on later.

**Picture it**
A canary is like a restaurant testing a new dish on one table before adding it to the menu.

**🗣️ Say this in the interview**
> "**Blue-green**: run two full environments and **switch all traffic** from blue to green at once. Instant rollback, but double the capacity. **Canary**: send a **small percentage**, say 5%, to the new version, watch errors and latency, then increase step by step. It limits the **blast radius** (how many users a bug can hurt), but needs traffic splitting and good metrics. **Feature flags** separate **deploy** from **release**: code ships switched off, then I turn it on for internal users, then 10%, then everyone. Rollback is flipping a switch.
> I like canary deploys for **infrastructure risk** and feature flags for **product risk**."

**⚠️ Avoid saying**
"Deploy and release are the same thing." Feature flags let you deploy without releasing.

---

## 12. What does a good CI/CD pipeline look like?

**In simple words**
Check every pull request automatically. After merge, build one Docker image and move that same image through staging to production.

**🗣️ Say this in the interview**
> "On every pull request: install from the lockfile, **lint**, **type-check**, **unit tests**, **integration tests**, a **security scan** of dependencies, and a build.
> On merge to main: build **one immutable artifact**, a Docker image tagged with the commit SHA. Scan it, push it to the registry, deploy to **staging**, run **smoke and E2E tests**, then **promote the same image** to production, with an approval step if needed. After that, post-deploy checks and automatic rollback if error rates spike.
> The key rule: **build once, promote the same artifact everywhere**. If you rebuild per environment, what you tested isn't what you ship."

**⚠️ Avoid saying**
"We build a new image for each environment." Then prod runs something you never tested.

---

## 13. How do you debug a pod that's misbehaving?

**In simple words**
Start with the pod's status, then its events, then the logs of the container that crashed.

```bash
kubectl get pods                                  # status: CrashLoopBackOff? Pending? OOMKilled?
kubectl describe pod <pod>                        # events: probe failures, scheduling, image pull errors
kubectl logs <pod> --previous                     # logs from the container that crashed
kubectl top pod <pod>                             # CPU and memory use
kubectl get events --sort-by=.lastTimestamp       # recent cluster events
kubectl exec -it <pod> -- sh                      # look inside (if the image has a shell)
kubectl rollout history deploy/<name>             # what changed recently
```

**🗣️ Say this in the interview**
> "I start with `kubectl get pods` to see the status. Then `describe` and `logs --previous` usually tell me the story: OOMKilled, a failing probe, a missing secret, or a crash on startup. I also check `rollout history` to see what changed recently."

**⚠️ Avoid saying**
"I check `kubectl logs`." Without `--previous` you see the new container, not the one that crashed.

---

## 14. What are Helm, Infrastructure as Code and GitOps?

**In simple words**
**Helm** makes Kubernetes YAML reusable with templates. **Infrastructure as Code** describes cloud resources in files kept in Git. **GitOps** makes the cluster copy whatever is in Git, automatically.

**🗣️ Say this in the interview**
> "**Helm** packages Kubernetes YAML into **templated charts**, so one chart deploys to dev, staging and prod with different values. **Infrastructure as Code** (Terraform, Bicep, CloudFormation) defines cloud resources in version-controlled files, so environments are repeatable and reviewed like code. **GitOps** (Argo CD, Flux) goes further: the cluster **keeps syncing itself to what's in Git**. A deploy is just a merged pull request, and manual changes (drift) are fixed automatically."

**⚠️ Avoid saying**
"We change things by hand in the cloud console." Changes should go through code and review.

---

## Quick questions
- **`CMD` vs `ENTRYPOINT`?** ENTRYPOINT is the fixed program to run. CMD gives default arguments that are easy to override.
- **`COPY` vs `ADD`?** Use COPY. ADD also unpacks archives and downloads URLs, which can surprise you.
- **Alpine vs slim vs distroless?** Alpine is tiny but uses musl libc, so some native modules break. Slim is Debian-based and compatible. Distroless has no shell, so the smallest attack surface.
- **CrashLoopBackOff?** The container keeps crashing, and Kubernetes waits longer between restarts. Check `logs --previous`.
- **Exit code 137?** Killed by SIGKILL, usually OOMKilled.
- **Pending pod?** It can't be placed: no node has enough of the requested CPU/memory, or a node selector/taint doesn't match.
- **ImagePullBackOff?** Wrong image name or tag, or missing registry credentials.
- **PodDisruptionBudget?** Keeps a minimum number of pods running during node maintenance.
- **Namespace?** A logical group inside a cluster (per team or environment), for RBAC and quotas.
- **Sidecar?** A helper container in the same pod, like a log shipper, proxy or service mesh.

## Remember these 10 things
1. A container shares the host kernel; a VM has its own. Image = template, container = running copy.
2. Multi-stage build, pinned small base image, non-root user, no secrets in the image.
3. Copy `package*.json` and install before copying code, so the install layer stays cached.
4. Run `node` directly (not `npm start`) and handle SIGTERM, because of the PID 1 problem.
5. Containers are disposable. Data goes in volumes, managed databases or object storage.
6. Deployment keeps pods running; Service gives them a stable address; Ingress brings outside traffic in.
7. Kubernetes Secrets are only base64. Use encryption at rest, RBAC, and Key Vault / Secrets Manager.
8. Readiness = gets traffic or not. Liveness = restart or not. Never check the database in liveness.
9. Memory over the limit = OOMKilled (137). CPU over the limit = throttled. Set Node's heap to about 75% of the limit.
10. Build once, promote the same image everywhere. Canary for infra risk, feature flags for product risk.
