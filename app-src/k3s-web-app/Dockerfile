# Set the environment variable for the Gemini API key
ENV GEMINI_API_KEY=${GEMINI_API_KEY}

# Make sure the key is not empty
RUN if [ -z "$GEMINI_API_KEY" ]; then echo "Error: GEMINI_API_KEY is not set." && exit 1; fi

# Build stage
FROM node:20 as build
WORKDIR /app
COPY . .
RUN npm install && npm run build

# Serve with Nginx
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]