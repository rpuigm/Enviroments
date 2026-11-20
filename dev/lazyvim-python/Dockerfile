FROM ubuntu:24.04

# Evitar preguntas interactivas durante la instalación
ENV DEBIAN_FRONTEND=noninteractive

# Instalar herramientas base, Python, dependencias de Neovim y utilidades para LazyVim
RUN apt-get update && apt-get install -y \
    curl \
    git \
    unzip \
    wget \
    build-essential \
    ripgrep \
    fd-find \
    python3 \
    python3-pip \
    python3-venv \
    locales \
    && rm -rf /var/lib/apt/lists/*

# Configurar idioma UTF-8 (necesario para los iconos de LazyVim)
RUN locale-gen en_US.UTF-8
ENV LANG=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8

# Instalar la última versión estable de Neovim (los repositorios de Ubuntu suelen tener versiones antiguas)
RUN curl -LO https://github.com/neovim/neovim/releases/download/v0.12.6/nvim-linux-x86_64.tar.gz \
    && rm -rf /opt/nvim \
    && tar -C /opt -xzf nvim-linux-x86_64.tar.gz \
    && ln -s /opt/nvim-linux64/bin/nvim /usr/local/bin/nvim \
    && rm nvim-linux-x86_64.tar.gz

# Clonar el starter de LazyVim
RUN git clone https://github.com/LazyVim/starter ~/.config/nvim \
    && rm -rf ~/.config/nvim/.git

# Configurar el extra oficial de Python para LazyVim (LSP Pyright, Ruff, etc.)
RUN echo '{\n  "extras": [\n    "lazyvim.plugins.extras.lang.python"\n  ],\n  "news": {\n    "NEWS.md": "6520"\n  },\n  "version": 6\n}' > /root/.config/nvim/lazyvim.json

ENV PATH="/opt/nvim-linux-x86_64/bin:${PATH}"

# Ejecutar Neovim en modo headless para que descargue todos los plugins antes de arrancar el contenedor
RUN nvim --headless "+Lazy! sync" +qa

# Directorio de trabajo por defecto
WORKDIR /workspace

# Mantener el contenedor vivo en segundo plano como si fuera una máquina virtual
CMD ["sleep", "infinity"]
