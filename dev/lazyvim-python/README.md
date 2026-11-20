podman build -t python-lazyvim .
podman run -d --name dev-box -v .:/workspace python-lazyvim
podman exec -it dev-box bash
