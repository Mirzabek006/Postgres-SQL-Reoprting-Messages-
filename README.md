# Postgres-SQL-Reoprting-Messages-
Reporting Messages 
Postgresql da PL/pgSQL tilida xabarlar va xatolar bilan ishlash uchun RAISE operatori ishlatiladi.
Bu operator yordamida xatolar, ogohlantirishlar va ma'lumot xabarlarini ko'rsatish, shuningdek,
ularga qo'shimcha ma'lumotlar qo'shish imkoniyati mavjud.U turli darajadagi xabarlar (DEBUG,LOG ,INFO,NOTICE, WARNING , EXCEPTION ) chiqarish imkonini beradi, bu xabarlar mijozga yoki server
loglariga yoziladi.Har bir daraja xabarlarining muhimlik darajasini va ular qayerda ko'rsatilishni
belgilaydi.
RAISE [level][format][,expression [,..]]][USING option =expression[,..]];
RAISE[level][format][,experssion[,..][USING option=expression[,..]];
level:XABAR darajasini belgilaydi. Agar daraja ko'rsatilmasa , standard sifatida
EXCEPTION qabul qilinadi.Mumkin bo'lgan darajalar:
DEBUG: Debuging uchun batafsil ma'lumotlar (odatda mijozga ko'rsatilamydi).
LOG:Server loglariga yoziladigan ma'lumot xabarlari.
INFO:Mijozga foydali ma'lumot xabrlari.
NOTICE:E'tibor talab qiluvchi, ammo xato bo'lmagan holatlar.
WARNING :Potensial muammolar haqida ogohlantirish.
EXCEPTION:Xato yuzaga keltiriladi va tranzaksiyani to'xtatadi(standard daraja).
fromat :Xabar matni, unda % belgisi yordamida o'zgaruvchilar joylashtiriladi.
Masalan,'Xato:%', variable.
expression:% o'rniga joylashtirilgan qiymatlar.
USING:Qo'shimcha ma'lumotlatar qo'shish uchun ishlatiladi,masalan:
MESSAGE:Asosiy xabar matnini o'zgartirish.
DETAIL:Xato haqida qo'shimcha ma'lumot.
HINT:Muammoni hal qilish bo'yicha maslahat.
ERRCODE:SQLSTATE xato kodi (masalan ,23505 -unique constraint violation).
TABLE ,COLUMN,CONSTRAIN:Tegishli ob'ekt nomlari.
Trap Erros
PostgreSQL'da PL/pgSQL tilida xatolar (error) bilan ishlash uchun TRY-CATCH 
mexanizm o'xshash tuzilma sifatida Begin..... EXEPTION ...END bloki sihlatiladi.
Bu blok yordamida xatolarni ushlash (trap errors), ularni boshqarish va ularga mos
javob qaytarish mumkin.
BEGIN
-NORMAL kod bloki
EXCEPTION
WHEN condition [OR condition..] THEN
--Xato ushlaganda bajariladigan kod
END;
Exception haqida ma'lumot olish:EXCEPTION blokida xato haqida ma'lumotlarni olish 
uchun maxsus o'zgaruvchilar ishlatiladi:
SQLERRM:Xato xabarning matni.
SQLSTATE:Xato kodi(5 belgili alfanumerik kod).
PG_EXCEPTION_DETAIL:;Xato haida qo'shimcha ma'lumot (agar mavjud bo'lsa).
PG_EXCEPTION_HINT:Xatoni hal qilish bo'yicha maslahat(agar RAIESE orqali qo'shilgan bo'lsa).
PG_EXCEPTION_CONTEXT:Xato yuzaga kelgan joy(funksiy, satr raqami va boshqalar).

Assert Statement
PostgreSQL da PL/pgSQL tilida ASSERT statement kodni sinovdan o'tkazissh va dasturiy
shartlarni  tekshirish uchun ishlatiladi. U shartning to'g'ri  yoki nato'g'ri ekanligini
tekshiradi va agar shart noto'g'ri bo'lsa xato chiqaradi. Bu bayonot assosan debugging
va kod sifatini ta'minlash uchun qulaydir.
ASSERT statement PL/pgSQL da PostgreSQL 9.5 versiyasidan boshlab qo'llab-quvvatlanadi.U shartni
tekshiradi va agar sharti false bo'lsa,EXCEPTION xato chiqaradi. Bu xato odatda  assertion_failure SQLSTATE kodi (kod:P0004) bilan belgilanadi.
ASSERT odatda dasturiy logikani sinash,kutilgan shartlarini tasdiqlash yoki debugging maqsadlarini ishlatiladi.
ASSERT condition[, message];
show plpsql.plpsql_check_asserts;
SET plpsql_check_asserts=on;--ASSERT larni yoqish
SET plpsqql_check_asserts=off;--ASSERT'larni o'chirsh

Qachon ASSERT ishlatish kerak?
Kodni sinovdan o'tkazishda.
Kutilgan shartlarini tasdiqlashda (masalan,parametrlarning to'g'riligi).
Ishlab chiqarish muhitida o'chirilishi mumkin bo'lgan tekshiruvlar uchun.
Qachon RAISE ishlatish kerak?
Foydalanuvchiga xabar yoki ogohlantirish yuborish uchun.
Tranzaksiyani to'xtatish yoki muayyan xato shartlarini boshqarish uchun.
