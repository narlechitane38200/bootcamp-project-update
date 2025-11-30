### Build and Test (7 Points)

Depuis mon repo git perso, j'ai ajouté un fichier .env (contenant les différentes variables sensibles) et un fichier .gitignore (qui contient le .env pour éviter qu'il ne soit copié lors du git clone). Aussi, j'ai ajouté les répertoires initdb et les fichiers 
Ensuite depuis une session Docker Playground, j'ai cloné le repo mini projet docker en local.

#### Database Initialization
The database schema is initialized using the initdb directory, which contains SQL scripts to set up the required tables and initial data. These scripts are automatically executed when the MySQL container starts.

#### Extra Challenges (Optional)
Secure Sensitive Information: Avoid hardcoding sensitive data such as database credentials directly in your Dockerfile. Instead, use Docker secrets or .env files to manage them securely. These environment variables can be set dynamically at runtime to protect sensitive information:

```bash
# Environment variables for database connection
# Do not hardcode credentials; use secrets or environment files instead.

# ENV SPRING_DATASOURCE_USERNAME  # Database username
# ENV SPRING_DATASOURCE_PASSWORD  # Database password
# ENV SPRING_DATASOURCE_URL       # Database connection URL
```

User Authentication: Add user authentication to the backend to restrict access to the API and transactions.

1. **Backend Dockerfile:**
   - Base image: `amazoncorretto:17-alpine`
   - Copy backend JAR file and expose port 8080
   - CMD: Run the backend service
   
2. **Database Setup:**
   - Use MySQL as a Docker service, mounting the data to a persistent volume
   - Expose port 3306

### Orchestration with Docker Compose (5 Points)

The `docker-compose.yml` will deploy both services:
- **paymybuddy-backend:** Runs the Spring Boot application.
- **paymybuddy-db:** MySQL database to handle user data and transactions.

Key features:
- Services depend on each other for smooth orchestration
- Volumes for persistent storage
- Environment variables for secure configuration

---

## Docker Registry (4 Points)

You need to push your built images to a private Docker registry and deploy the images using Docker Compose.

### Steps:
1. Build the images for both backend and MySQL.
2. Deploy a private Docker registry.
3. Push your images to the registry and use them in `docker-compose.yml`.

---

## Delivery (4 Points)

For your delivery, provide the following in your repository:

- **README** with screenshots and explanations.
- **Dockerfile** and **docker-compose.yml**.
- **Screenshots** showing the application running.
  
Your delivery will be evaluated based on:
- Quality of explanations and screenshots
- Repository structure and clarity

**Good luck!**

**![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc-CjKFk4NY9yXiR1oheHsFR4YYn4HcD_0A6fgd11tHcT3p1U2RKXvIs6HflkvuLOOUzFxzxYCjDno2f1p6_q31dDE9AaUoEx1pi0Fs9ApJG2czL-88xrx3XO-oEP5ZXXsyXw0GKjA2W0A5q1Bk979SB1M?key=mLqAl_ccMoG4hHcRzSYKpw)**

