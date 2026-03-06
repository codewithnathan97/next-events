# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Next-Events is a Next.js 14 event listing web app built as part of the _Beginning Next.js Development_ book. It uses the App Router with JavaScript (not TypeScript).

## Commands

- `npm run dev` — start dev server at localhost:3000
- `npm run build` — production build
- `npm run lint` — ESLint via next lint

No test framework is configured.

## Architecture

**App Router structure** (`app/`):
- `page.js` — home page listing all events with search
- `login/`, `signup/` — auth pages
- `profile/` — user profile (protected)
- `events/create/` — create event (protected)
- `events/[id]/` — event detail
- `events/[id]/update/` — edit event (protected)
- `api/auth/[...nextauth]/` — NextAuth credentials provider
- `api/uploadthing/` — file upload endpoint

**Key patterns:**
- Server Actions in `lib/actions/` (user.action.js, event.action.js) handle all DB mutations. They call `dbConnect()` then use Mongoose models, and return `JSON.parse(JSON.stringify(...))` to serialize Mongoose docs.
- `lib/dbConnect.js` — cached MongoDB connection using global singleton pattern
- `lib/models/` — Mongoose schemas: `User` (email, name, password, imageUrl) and `Event` (title, description, location, dates, imageUrl, url, category, organizer ref)
- Auth: NextAuth v4 with credentials provider (`authConfig.js`), bcrypt password hashing
- Middleware (`middleware.js`) protects `/profile`, `/events/create`, `/events/:id/update`
- Components are in `components/` (flat, no nesting) — forms use react-hook-form

**Styling:** Tailwind CSS + DaisyUI. Tailwind config wraps with `withUt()` for Uploadthing components.

**Image uploads:** Uploadthing (`lib/uploadthing.js`, `components/ImageUploader.js`)

## Environment Variables

See `.env.sample`: `MONGODB_URI`, `NEXTAUTH_SECRET`, `NEXTAUTH_URL`, `UPLOADTHING_SECRET`, `UPLOADTHING_APP_ID`

## Key Dependencies

- next 14.1.0, react 18, next-auth 4, mongoose 8, bcrypt, uploadthing, react-hook-form, react-datepicker, react-hot-toast, daisyui
