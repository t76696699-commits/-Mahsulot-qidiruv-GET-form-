Restoran statistikasi moduli
Python
from collections import defaultdict

# 1. 15+ buyurtmadan iborat ma'lumotlar to'plami
orders = [
    {"ofitsiant": "Ali", "taom": "Osh", "narx": 45000, "sana": "2026-07-01", "kategoriya": "Milliy"},
    {"ofitsiant": "Vali", "taom": "Shashlik", "narx": 30000, "sana": "2026-07-01", "kategoriya": "Gril"},
    {"ofitsiant": "Ali", "taom": "Sho'rva", "narx": 25000, "sana": "2026-07-01", "kategoriya": "Sho'rva"},
    {"ofitsiant": "Guli", "taom": "Pizza", "narx": 60000, "sana": "2026-07-02", "kategoriya": "Fast-food"},
    {"ofitsiant": "Vali", "taom": "Lag'mon", "narx": 35000, "sana": "2026-07-02", "kategoriya": "Milliy"},
    {"ofitsiant": "Ali", "taom": "Steak", "narx": 80000, "sana": "2026-07-02", "kategoriya": "Gril"},
    {"ofitsiant": "Guli", "taom": "Osh", "narx": 45000, "sana": "2026-07-03", "kategoriya": "Milliy"},
    {"ofitsiant": "Ali", "taom": "Salat", "narx": 15000, "sana": "2026-07-03", "kategoriya": "Salat"},
    {"ofitsiant": "Vali", "taom": "Burger", "narx": 25000, "sana": "2026-07-03", "kategoriya": "Fast-food"},
    {"ofitsiant": "Guli", "taom": "Shashlik", "narx": 30000, "sana": "2026-07-03", "kategoriya": "Gril"},
    {"ofitsiant": "Ali", "taom": "Lag'mon", "narx": 35000, "sana": "2026-07-04", "kategoriya": "Milliy"},
    {"ofitsiant": "Vali", "taom": "Pizza", "narx": 60000, "sana": "2026-07-04", "kategoriya": "Fast-food"},
    {"ofitsiant": "Guli", "taom": "Sho'rva", "narx": 25000, "sana": "2026-07-04", "kategoriya": "Sho'rva"},
    {"ofitsiant": "Ali", "taom": "Osh", "narx": 45000, "sana": "2026-07-04", "kategoriya": "Milliy"},
    {"ofitsiant": "Vali", "taom": "Steak", "narx": 80000, "sana": "2026-07-04", "kategoriya": "Gril"}
]

# 2. Set comprehension: Unique ofitsiantlar
unique_waiters = {order['ofitsiant'] for order in orders}

# 3. Dict comprehension: Ofitsiant -> Jami summa
waiter_performance = {w: sum(o['narx'] for o in orders if o['ofitsiant'] == w) for w in unique_waiters}

# 4. Sorted + lambda: TOP 3 ofitsiant
top_3_waiters = sorted(waiter_performance.items(), key=lambda x: x[1], reverse=True)[:3]

# 5. Generator expression: Umumiy summa hisoblash
total_revenue = sum(order['narx'] for order in orders)

# 6. Max: Eng qimmat buyurtma
most_expensive_order = max(orders, key=lambda x: x['narx'])

# 7. Kunlik statistika
daily_stats = defaultdict(int)
for o in orders:
    daily_stats[o['sana']] += o['narx']

# --- Natijani jadval ko'rinishida chiqarish ---
print(f"{'Ofitsiant':<15} | {'Jami summa':<10}")
print("-" * 30)
for waiter, total in waiter_performance.items():
    print(f"{waiter:<15} | {total:<10}")

print(f"\nTOP 3 Ofitsiantlar: {top_3_waiters}")
print(f"Jami tushum: {total_revenue} so'm")
print(f"Eng qimmat buyurtma: {most_expensive_order['taom']} ({most_expensive_order['narx']} so'm)")
