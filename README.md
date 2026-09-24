# Dorm Parcel Frontend

Static frontend for the dormitory parcel management system. Data is stored
in MongoDB through the backend API (Hugging Face Space).

## How saving works

Each change sends **only the one record that changed**, so two people using
the app at the same time no longer overwrite each other's work:

| Action | Request |
| --- | --- |
| Load all records | `GET /api/records` |
| Add a record | `POST /api/records` |
| Edit / tick items / received all / delivered / delete | `PATCH /api/records/:id` |

The old `PUT /api/records` (replace the whole set) has been removed from the
backend, because it caused one device's stale copy to overwrite another
device's changes.

Each record can also store an optional LINE name (`lineName`), shown under
the person's name in the detail view with a copy button.

The "รอส่งของ" (pending) and "ประวัติ" (history) tabs have a search box that
filters by name, LINE name, room or tracking number.

## Configure the backend URL

Edit `config.js`:

```js
window.API_BASE_URL = 'https://your-username-dorm-parcel-backend.hf.space';
```

## Deploying

The frontend and backend must be updated **together**: the new frontend
calls `POST` / `PATCH`, which only exist in the new `server.js`.

1. Backend: replace `server.js` in the Hugging Face Space and let it rebuild.
2. Frontend:

```bash
npm i -g vercel   # if you don't have it
cd frontend
vercel --prod
```

Or push this folder to a Git repo and import it in the Vercel dashboard
(Framework Preset: **Other** / static).
