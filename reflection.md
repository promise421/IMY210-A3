# Reflection - IMY210 Assignment 3

## GitHub Repository
https://github.com/promise421/IMY210-A3

## Commands to Run

### Frontend
docker build -t blog-frontend ./frontend
docker run -p 3000:3000 blog-frontend

### Backend
docker build -t blog-backend ./backend
docker run -p 1337:1337 blog-backend

## My Experience

Working on this assignment introduced me to tools I had never used before, 
specifically Docker and Strapi. Setting up Docker on Windows was the first 
challenge I faced, as I had to install WSL2 and troubleshoot network errors 
before Docker Desktop would run properly.

Strapi was surprisingly easy to use once it was running. Creating the Post 
collection type and populating it with content through the admin panel was 
straightforward. Connecting the Strapi API to NuxtJS taught me how modern 
JAMstack applications communicate between the frontend and backend.

The biggest challenge was structuring the NuxtJS project correctly, as Nuxt 4 
uses a different folder structure than previous versions. I also learned how 
Dockerfiles work and how to containerize both a frontend and backend application 
separately.

Overall this assignment gave me a solid understanding of how modern web 
applications are built, deployed and managed using industry standard tools.