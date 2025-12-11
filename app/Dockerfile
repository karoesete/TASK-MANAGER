# Use official Node.js 18 LTS (Alpine for small size)
FROM node:18-alpine

# Set working directory inside container
WORKDIR /usr/src/app

# Create non-root user for security
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nextuser -u 1001

# Copy package files first (to leverage Docker layer caching)
COPY package*.json ./

# Install dependencies (omit dev dependencies for production)
RUN npm ci --omit=dev && npm cache clean --force

# Copy application code
COPY . .

# Change ownership to non-root user
RUN chown -R nextuser:nodejs .
USER nextuser

# Expose the port the app runs on
EXPOSE 3000

# Health check (optional but recommended)
#HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
#  CMD wget -qO- http://localhost:3000/health || exit 1

# Run the application
CMD ["node", "server.js"]