# syntax=docker/dockerfile:1.4

ARG NODE_VERSION=22.1.0
ARG PNPM_VERSION=9.0.6

FROM node:${NODE_VERSION}-alpine

WORKDIR /app

COPY package*.json pnpm-lock.yaml ./

# Install pnpm
RUN --mount=type=cache,target=/root/.npm \
    npm install -g pnpm@${PNPM_VERSION}

# Install deps
RUN --mount=type=bind,source=package.json,target=package.json \
    --mount=type=bind,source=pnpm-lock.yaml,target=pnpm-lock.yaml \
    --mount=type=cache,target=/root/.local/share/pnpm/store \
    pnpm install --prod --frozen-lockfile

COPY . .

ARG PORT
ARG FRONTEND_PORT
ENV NODE_ENV=production
ENV HOST=0.0.0.0

RUN pnpm build

EXPOSE ${FRONTEND_PORT}

ENTRYPOINT ["node", "./dist/server/entry.mjs"]
