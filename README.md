# QR Code Generator (Python + Docker)

A simple command-line QR code generator that validates URLs and saves timestamped PNG files.

## What This Project Does

- Generates a QR code from a URL.
- Validates the URL before generating the image.
- Saves the output as a timestamped PNG file.
- Supports configurable output directory and colors via environment variables.
- Runs locally with Python or in Docker.

## Tech Stack

- Python 3.12
- qrcode
- Pillow
- validators
- python-dotenv
- Docker

## Project Structure

```text
.
|-- main.py
|-- requirements.txt
|-- Dockerfile
|-- .gitignore
|-- .dockerignore
|-- qr_codes/
```

## How It Works

The app:

1. Reads environment variables.
2. Accepts an optional URL from the CLI.
3. Validates that URL.
4. Creates the output folder if needed.
5. Saves a PNG named like QRCode_YYYYMMDDHHMMSS.png.

Default CLI URL:

- https://github.com/jam398

Default output folder:

- qr_codes

## Environment Variables

You can set these values in your shell or a .env file.

- QR_CODE_DIR: output directory (default: qr_codes)
- FILL_COLOR: QR foreground color (default: red)
- BACK_COLOR: QR background color (default: white)

Example .env:

```env
QR_CODE_DIR=qr_codes
FILL_COLOR=black
BACK_COLOR=white
```

## Run Locally (Python)

1. Create and activate a virtual environment.
2. Install dependencies.
3. Run the script.

```bash
python -m venv .venv
source .venv/Scripts/activate
pip install -r requirements.txt
python main.py --url https://example.com
```

If your URL is valid, the PNG is saved in qr_codes/.

## Run With Docker

Docker Hub repository:

- https://hub.docker.com/repository/docker/lakalma/qr-code-generator-app/general

### Build Image

```bash
docker build -t qr-code-generator-app .
```

### Run Container (foreground)

```bash
docker run --rm qr-code-generator-app
```

### Run with custom URL

```bash
docker run --rm qr-code-generator-app --url https://example.com
```

## Save Files to Host Folder

To persist generated images on your machine, bind-mount the local qr_codes folder.

### Git Bash on Windows (recommended)

```bash
mkdir -p qr_codes
MSYS_NO_PATHCONV=1 docker run --rm -v "$(pwd)/qr_codes:/app/qr_codes" qr-code-generator-app
```

### PowerShell on Windows

```powershell
docker run --rm -v "${PWD}\qr_codes:/app/qr_codes" qr-code-generator-app
```

## Common Issues

### PNG not appearing in local qr_codes

Cause:

- The container wrote to its internal filesystem.
- Or Git Bash path conversion broke the -v mount path.

Fix:

- Use a correct bind mount.
- In Git Bash, use MSYS_NO_PATHCONV=1.

### Invalid URL message

Cause:

- The URL did not pass validators.url.

Fix:

- Include protocol, for example https://example.com.

## Useful Docker Commands

View containers:

```bash
docker ps -a
```

View logs:

```bash
docker logs <container-name-or-id>
```

Remove a stopped container:

```bash
docker rm <container-name-or-id>
```

Force remove a running container:

```bash
docker rm -f <container-name-or-id>
```

## Notes

- The app runs as a non-root user inside the container.
- Generated artifacts in qr_codes/ are ignored by Git and Docker context via .gitignore and .dockerignore.
