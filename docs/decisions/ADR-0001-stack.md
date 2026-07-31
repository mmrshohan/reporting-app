# ADR-0001 — Mobile-only Fastify foundation

- **Status:** Superseded by [ADR-0002](ADR-0002-platform-stack.md)
- **Date:** 2026-07-06
- **Superseded:** 2026-08-01

## Original decision

The original foundation selected a mobile-only bare React Native client, WatermelonDB, Fastify, Drizzle, Supabase PostgreSQL, and Supabase Auth.

## Why it was superseded

Product requirements changed materially:

- Web became a first-class client.
- The product became commercial and multi-tenant, supporting personal and organization workspaces.
- Speech-to-text moved into the initial product.
- The owner chose a self-hosted PostgreSQL and authentication path instead of Supabase.
- FastAPI was selected as the independent core API for web and mobile.
- Expo development builds were selected to reduce native maintenance for a small team.

This ADR remains as decision history. It must not be used as current implementation guidance.
