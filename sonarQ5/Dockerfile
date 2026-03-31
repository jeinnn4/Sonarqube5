# Use official Node.js Alpine image
FROM node:18-alpine

WORKDIR /app

# Copy dependency files first (layer caching)
COPY package*.json ./

# Install production dependencies only
RUN npm install --production

# Copy all project files
COPY . .

# Create non-root user (keeps Trivy scan clean)
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
RUN chown -R appuser:appgroup /app
USER appuser

# Expose port (check your .env or server.js for the actual port)
EXPOSE 3000

# Health check
HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
    CMD wget -qO- http://localhost:3000 || exit 1

# Start with server.js (not index.js)
CMD ["node", "server.js"]