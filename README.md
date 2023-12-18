# STMS Infrastructure

This Docker Compose configuration sets up the infrastructure required
for running the STMS (Simple Task Management System) solution.

## Prerequisites
- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Usage

1. Clone this repository:

   ```
   git clone https://github.com/pedjango/stms-infrastructure.git
   cd stms-infrastructure
   ```

2. Start the services:

   ```
   docker-compose up -d
   ```

3. Access Keycloak at [http://localhost:8180](http://localhost:8180) and log in with the following credentials:
    - Username: `admin`
    - Password: `admin`

## Services

### STMS Database (PostgreSQL 14.1)
- Container Name: `stms-database`
- Port: `5432`
- Database Name: `stms-user`
- Database Initialization: `./sql/init.sql`

### STMS Keycloak Database (PostgreSQL 9.6)
- Container Name: `stms-keycloak-database`
- Port: `25432`
- Database Name: `keycloak`
- Database User: `keycloak`
- Database Password: `keycloak`

### STMS Keycloak (Bitnami Keycloak 22.0.4)
- Container Name: `stms-keycloak`
- Port: `8180`
- Admin Console: [http://localhost:8180/auth/admin](http://localhost:8180/auth/admin)
- Admin User: `admin`
- Admin Password: `admin`
- Database Configuration: PostgreSQL on `stms-keycloak-database`

### Networks
- `stms-keycloak-net`: Custom network for communication between Keycloak components.

### Volumes
- `main-datastore`: Volume for STMS Database data.
- `keycloak-datastore`: Volume for Keycloak Database data.

Feel free to adapt this configuration to your specific needs.
Happy coding!

### Setting up Keycloak

After creating our project and adding all the dependencies we need, as well as creating
our infrastructure project which contains the Postgres database and the Keycloak instance,
we are going to move on to setting up our Keycloak instance for the purpose of our project.

#### Step 1 | Create a new realm

#### Step 2 | Create a new client

#### Step 3 | Configure the new client
```
[ General Settings ]
Client ID: stms-admin
Name: STMS Admin
Description: Simple Task Management System Admin

[ Access settings ]
Root URL: http://localhost:8040/*
Home URL: http://localhost:8040
Valid redirect URIs: http://localhost:8040 | http://localhost:8040/*
Web origins: http://localhost:8040/*
Admin URL: http://localhost:8040/*

[ Capability config ]
[+] Client authentication
[+] Authorization
Authentication Flow:
  [+] Standard Flow
  [+] Direct Access Grants
  [+] Service Account Roles
```
- Note that [http://localhost:8040](http://localhost:8040) is going to be our application's endpoint, hence we are using it in this setup

#### Step 4 | Create a new user
```
Username: stms-user
Password: STMS.trustno1!
```

#### Note
Keep in mind that we'll need a URL for the resource server, as well as the client secret.
```
Resource server: http://localhost:8180/realms/stms/protocol/openid-connect/certs
Client secret:   ImBX2zCQS00Ww4dvXrasZoJYxX2JDBxN
```
