FROM node:22-alpine AS builder
RUN corepack enable && corepack prepare pnpm@latest --activate
WORKDIR /app
COPY package.json pnpm-lock.yaml pnpm-workspace.yaml ./
RUN pnpm install --frozen-lockfile
COPY . .
ARG BASE_HREF=/
ARG PROJECT_NAME=test
RUN pnpm build:libs && pnpm exec ng build $PROJECT_NAME --configuration=production --base-href="${BASE_HREF}"

FROM nginx:1.27-alpine AS runtime
COPY --from=builder /app/projects/$PROJECT_NAME/docker/nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /app/dist/$PROJECT_NAME/browser /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
