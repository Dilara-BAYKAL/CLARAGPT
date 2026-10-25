# Samimi ve bilgili sohbet botu

## Ne yapılacak
- Giriş / kayıt sayfası (e-posta ve şifre).
- Giriş yapınca tek, sürekli bir sohbet ekranı: mesajlar anlık akarak gelir, "yazıyor..." göstergesi, biçimli (markdown) cevaplar.
- Tüm mesajlar hesaba kaydedilir; sayfa yenilense veya başka cihazdan girilse de sohbet geri gelir.
- "Sohbeti temizle" butonu ve çıkış yap butonu.
- Botun kişiliği: samimi, sıcak, bilgili; kullanıcının dilinde (varsayılan Türkçe) cevap verir.
- Sıcak, kendine özgü bir görsel tasarım (krem/terrakota tonları, karakterli yazı tipi), özel bir bot simgesi.

## Teknik detaylar
- Lovable Cloud etkinleştirilir; e-posta girişi açılır.
- Tablo: `chat_messages` (id uuid, user_id, role, parts jsonb, created_at) + GRANT + RLS (kullanıcı yalnızca kendi satırları).
- Rotalar: `/auth` (açık), `_authenticated/route.tsx` korumalı düzen, `/` girişsizse auth'a yönlendirir, sohbet `_authenticated/chat.tsx`.
- Sunucu: `/api/chat` rotası, bearer doğrulama, AI SDK + Lovable AI Gateway (`openai/gpt-6-astra`, Responses API, streaming), `onFinish` ile asistan mesajı kaydı; hata durumları (402/429) kullanıcıya gösterilir.
- UI: AI Elements (Conversation, Message, MessageResponse, PromptInput), `useChat` ile.
