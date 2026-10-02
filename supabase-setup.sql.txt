-- الصق الكود ده في Supabase > SQL Editor ثم Run
create table public.orders (
  id uuid primary key default gen_random_uuid(),
  created_at timestamptz not null default now(),
  order_no text not null,
  name text not null check (char_length(name) between 2 and 120),
  phone text not null check (char_length(phone) between 8 and 20),
  email text check (char_length(email) <= 120),
  gov text not null check (char_length(gov) <= 60),
  city text not null check (char_length(city) <= 120),
  addr text not null check (char_length(addr) between 3 and 400),
  notes text check (char_length(notes) <= 400),
  qty int not null check (qty between 1 and 5),
  payment text not null check (char_length(payment) <= 60),
  total int not null check (total between 1 and 100000),
  status text not null default 'new'
);

alter table public.orders enable row level security;

-- الزوار يقدروا يضيفوا حجز فقط (مش يقروا ولا يعدلوا ولا يمسحوا)
create policy "anon can insert orders"
  on public.orders for insert to anon
  with check (true);

-- القراءة والتعديل متاحين لك انت بس من لوحة Supabase (Table Editor)
