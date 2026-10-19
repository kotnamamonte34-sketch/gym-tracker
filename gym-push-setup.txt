// GYM TRACKER PWA setup: registers service worker and adds manifest metadata.
// Actual scheduled push delivery requires VAPID keys, a push subscription endpoint,
// and a server-side scheduler; this file intentionally does not claim to provide them.
(() => {
  if (!('serviceWorker' in navigator) || !window.isSecureContext) return;
  if (!document.querySelector('link[rel="manifest"]')) {
    const link = document.createElement('link');
    link.rel = 'manifest'; link.href = './manifest.webmanifest';
    document.head.appendChild(link);
  }
  if (!document.querySelector('meta[name="theme-color"]')) {
    const meta = document.createElement('meta');
    meta.name = 'theme-color'; meta.content = '#111827';
    document.head.appendChild(meta);
  }
  navigator.serviceWorker.register('./sw.js', { scope: './' }).catch(err => {
    console.warn('GYM TRACKER: service worker registration failed', err);
  });
})();
