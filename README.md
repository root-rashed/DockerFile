# 🐳 **Dockerfile Instructions Explained (With Examples)**

## ✅ **1. FROM**

Specifies the base image.

```Dockerfile
FROM python:3.11-slim
```

> Every Dockerfile must start with **FROM**.

---

## ✅ **2. LABEL**

Adds metadata to the image.

```Dockerfile
LABEL maintainer="rashed@example.com"
LABEL version="1.0"
LABEL description="My API Service"
```

---

## ✅ **3. ENV**

Set environment variables.

```Dockerfile
ENV PORT=8000
ENV MODE=production
```

You can access them inside the container:

```
$ echo $PORT
```

---

## ✅ **4. WORKDIR**

Sets the working directory for all commands that follow.

```Dockerfile
WORKDIR /app
```

Equivalent to running:

```
cd /app
```

---

## ✅ **5. COPY**

Copy files from your machine → container.

```Dockerfile
COPY requirements.txt .
COPY src/ /app/src/
```

---

## ✅ **6. ADD**

Similar to COPY, **but more powerful** (downloads URLs, extracts tar.gz automatically).

```Dockerfile
ADD archive.tar.gz /app/data
ADD https://example.com/file.txt /app/
```

⚠️ Recommended: **Use COPY unless you need ADD features**.

---

## ✅ **7. RUN**

Runs commands **during image build**.

```Dockerfile
RUN apt update && apt install -y nginx
RUN pip install -r requirements.txt
```

Images store the results of RUN steps.

---

## ✅ **8. CMD**

Specifies the **default command** when container starts.

```Dockerfile
CMD ["python", "app.py"]
```

Only one **CMD** is used — if you add multiple, only the last one works.

---

## ✅ **9. ENTRYPOINT**

Sets a fixed command that always runs.

```Dockerfile
ENTRYPOINT ["python"]
CMD ["app.py"]
```

Container runs:

```
python app.py
```

Good for CLI applications.

---

## CMD vs ENTRYPOINT (Important!)

### Example 1 — ENTRYPOINT acts like a base command

Dockerfile:

```Dockerfile
ENTRYPOINT ["echo"]
```

Running:

```
docker run myimage Hello
```

Output:

```
Hello
```

### Example 2 — CMD is override-able

Dockerfile:

```Dockerfile
CMD ["echo", "Hello"]
```

Running:

```
docker run myimage Hi
```

Output:

```
Hi
```

---

## ✅ **10. EXPOSE**

Documents the port (does NOT publish it).

```Dockerfile
EXPOSE 8080
```

To publish, you still need:

```
docker run -p 8080:8080 myimage
```

---

## ✅ **11. VOLUME**

Defines mount points.

```Dockerfile
VOLUME ["/data"]
```

Used for storing persistent data.

---

## ✅ **12. USER**

Specifies the user to run commands.

```Dockerfile
USER root
USER appuser
```

Best practice: avoid running apps as **root**.

---

## ✅ **13. ARG**

Build-time variables (only available at build time).

```Dockerfile
ARG NODE_VERSION=20
FROM node:${NODE_VERSION}
```

Build with:

```
docker build --build-arg NODE_VERSION=18 .
```

---

## ✅ **14. HEALTHCHECK**

Tells Docker how to test if the container is healthy.

```Dockerfile
HEALTHCHECK CMD curl --fail http://localhost:8080 || exit 1
```

---

## ✅ **15. SHELL** (rarely used)

Changes the default shell for RUN commands.

Linux default:

```Dockerfile
SHELL ["/bin/bash", "-c"]
```

Windows:

```Dockerfile
SHELL ["powershell", "-command"]
```

---

## 🧱 **16. Multi-Stage Build Instructions**

Used for small, optimized images.

### Example:

```Dockerfile
FROM node:20 AS builder
WORKDIR /app
COPY . .
RUN npm install && npm run build

FROM nginx
COPY --from=builder /app/dist /usr/share/nginx/html
```

---

# 🎯 Summary Table (Quick Reference)

| Instruction | Purpose                   |
| ----------- | ------------------------- |
| FROM        | Base image                |
| LABEL       | Metadata                  |
| ENV         | Environment vars          |
| WORKDIR     | Working directory         |
| COPY        | Copy local files          |
| ADD         | Copy + extract + download |
| RUN         | Build-time commands       |
| CMD         | Default runtime command   |
| ENTRYPOINT  | Fixed runtime command     |
| EXPOSE      | Document port             |
| VOLUME      | Persistent data           |
| USER        | Specify user              |
| ARG         | Build-time variable       |
| HEALTHCHECK | Test container health     |
| SHELL       | Change shell              |
| STAGES      | Optimized builds          |
