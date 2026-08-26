# Open-WebUI - Gemma4 E2B

## 環境構築

compose.yml

```yaml
services:
  ollama:
    image: ollama/ollama:0.32.15
    ports:
      - 11434:11434
    volumes:
      - ./ollama/.ollama:/root/.ollama
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              count: all
              capabilities: [gpu]

  open-webui:
    image: ghcr.io/open-webui/open-webui:main
    ports:
      - 18080:8080
    volumes:
      - ./open-webui/data:/app/backend/data
    environment:
      - OLLAMA_BASE_URL=http://ollama:11434
    depends_on:
      - ollama
```

Nvidia Container Toolkit のインストール  
コンテナが GPU を使えるようにする

```sh
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | \
    sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
    && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
        sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
        sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
```

コンテナの起動

```sh
docker compose up -d
```

Ollama に Gemma モデルをダウンロード

```sh
docker compose exec -it ollama ollama pull gemma4:e2b
```