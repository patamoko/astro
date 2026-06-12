# =============================================================================
# astro-koharu Dockerfile (Optimized)
# Multi-stage build for static site generation with nginx
# =============================================================================

# =============================================================================
# Stage 1: Builder
# =============================================================================
FROM node:22-alpine AS builder
WORKDIR /app

# Install pnpm
RUN corepack enable && corepack prepare pnpm@9.15.1 --activate

# Copy package files first (for Docker layer caching)
COPY package.json pnpm-lock.yaml ./

# Install dependencies (include dev deps for build)
RUN pnpm install --frozen-lockfile --prod=false

# Copy source files
COPY . .

# Build the static site
RUN pnpm build

# =============================================================================
# Stage 2: Production (nginx)
# =============================================================================
FROM nginx:alpine AS production

# Copy nginx configuration
COPY docker/nginx/default.conf /etc/nginx/conf.d/default.conf

# Copy built static files
COPY --from=builder /app/dist /usr/share/nginx/html

# Pre-compress static assets with gzip for nginx gzip_static
# This avoids re-compressing on every request
RUN find /usr/share/nginx/html -type f \( -name "*.html" -o -name "*.css" -o -name "*.js" -o -name "*.xml" -o -name "*.svg" -o -name "*.json" \) -exec gzip -9 -k -f {} \;

# Expose port 80
EXPOSE 80

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost/ || exit 1

# Start nginx
CMD ["nginx", "-g", "daemon off;"]
