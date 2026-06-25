-- STREAMING_CHUNK: Setting up tables...
-- =========================================================================
-- CLEANUP (Optional: Remove tables if they exist to start fresh)
-- =========================================================================
DROP TABLE IF EXISTS public.orders;
DROP TABLE IF EXISTS public.feedback;
DROP TABLE IF EXISTS public.vibes;
DROP TABLE IF EXISTS public.bookings;

-- =========================================================================
-- CREATE TABLE STRUCTURES WITH ALL SYSTEM SCHEMAS
-- =========================================================================

-- 1. Curbside Pre-Orders Table
CREATE TABLE public.orders (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    customer_name TEXT NOT NULL DEFAULT 'Boutique Visitor',
    items JSONB NOT NULL,                          -- Stores the cart array
    total_price NUMERIC(10, 2) NOT NULL DEFAULT 0.00,
    status TEXT NOT NULL DEFAULT 'Pending',         -- 'Pending', 'Order Done', 'Cancelled'
    timestamp TIMESTAMPTZ DEFAULT timezone('utc'::text, now()) NOT NULL
);

-- 2. Community Feedback & Suggestions Table
CREATE TABLE public.feedback (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    handle TEXT NOT NULL,
    favorite_product TEXT NOT NULL,
    comment TEXT NOT NULL,
    approved BOOLEAN DEFAULT false NOT NULL,
    timestamp TIMESTAMPTZ DEFAULT timezone('utc'::text, now()) NOT NULL
);

-- 3. Vibe Board Uploads Table
CREATE TABLE public.vibes (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    handle TEXT NOT NULL,
    ordered_item TEXT NOT NULL,
    image_url TEXT NOT NULL,
    timestamp TIMESTAMPTZ DEFAULT timezone('utc'::text, now()) NOT NULL
);

-- 4. Lounge Room Bookings Table
CREATE TABLE public.bookings (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    name TEXT NOT NULL,
    guests INTEGER NOT NULL DEFAULT 1,
    arrival_time TEXT NOT NULL,
    zone TEXT NOT NULL,
    timestamp TIMESTAMPTZ DEFAULT timezone('utc'::text, now()) NOT NULL
);

-- =========================================================================
-- ENABLE ROW LEVEL SECURITY (RLS)
-- =========================================================================
ALTER TABLE public.orders ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.feedback ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.vibes ENABLE ROW LEVEL SECURITY;
ALTER TABLE public.bookings ENABLE ROW LEVEL SECURITY;

-- =========================================================================
-- CONFIGURE RLS SECURITY POLICIES FOR ALL ROLES (ANON & AUTHENTICATED)
-- =========================================================================

-- POLICIES FOR 'orders'
CREATE POLICY "Allow public read access to orders" ON public.orders FOR SELECT USING (true);
CREATE POLICY "Allow public insert access to orders" ON public.orders FOR INSERT WITH CHECK (true);
CREATE POLICY "Allow public update access to orders" ON public.orders FOR UPDATE USING (true) WITH CHECK (true); 

-- POLICIES FOR 'feedback'
CREATE POLICY "Allow public read access to feedback" ON public.feedback FOR SELECT USING (true);
CREATE POLICY "Allow public insert access to feedback" ON public.feedback FOR INSERT WITH CHECK (true);
CREATE POLICY "Allow public update access to feedback" ON public.feedback FOR UPDATE USING (true) WITH CHECK (true);
CREATE POLICY "Allow public delete access to feedback" ON public.feedback FOR DELETE USING (true);

-- POLICIES FOR 'vibes'
CREATE POLICY "Allow public read access to vibes" ON public.vibes FOR SELECT USING (true);
CREATE POLICY "Allow public insert access to vibes" ON public.vibes FOR INSERT WITH CHECK (true);
CREATE POLICY "Allow public update access to vibes" ON public.vibes FOR UPDATE USING (true) WITH CHECK (true);
CREATE POLICY "Allow public delete access to vibes" ON public.vibes FOR DELETE USING (true);

-- POLICIES FOR 'bookings'
CREATE POLICY "Allow public read access to bookings" ON public.bookings FOR SELECT USING (true);
CREATE POLICY "Allow public insert access to bookings" ON public.bookings FOR INSERT WITH CHECK (true);
CREATE POLICY "Allow public delete access to bookings" ON public.bookings FOR DELETE USING (true);