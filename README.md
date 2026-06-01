# my-static-site

A minimal static site served by **Nginx inside Docker**, following the Dev → GitHub → Docker → Client flow.

---

## 📁 Project Structure

```
my-static-site/
├── index.html      ← Your website (edit this!)
├── Dockerfile      ← Builds the Nginx container
└── README.md
```

---

## 🚀 Full Workflow (Step by Step)

### ✅ One-Time Setup

**1. Create a GitHub repo**
- Go to github.com → New Repository → name it `my-static-site`
- Copy the repo URL

**2. Push this project to GitHub (Device 1)**
```bash
cd my-static-site
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/my-static-site.git
git push -u origin main
```

---

### 🖥️ On the Server (or your machine acting as server)

**3. Clone from GitHub**
```bash
git clone https://github.com/YOUR_USERNAME/my-static-site.git
cd my-static-site
```

**4. Build the Docker image**
```bash
docker build -t my-static-site .
```

**5. Run the container**
```bash
docker run -d --name my-site -p 80:80 my-static-site
```

**6. Open in browser (Device 2)**
```
http://localhost        ← if running locally
http://<server-ip>      ← if running on a server/VM
```

---

## 🔄 Update Cycle (How Changes Propagate)

### Device 1 — Make a change
```bash
# Edit index.html in VS Code, then:
git add .
git commit -m "update: changed the heading"
git push origin main
```

### Server — Pull & Rebuild
```bash
cd my-static-site
git pull origin main
docker build -t my-static-site .
docker stop my-site && docker rm my-site
docker run -d --name my-site -p 80:80 my-static-site
```

### Device 2 — Refresh the browser
```
http://<server-ip>   ← just hit F5, changes appear!
```

---

## 💡 Tips

| Scenario | Command |
|----------|---------|
| Check container is running | `docker ps` |
| See container logs | `docker logs my-site` |
| Stop the site | `docker stop my-site` |
| Remove container | `docker rm my-site` |
| Rebuild from scratch | `docker build --no-cache -t my-static-site .` |

---

## 🗺️ Architecture

```
[VS Code] → git push → [GitHub] → git pull → [Docker + Nginx] → [Browser]
 Device 1               (cloud)     Server       Port 80           Device 2
```