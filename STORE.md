# Samsung TV store submission checklist

Status of the store edition. Nothing here has been submitted or reviewed by Samsung yet.

## Done in the code

- [x] No keys, tokens or user-edited files: works right after install
- [x] Settings screen driven by the remote, saved on the TV
- [x] English and Hebrew interface
- [x] In-app "How to use" screen
- [x] Back on the main screen asks before exiting; Back closes the settings and help screens
- [x] Releases the screen lock and stops animating when the app goes to the background
- [x] Only public-level privileges: `internet`, `power`, `tv.inputdevice`
- [x] Own package ID, so it does not collide with the personal build

## To verify on a real TV before submitting

- [ ] Photos load from Wikimedia Commons and paintings from the Cleveland Museum of Art (tested so far only with simulated responses)
- [ ] Launch time under 10 seconds
- [ ] Settings, help and exit dialog work with the Smart Remote
- [ ] Returning to the app after pressing Home works (multitasking)
- [ ] Live art runs smoothly; if not, lower `ART_FPS`
- [ ] Check Samsung's current Return/Exit key policy against the exit dialog

## To do in Seller Office

- [ ] Register for the TV Seller Office with a Samsung account ([guide](https://developer.samsung.com/tv-seller-office/guides/membership/becoming-seller-office-member.html)). A new account is a Public Seller and can release in the United States only; other countries need a partnership request.
- [ ] Create the app and upload the signed `.wgt`
- [ ] Icons: 1920x1080 and 512x423
- [ ] Screenshots and store description
- [ ] App UI Description document (Samsung's template)
- [ ] Privacy policy URL (the app collects no personal data and has no backend)

## Open questions

- Samsung sells Art Store as a subscription. It is not known whether a free ambient app will pass review.
- Image licences: Wikimedia Commons photos carry per-image licences (mostly CC BY-SA). The app credits the author on screen but does not show the licence name. Worth settling before a public release.
- Narrow `<access origin="*">` in `config.xml` to the hosts actually used, after testing on a TV.

## Later

- Phone casting (photo, video, text) through one shared bot or a QR upload page, with a small backend.
- More languages.
