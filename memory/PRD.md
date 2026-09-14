# Artirdim.com — Açık Artırma Platformu

## Problem Statement
Kullanıcının Laravel 12 + Inertia.js + Vue 3 ile geliştirdiği açık artırma (müzayede) projesinin geliştirilmesi. Repo: https://github.com/xprlyzed/projecct

## Mimari
- Laravel 12 (PHP 8.2) + Inertia.js + Vue 3 + Tailwind CSS + Vite
- Repo konumu: /app/projecct
- Önizleme ortamında: SQLite veritabanı (/app/projecct/database/database.sqlite), file cache/session, sync queue
- Sunucu: supervisor programı `laravel` → `php artisan serve --host=0.0.0.0 --port=3000` (orijinal React frontend supervisor'da durduruldu)
- Paketler: spatie/medialibrary, spatie/permission, laravel/scout (collection driver), laravel/reverb (kapalı), laravel/horizon (kapalı), livekit (canlı yayın), ziggy

## Yapılanlar
- 14.09.2026: Repo klonlandı, PHP 8.2 + Composer kuruldu, composer install + npm install + vite build tamam
- 14.09.2026: .env oluşturuldu (SQLite, tr locale, APP_URL=preview URL), migrate + db:seed + AuctionSeeder çalıştırıldı, storage:link yapıldı
- 14.09.2026: Laravel supervisor'a eklendi (port 3000), site canlı ve admin girişi doğrulandı (admin panel: 103 kullanıcı, 39 aktif müzayede, 321 teklif)

## Kullanıcı Personaları
- Admin (yönetim paneli, kullanıcı/kategori/müzayede/sipariş/destek yönetimi)
- Satıcı (ilan açma, canlı yayın, sipariş kargolama)
- Alıcı (teklif verme, watchlist, mesajlaşma, sipariş onayı)

## Bilinen Özellikler (seed'den + rotalardan)
Açık artırma CRUD, teklif sistemi, kategoriler, watchlist, takip sistemi, hikayeler (24s), mesajlaşma, satıcı değerlendirme, sipariş/emanet (escrow) akışı, bakiye sistemi (demo ödeme), admin panel, canlı yayın (LiveKit), destek talepleri, Google login (Socialite)

## Sıradaki Görevler
- Kullanıcı geliştirme görevlerini yazacak (bekleniyor)
- PROGRESS.md'de açık maddeler: C) Login/Register sağ panel yenileme, D) Mesajlar görüldü bilgisi, E) Admin EFT/Havale yönetim sayfaları, F) Production readiness, G) KURULUM.md düzeltmesi

## Notlar
- Ödeme/bakiye yükleme DEMO durumda (gerçek sağlayıcı yok)
- Scheduler görevleri (auctions:close, orders:auto-release, stories:prune) cron gerektirir; önizlemede fırsatçı tetikleniyor
- Google OAuth ve LiveKit canlı yayın önizlemede yapılandırılmadı (anahtar yok)
