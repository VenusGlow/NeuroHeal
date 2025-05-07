NeuroHeal Deployment Guide

This README provides step-by-step instructions to clone, install, configure, and deploy the NeuroHeal MVP on Vercel.

1. Clone the repository




---

git clone https://github.com/VenusGlow/NeuroHeal.git
cd NeuroHeal

2. Install dependencies




---

Ensure you have Node.js (v16+) and npm installed.

npm install

3. Configure environment variables




---

Create a .env.local file in the project root with the following keys:

# Supabase settings (from your Supabase dashboard)
NEXT_PUBLIC_SUPABASE_URL=https://<your-project-ref>.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=<your-anon-public-key>

# OpenAI API key (server-side only)
OPENAI_API_KEY=<your-openai-api-key>

# Vercel environment
# (optional) Add any additional secrets or keys here

4. Initialize Supabase (if not already set up)




---

In your Supabase SQL editor, run:

CREATE TABLE public.messages (
  id uuid PRIMARY KEY DEFAULT uuid_generate_v4(),
  session_id text NOT NULL,
  role text NOT NULL,
  content text NOT NULL,
  created_at timestamptz NOT NULL DEFAULT now()
);

ALTER TABLE public.messages ENABLE ROW LEVEL SECURITY;

CREATE POLICY allow_insert_messages
  ON public.messages
  FOR INSERT
  WITH CHECK ( true );

CREATE POLICY allow_select_messages
  ON public.messages
  FOR SELECT
  USING ( true );

5. Local development




---

Run the development server:

npm run dev

Visit http://localhost:3000/app to test:

Responsive navigation across breakpoints

Sound therapy player in Sounds tab

Chat with “Haven” in Chat tab (store messages in Supabase)


6. Build for production




---

npm run build
npm run start

7. Deploy to Vercel




---

If you haven’t already, install the Vercel CLI:

npm install -g vercel

Login and link your project:

vercel login
vercel link # Choose existing project or create new

Deploy to production:

vercel --prod

8. Post-Deployment Verification




---

Confirm your live URL loads /app correctly

Test navigation, mobile layout, and UI responsiveness

Verify audio tracks loop and play in background

Send messages in the Chat tab and ensure Haven replies


9. Troubleshooting




---

Blank or 404 pages: Ensure .env values are set on Vercel (Dashboard → Settings → Environment Variables).

Supabase auth errors: Confirm RLS policies match your setup and keys are correct.

OpenAI errors: Verify OPENAI_API_KEY has sufficient permissions and is set server-side only.

Audio playback issues: Host MP3s in Supabase Storage or a public CDN with CORS enabled.
