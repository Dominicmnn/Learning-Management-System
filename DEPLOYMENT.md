# Deployment

This repository is prepared for a Vercel frontend and Render Django backend.

## GitHub

Push the repository to GitHub, including `frontend/package-lock.json`:

```powershell
git add .
git commit -m "Prepare Vercel and Render deployment"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
git push -u origin main
```

Do not commit `.env`, `db.sqlite3`, `media/`, `staticfiles/`, or `node_modules/`.

## Render backend

1. In Render, choose **New > Blueprint** and select the GitHub repository.
2. Render will read `render.yaml`, create the PostgreSQL database, and create the web service.
3. Set `ALLOWED_HOSTS` to the backend hostname, for example `shire-jama-api.onrender.com`.
4. Set `CORS_ALLOWED_ORIGINS` and `CSRF_TRUSTED_ORIGINS` to the final Vercel URL, for example `https://shire-jama.vercel.app`.
5. Deploy and open `/admin/` on the backend URL.
6. Create an administrator from the Render service shell:

```text
python manage.py createsuperuser
```

The Render build runs migrations and collects static files automatically.

## Vercel frontend

1. In Vercel, import the GitHub repository.
2. Set **Root Directory** to `frontend`.
3. Use `npm run build` as the build command.
4. Use `dist` as the output directory.
5. Add `VITE_API_URL=https://YOUR_RENDER_SERVICE.onrender.com/api`.
6. Deploy.

`frontend/vercel.json` keeps client-side routes working after refresh.

## Important application note

The current frontend service in `frontend/src/services/api.ts` is still a local mock implementation using `localStorage`. The deployment will work, but it will not use the Django database until that service is replaced with requests to `VITE_API_URL`. Uploaded media also needs object storage such as Cloudflare R2 or Amazon S3; Render's local filesystem is not permanent.