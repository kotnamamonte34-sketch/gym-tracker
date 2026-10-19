// GYM TRACKER: Web Push service worker. No caching of private user data.
self.addEventListener('install', () => self.skipWaiting());
self.addEventListener('activate', event => event.waitUntil(self.clients.claim()));
self.addEventListener('push', event => {
  let payload = {};
  try { payload = event.data ? event.data.json() : {}; } catch { payload = { body: event.data ? event.data.text() : '' }; }
  const title = String(payload.title || 'GYM TRACKER').slice(0, 100);
  const body = String(payload.body || 'Новое уведомление').slice(0, 250);
  event.waitUntil(self.registration.showNotification(title, {
    body, icon: './icon.svg', badge: './icon.svg',
    tag: String(payload.tag || 'gym-tracker'),
    data: { url: './' }
  }));
});
self.addEventListener('notificationclick', event => {
  event.notification.close();
  event.waitUntil(self.clients.matchAll({ type: 'window', includeUncontrolled: true }).then(async windows => {
    const match = windows.find(w => w.url.includes('/gym-tracker/'));
    if (match) return match.focus();
    return self.clients.openWindow('./');
  }));
});
