class BankAccount:

    def __init__(self, ism, qoldiq=0):
        self.ism = ism
        self.qoldiq = qoldiq
        self.tarix = []  # Amallar tarixi
        self.tarix.append(f"Hisob ochildi. Boshlang'ich qoldiq: {qoldiq}")

    def pul_qoyish(self, summa):
        if summa <= 0:
            print("Xato: Summa 0 dan ko'p bo'lishi kerak!")
            return False

        self.qoldiq += summa
        self.tarix.append(f"Pul qo'shildi: +{summa}")
        print(f"{self.ism}: {summa} so'm qo'shildi.")
        return True

    def pul_chiqarish(self, summa):
        if summa <= 0:
            print("Xato: Noto'g'ri summa!")
            return False

        if self.qoldiq - summa < 0:
            raise ValueError(f"Xato: Mablag' yetarli emas! Qoldiq: {self.qoldiq}")

        self.qoldiq -= summa
        self.tarix.append(f"Pul yechildi: -{summa}")
        print(f"{self.ism}: {summa} so'm yechildi.")
        return True

    def qoldiq_korish(self):
        print(f"[{self.ism}] Balans: {self.qoldiq} so'm")
        return self.qoldiq

    def transfer(self, boshqa_hisob, summa):
        print(f"\n{self.ism} dan {boshqa_hisob.ism} ga {summa} so'm o'tkazilmoqda...")
        # Pulni yechib olamiz (agar yetmasa ValueError beradi)
        self.pul_chiqarish(summa)
        # Boshqa hisobga qo'shamiz
        boshqa_hisob.pul_qoyish(summa)

        self.tarix.append(f"Transfer: {boshqa_hisob.ism} ga {summa} o'tkazildi")
        boshqa_hisob.tarix.append(f"Transfer: {self.ism} dan {summa} keldi")
        print("Transfer muvaffaqiyatli bajarildi!")

    def __str__(__self_obj):
        return f"Mijoz: {__self_obj.ism}, Balans: {__self_obj.qoldiq} so'm"


# INHERITANCE: PremiumAccount (Kredit limiti bor)
class PremiumAccount(BankAccount):

    def __init__(self, ism, qoldiq=0, kredit_limiti=500000):
        super().__init__(ism, qoldiq)
        self.kredit_limiti = kredit_limiti
        self.tarix.append(f"Premium status berildi. Kredit: {kredit_limiti}")

    # Metodni qayta yozamiz (Overriding) - kredit hisobiga ham pul yechish mumkin
    def pul_chiqarish(self, summa):
        if summa <= 0:
            print("Xato: Noto'g'ri summa!")
            return False

        # Umumiy pul (qoldiq + kredit limiti) yetishini tekshiramiz
        if (self.qoldiq + self.kredit_limiti) - summa < 0:
            raise ValueError("Xato: Kredit limitidan ham oshib ketdi!")

        self.qoldiq -= summa
        self.tarix.append(f"Premium yechish: -{summa}")
        print(f"{self.ism} (Premium): {summa} so'm yechildi.")
        return True


# --- SINOV (KAMIDA 3 TA OBYEKT VA TRANSFER) ---
if __name__ == "__main__":
    print("--- BANK TIZIMI TESTI ---")

    # 1. Obyektlar yaratish
    hisob1 = BankAccount("Ali", 100000)
    hisob2 = BankAccount("Vali", 50000)
    hisob3 = PremiumAccount("Sardor", 20000, 300000)

    print("\n1. Obyektlar holati (__str__):")
    print(hisob1)
    print(hisob2)
    print(hisob3)

    print("\n2. Transfer amallari:")
    # Ali Valiga 30000 o'tkazadi
    hisob1.transfer(hisob2, 30000)

    # Sardor (Premium) Aliga 40000 o'tkazadi
    hisob3.transfer(hisob1, 40000)

    print("\n3. Oxirgi balanslar:")
    hisob1.qoldiq_korish()
    hisob2.qoldiq_korish()
    hisob3.qoldiq_korish()

    print("\n4. Ali hisob tarixi:")
    for t in hisob1.tarix:
        print("-", t)
