
# Elering Daily Visualizer

This project is a Flask server and Plotly frontend for visualizing daily energy consumption from the Estfeed/Elering API. It supports caching, nicknames/colors for meters, and a modern UI.

 ![Screenshot](https://raw.githubusercontent.com/npuee/energy-visualizer/refs/heads/master/docs/Screenshot.png)

## Features
* Optional HTTP Basic Auth (set `basic_auth_user` and `basic_auth_password` in `settings.json`)
## Manually Refresh Data

To force a fresh fetch from the API (bypassing the cache), open this URL in your browser or use curl:

```
http://localhost:8889/data?clear_cache=1
```

Or with curl:

```bash
curl "http://localhost:8889/data?clear_cache=1"
```

This will clear the cache and trigger a new data fetch on the next request.
- Visualizes daily kWh per meter and total
- Caching (1 hour, file-backed)
- Configurable nicknames/colors for each EIC in `settings.json`
- Legend and hover show nicknames
- Timestamp of last data fetch
- Docker support


## Quick Start (Local)

1. Install dependencies:
	```bash
	python3 -m pip install -r requirements.txt
	```
2. Configure your settings in `settings.json` (see below).
3. Run the server with WSGI (Waitress):
	```bash
	python3 wsgi.py
	# then open http://localhost:8889 (or the port in settings.json)
	```
   ```json
   "basic_auth_user": "admin",
   "basic_auth_password": "changeme"
   ```
   
   Or for development only (not recommended for production):
	```bash
	python3 app.py
	```


## Docker Usage

You can use the provided `run.sh` script to create a default `settings.json` (if missing), build the Docker image, and run the container:

```bash
chmod +x run.sh
./run.sh

## HTTP Basic Authentication

If you set `basic_auth_user` and `basic_auth_password` in your `settings.json`, all endpoints will require HTTP Basic Auth.

- Most browsers and API clients (curl, Postman, etc.) will prompt for a username and password.
- Use the values you set in `settings.json`.
- Example curl usage:
	```bash
	curl -u admin:changeme http://localhost:8889/
	```
- If credentials are missing or incorrect, the server will respond with HTTP 401 Unauthorized.

**Note:** If you do not set these fields, authentication is not required.

```

This will:
- Create a default `settings.json` if it does not exist (edit it with your real credentials!)
- Build the Docker image
- Run the container, mounting your `settings.json` for configuration

Alternatively, you can build and run manually:

```bash
docker build -t energy-visualizer:latest .
docker run -p 8889:8889 -v "$PWD/settings.json:/app/settings.json:ro" energy-visualizer
```


## Configuration

Edit `settings.json` to set:
- `auth_data` for Elering API (see below)
- `cache_ttl` (seconds)
- `server_port` (server always listens on 0.0.0.0)
- `eic_nicknames` for meter display names and colors

### How to Get Elering API Credentials

1. Go to [Estfeed Elering API Guide](https://estfeed.elering.ee/account-settings/efkp-api-guide)
2. Log in with your account.
3. Follow the instructions to create an API client:
		- Register a new API client.
		- Copy your `client_id` and `client_secret`.
		- Set `grant_type` to `client_credentials`.
4. Add these values to your `settings.json`:
		```json
		"auth_data": {
			"client_id": "your_client_id",
			"client_secret": "your_client_secret",
			"grant_type": "client_credentials"
		}
		```

Example:
```json
{
	"auth_data": { "client_id": "...", "client_secret": "...", "grant_type": "client_credentials" },
	"cache_ttl": 3600,
	"server_port": 8889,
	"host": "0.0.0.0",
	"eic_nicknames": {
		"38ZEE-00261472-Y": { "nick": "Shortname.", "color": "#1f77b6" }
	}
}
```

## Security

- `settings.json` is in `.gitignore` and should not be committed to version control.
- Do not share your API credentials.

## License
MIT
