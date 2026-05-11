# TaskFlow Pro

## Overview

A productivity app that combines a to-do list manager, habit tracker with streaks, smart reminders, and a daily dashboard with weekly trends and recent activity.

## Stack

- **Monorepo**: pnpm workspaces, TypeScript 5.9
- **Frontend**: React + Vite (artifacts/taskflow), Tailwind v4, wouter routing, TanStack Query, framer-motion, recharts, sonner
- **Backend**: Express 5 API server (artifacts/api-server) with pino logging
- **Database**: PostgreSQL via Drizzle ORM (lib/db)
- **API contract**: OpenAPI 3.1 (lib/api-spec) — Orval generates React Query hooks (lib/api-client-react) and Zod schemas (lib/api-zod)

## Domain Models (lib/db/src/schema/)

- `tasksTable` — id, title, description, category, priority, dueDate, status, completedAt
- `habitsTable` — id, name, description, icon, color, frequency, difficulty, bestStreak
- `habitLogsTable` — habitId, date, completed (unique on habitId+date)
- `remindersTable` — id, title, remindAt, repeat, linkedTaskId, linkedHabitId

## Routes (artifacts/api-server/src/routes/)

- `tasks.ts` — CRUD + `/tasks/:id/toggle`
- `habits.ts` — CRUD + `/habits/:id/check-in`, `/habits/:id` returns 30-day log + completion rate
- `reminders.ts` — CRUD
- `dashboard.ts` — `/dashboard/summary`, `/dashboard/weekly`, `/dashboard/activity`

Streak computation lives in `artifacts/api-server/src/lib/dates.ts`.

## Key Commands

- `pnpm run typecheck` — full typecheck
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks/schemas after editing openapi.yaml
- `pnpm --filter @workspace/db run push` — push DB schema changes
- `pnpm --filter @workspace/scripts run seed` — seed example tasks, habits with streaks, and reminders
