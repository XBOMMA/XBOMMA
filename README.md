# XBomma — Movies, Web Series & Videos

Production-ready full-stack streaming site with a **separate ad-free admin panel**.

## Features

- **Public site**: Home, Movies, Web Series, Videos, Search, Watch player
- **Admin panel** (`/admin`): CRUD for movies / series / videos, genres, seasons & episodes
- **Posters via Pinterest**: paste pin URL → Resolve → poster image fetched from `og:image`
- **Video sources**: direct `.mp4` URLs **or** VdoHide / fembed iframe embed codes
- **Ads on public pages only** (popunder, native banner, social bar, 300×250) — **never on `/admin`**
- **Download App** button in the header
- Auth-protected admin (NextAuth credentials)
- SQLite + Prisma (easy local / VPS deploy; swap to Postgres when needed)
- SEO: metadata, sitemap, robots

## Quick start

```bash
# 1. Install
npm install

# 2. Configure env (already created — change secrets before production)
#    .env → NEXTAUTH_SECRET, ADMIN_EMAIL, ADMIN_PASSWORD, NEXTAUTH_URL

# 3. Database + seed sample content
npm run db:setup

# 4. Dev server
npm run dev
```

Open:

| URL | Purpose |
|-----|---------|
| http://localhost:3000 | Public site |
| http://localhost:3000/admin | Admin panel |

**Default admin** (from `.env`):

- Email: `admin@xbomma.com`
- Password: `Admin@123456`

## Production

```bash
npm run build
npm start
```

Set on your host:

- `NEXTAUTH_URL` = your public HTTPS URL
- `NEXTAUTH_SECRET` = long random string
- Strong `ADMIN_PASSWORD`
- Optional: change `DATABASE_URL` to PostgreSQL and update `prisma/schema.prisma` `provider`

## Admin workflow

1. Sign in at `/admin/login`
2. **Add content** → choose Movie / Series / Video
3. Paste **Pinterest pin URL** → **Resolve** for poster
4. For movies/videos: paste **embed iframe** or **direct video URL**
5. For series: save first, then add **seasons & episodes** (each episode has its own embed/URL)
6. Toggle Featured / Published

## Ads placement

| # | Script | Where |
|---|--------|--------|
| 1 | Popunder (`benchform.org/1/…`) | Public layout (once per page) |
| 2 | Native banner | Home, listing, watch pages |
| 3 | Social / sticky (`biomanos.org/14/…`) | Public layout (end of body) |
| 4 | 300×250 banner | Home, listings, watch sidebar |
| 5 | Download App | Header → `NEXT_PUBLIC_DOWNLOAD_APP_URL` |

Admin routes under `/admin/**` intentionally **do not** load any ad components.

## Project structure

```
src/app/(public)/     Public pages + ads layout
src/app/admin/        Ad-free admin
src/app/api/          REST APIs (content, seasons, episodes, genres, pinterest)
src/components/ads/   Ad scripts (public only)
prisma/               Schema + seed
```

## Notes

- Embed codes are sanitized to a single allowlisted `https` iframe `src`
- Middleware protects `/admin` (except `/admin/login`)
- Change default credentials before going live
