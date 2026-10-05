# SparkleClean Pro: website + admin (Netlify + Supabase)

Static site with no build step. Public site at `/`, owner dashboard at `/admin`. All content, images, bookings and version history live in Supabase.

## 1. Create a Supabase project
Sign in at supabase.com, click **New project**, choose a name, region and database password, and wait until it finishes.

## 2. Run the database schema
Open **SQL Editor > New query**, paste all of `supabase/schema.sql`, click **Run**. This creates the tables, the security rules and a public image bucket called `media`. Check **Storage** shows the `media` bucket. If it is missing, create a bucket named `media` and tick **Public bucket**, then re-run the last four lines of the schema.

## 3. Create the owner login
1. **Authentication > Users > Add user > Create new user**: enter your email and a strong password, tick **Auto Confirm User**.
2. **Authentication > Sign In / Providers**: turn OFF "Allow new users to sign up".
3. In **SQL Editor** run (use your email):
```sql
insert into admins(user_id) select id from auth.users where email='you@example.com';
```

## 4. Get your keys
**Project Settings > API**: copy the **Project URL** and the **anon public** key. Never put the service_role key anywhere.

## 5. Deploy to Netlify
1. Unzip, upload the folder to a new GitHub repository.
2. Netlify > **Add new site > Import an existing project**, choose the repo. Leave the build command empty and the publish directory as `.` (`netlify.toml` sets this). Netlify Functions are needed, so drag-and-drop deploys of the folder may not work; use Git, or the Netlify CLI (`netlify deploy --prod`).
3. **Site configuration > Environment variables**: add `SUPABASE_URL` and `SUPABASE_ANON_KEY` (see `.env.example`).
4. **Deploys > Trigger deploy** so the variables take effect.

## 6. Sign in
Open `https://your-site.netlify.app/admin` and log in with the owner email and password.

## 7. Edit and publish
Edit any tab, click **Save Draft**, **Preview** (shows your draft), then **Publish Changes**. Visitors see the change on reload with no redeploy. **History** restores older published versions. **Media** manages images.

## Notes
- Before the first publish, the site shows built-in starter content. Publishing replaces it.
- Bookings and quote requests from visitors appear in the **Bookings** tab. Payments are not processed.
- Add a custom domain in Netlify > Domain management.
