# templates

```bash
docker buildx create --name multiplatform --bootstrap --use
docker buildx ls
```

```bash
git clone https://github.com/tonistiigi/binfmt.git
docker buildx bake
docker run --privileged --rm tonistiigi/binfmt --install all
```
