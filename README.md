![Carbonetes](https://carbonetes-oss.s3.us-west-2.amazonaws.com/carbonetes-lite.png) 


<br/>
<br/>

# Carbonetes Lite

Carbonetes Lite is the ultimate app for connecting with our open-source technologies Jacked, Diggity, and BrainIaC. Streamline your workflows, boost productivity, and simplify collaboration by leveraging the power of these integrated OSS tools. Scan for vulnerabilities, protect sensitive information, and ensure compliance. Stay secure with Carbonetes Lite!

# Available Features
### Integrations

- [Jacked](https://github.com/carbonetes/jacked)
- [Diggity](https://github.com/carbonetes/diggity)

### Features/Functionalities

- Manage Licenses
- Container Registries

# Coming Soon
### Integrations
- [BrainIAC](https://github.com/carbonetes/jacked)
### Features/Functionalities
- Policy Bundles
- Dashboard
- Notifications
# Pre-Requisites
Make sure you have the following software installed on your system:
- Docker: [Install Docker](https://docs.docker.com/get-docker/)
- Docker Compose: [Install Docker Compose](https://docs.docker.com/compose/install/)


# Installation

1. Create a yaml file named 'docker-compose.yaml'
2. Copy and paste this to the created file.

```yaml
version: '3.8'
services:
  db:
    image: postgres:alpine3.17
    volumes:
      - "/Users/Shared/postgresql/data/db:/var/lib/postgresql/data" #Update accordingly when running in windows os
    ports:
      - "5432:5432"
    environment:
      - POSTGRES_USER=username
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=lite
    healthcheck:
        test: ["CMD", "pg_isready", "-d", "lite", "-U", "username"]
        interval: 5s
        timeout: 10s
        retries: 5
    networks:
      - internal  
  carbonetes-engine:
    image: carbonetes/lite-carbonetes-engine:latest
    ports:
    - "3001:3001" 
    restart: always
    depends_on:
      db:
        condition: service_healthy
    environment:
      - HOST=localhost
      - USERNAME=username
      - PASSWORD=password   
      - DBNAME=lite
      - DBPORT=5432
      - DBHOST=db
    healthcheck:
      test: ["CMD", "wget", "--no-verbose","--tries=1", "--spider", "http://host.docker.internal:3001/healthcheck"]
      interval: 5s
      timeout: 10s
      retries: 5  
    networks:
      - internal 
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
  web:
    image: carbonetes/lite-web:latest
    ports:
    - "80:80"
    restart: always
    depends_on:
      carbonetes-engine:
        condition: service_healthy
    networks:
    - internal   
networks:
  internal:
    driver: bridge            
```

3. Modify the Docker Compose file (if necessary): Open the `docker-compose.yml` file in a text editor. If you need to modify any configuration parameters, such as volume paths or environment variables, make the necessary changes according to your requirements.
3. Run `docker compose up`or `$ docker-compose up`
5. Once the containers are up and running, you can access the page [<ins> here <ins/>](http://localhost)
