# ----------- 1. Build Stage -----------
FROM node:18-alpine AS builder

# Set working directory
WORKDIR /app

# Install dependencies
COPY package*.json ./
RUN npm install

# Copy the rest of the app
COPY . .

# Build the Next.js app
RUN npm run build

# ----------- 2. Production Stage -----------
FROM node:18-alpine AS runner

WORKDIR /app

# Only copy necessary files
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/package.json ./package.json

# Install only production deps
RUN npm install --omit=dev

# Expose port
EXPOSE 3000

# Start the Next.js app
CMD ["npm", "start"]
