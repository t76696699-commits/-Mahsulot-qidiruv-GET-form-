Bu topshiriq Python'dagi lazy evaluation (kechiktirilgan hisoblash) va generator pipeline (konveyer) tushunchalarini mukammal o'zida aks ettiradi. List yaratmasdan, faqat kerakli ma'lumotni oqim sifatida qayta ishlash uchun quyidagi koddan foydalanishimiz mumkin:

Python yechimi
Python
import datetime

# 1. Faylni qator-qator o'qish generatori
def read_file(file_path):
    with open(file_path, 'r', encoding='utf-8') as file:
        for line in file:
            yield line.strip()

# 2. Faqat "ERROR" satrlarini filterlash generatori
def filter_errors(lines):
    for line in lines:
        if "ERROR" in line.upper():
            yield line

# 3. Har satrga vaqt belgisi qo'shish generatori
def add_timestamp(lines):
    for line in lines:
        timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        yield f"[{timestamp}] {line}"

# --- Pipeline ishga tushirilishi ---
file_path = 'log.txt' # Fayl yo'li

# Pipeline bog'lanishi
lines = read_file(file_path)
errors = filter_errors(lines)
pipeline = add_timestamp(errors)

# Statistika uchun o'zgaruvchilar
total_lines = 0
error_count = 0

print("--- Natijalar oqimi ---")
# 4. Asosiy oqim (list yaratilmaydi, faqat iteratsiya qilinadi)
for processed_line in pipeline:
    print(processed_line)
    error_count += 1

# Jami qatorlar sonini aniqlash uchun faylni qayta sanaymiz (yoki o'qish jarayonida ham hisoblash mumkin edi)
with open(file_path, 'r', encoding='utf-8') as f:
    total_lines = sum(1 for _ in f)

print(f"\n--- Statistika ---")
print(f"Jami satrlar: {total_lines}")
print(f"ERROR satrlari: {error_count}")
