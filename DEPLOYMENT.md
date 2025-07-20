# Deployment Guide - Matcha Café POS v16

## 🎯 Current Version Features
- ✅ Supabase real-time database
- ✅ Cross-device order syncing
- ✅ Matcha-themed UI (emerald/yellow)
- ✅ Sweetness customization in cart
- ✅ Single special instructions field
- ✅ Real-time barista dashboard
- ✅ Mobile responsive design

## 🚀 Quick Deploy Steps

### 1. Get the Latest Code
- Download from v0 interface (this ensures you get v16)
- Or clone: `git clone [your-repo-url]`

### 2. Environment Variables (REQUIRED)
Add these to your deployment platform:

\`\`\`env
NEXT_PUBLIC_SUPABASE_URL=your_supabase_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_supabase_service_role_key
\`\`\`

### 3. Database Setup
Run this SQL in Supabase SQL Editor:

\`\`\`sql
CREATE TABLE IF NOT EXISTS orders (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  customer_name TEXT NOT NULL,
  items JSONB NOT NULL,
  notes TEXT DEFAULT '',
  status TEXT DEFAULT 'pending' CHECK (status IN ('pending', 'completed')),
  created_at TIMESTAMPTZ DEFAULT NOW()
);

ALTER TABLE orders ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow all operations on orders" ON orders FOR ALL USING (true);
ALTER PUBLICATION supabase_realtime ADD TABLE orders;
\`\`\`

### 4. Deploy Platforms

#### Vercel (Recommended)
1. `npm i -g vercel`
2. `vercel --prod`
3. Add environment variables in dashboard

#### Netlify
1. Connect GitHub repo
2. Build: `npm run build`
3. Publish: `.next`

## 🔍 Version Verification

After deployment, check these features work:
- [ ] Customer can place orders
- [ ] Orders appear instantly on barista dashboard
- [ ] Sweetness options in cart (not popup)
- [ ] Single notes field for entire order
- [ ] Real-time status shows "Real-time" not "Polling"
- [ ] Matcha green/yellow theme throughout

## 🐛 Troubleshooting

**Wrong version deployed?**
- Re-download code from current v0 session
- Check package.json version is 1.0.0
- Verify Supabase integration exists

**Orders not syncing?**
- Check environment variables are set
- Verify Supabase table exists
- Check browser console for errors
