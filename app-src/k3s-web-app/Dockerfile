# Build stage
FROM node:20 as build

# ---- FIX IS HERE ----
# 1. Declare the build-time argument that will be passed from Jenkins
ARG GEMINI_API_KEY

# 2. Set it as an environment variable *within this build stage*
ENV GEMINI_API_KEY=${GEMINI_API_KEY}

# 3. (Optional but good practice) Make sure the key is not empty before proceeding
RUN if [ -z "$GEMINI_API_KEY" ]; then echo "Error: GEMINI_API_KEY is not set." && exit 1; fi
# ---- END OF FIX ----

WORKDIR /app
COPY . .

# The build process can now access the $GEMINI_API_KEY environment variable
RUN npm install && npm run build


# --- Final Stage ---
# This stage serves the built static files
FROM nginx:alpine

# Copy only the compiled assets from the 'build' stage
COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]