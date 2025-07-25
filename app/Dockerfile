FROM node:20-alpine AS devtechtest_build

WORKDIR /app

# Copy package files and install all dependencies
COPY package*.json ./
RUN npm i

# Copy source and build app
COPY . .
RUN npm run compile

# Stage 2: Production runtime
FROM node:20-alpine

WORKDIR /app

# Copy only production files from build stage
COPY package*.json ./
RUN npm i --omit=dev

# Create non-root user for security
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

COPY --from=devtechtest_build /app/dist ./dist

# Copy config folder
COPY --from=devtechtest_build /app/config ./config

ENV NODE_ENV=production
ENV APP_PORT=9002

EXPOSE 9002

CMD ["node", "dist/index.js"]