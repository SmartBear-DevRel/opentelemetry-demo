# Copyright The OpenTelemetry Authors
# SPDX-License-Identifier: Apache-2.0

#   Creating an optimized production build ...
# ⚠ Attempted to load @next/swc-linux-arm64-gnu, but it was not installed
# ⚠ Attempted to load @next/swc-linux-arm64-musl, but an error occurred: /lib/aarch64-linux-gnu/libc.so: invalid ELF header
# ⨯ Failed to load SWC binary for linux/arm64, see more info here: https://nextjs.org/docs/messages/failed-loading-swc
FROM node:20-alpine AS builder

WORKDIR /app

COPY ./src/flagd-ui/package*.json ./

RUN npm ci

COPY ./src/flagd-ui/. ./

RUN npm run build

# -----------------------------------------------------------------------------

FROM node:20-alpine

WORKDIR /app

COPY ./src/flagd-ui/package*.json ./

RUN npm ci --only=production

COPY --from=builder /app/src/instrumentation.ts ./instrumentation.ts
COPY --from=builder /app/next.config.mjs ./next.config.mjs

COPY --from=builder /app/.next ./.next

EXPOSE 4000

CMD ["npm", "start"]
