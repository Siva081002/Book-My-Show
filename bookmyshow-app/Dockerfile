# ---- Stage 1: The "Factory" - Where We Build the App ----
 
# We're starting with a slim version of Node.js 18. Think of this as our temporary workshop.
# The 'AS builder' part gives this stage a name we can refer to later on.
FROM node:18-alpine AS builder
 
# It's good practice to set a working directory so we're not just dumping files in the root.
WORKDIR /app
 
# Now, we copy over the package files first. This is a neat Docker trick. If these files don't change,
# Docker is smart enough to skip re-installing all the dependencies on future builds, which saves a ton of time.
COPY package.json package-lock.json ./
 
# You mentioned needing this for some older dependencies. We'll set it here so the install process doesn't trip up.
ENV NODE_OPTIONS=--openssl-legacy-provider
 
# Here, we run your specific PostCSS fix first, then the main install. This makes sure all the
# project dependencies are installed correctly inside our little workshop.
RUN npm install postcss@8.4.21 postcss-safe-parser@6.0.0 --legacy-peer-deps
RUN npm install
 
# Okay, with the dependencies ready, we can copy over all of our actual source code (the 'public' and 'src' folders, etc.).
COPY . .
 
# This is the magic command. 'npm run build' tells React to bundle everything up into a small,
# super-optimized package of static files. This is the final product we'll actually serve to users.
RUN npm run build
 
 
# ---- Stage 2: The "Final Product" - Where We Serve the App ----
 
# Now we start fresh with a tiny Nginx web server. This thing is purpose-built for one job:
# serving static files really, really fast. No Node.js, no source code, just the essentials.
FROM nginx:stable-alpine
 
# Here's where we connect the two stages. We're copying the '/app/build' folder (the optimized output)
# from our 'builder' stage into the default folder where Nginx serves files from.
COPY --from=builder /app/build /usr/share/nginx/html
 
# We're just telling Docker to open up port 80 on the container so Nginx can receive traffic.
EXPOSE 80
 
# This command starts the Nginx server. The '-g "daemon off;"' part is important; it tells Nginx
# to run in the foreground so the Docker container stays alive.
CMD ["nginx", "-g", "daemon off;"]
