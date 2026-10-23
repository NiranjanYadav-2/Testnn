# 08 — Privacy, Security & Future Backup

## 1. V1 privacy posture
- No login required.
- No backend required for core use.
- User-created goals, journal entries, and ritual sessions stay on-device.
- Request only permissions necessary for a feature.
- No hidden collection or upload of journal content.
- Avoid analytics by default unless explicitly approved and disclosed.

## 2. Journal privacy
- Do not log journal bodies, intentions, affirmations, or goal descriptions.
- Avoid including sensitive content in notifications.
- Use Android's app-private storage and Room database.
- Consider optional app lock only if it can be implemented reliably without overcomplicating V1.
- Do not claim database encryption unless an actual, reviewed encryption solution is implemented.

## 3. Permissions
Request permissions only when needed and explain why. Notifications are optional. Do not request contacts, location, microphone, or broad storage access unless a clearly approved feature needs them.

## 4. Data deletion
- Provide an understandable delete-data flow.
- Require explicit confirmation.
- Make clear whether deletion removes goals, journal entries, sessions, tasks, and local preferences.
- Avoid irreversible deletion from a single accidental tap.
- If export exists, explain that exported files are outside the app's control and may contain private content.

## 5. Future V2 backup/restore
Design repository interfaces so cloud backup can be introduced later without coupling UI directly to a provider.
Potential approaches require evaluation at V2:
- authenticated private cloud storage;
- user-controlled encrypted export/import;
- a carefully designed backend.

Google Sheets may be useful for prototypes but is not recommended as the primary storage for private journal content at scale. Never embed a service-account private key or privileged write credential in the APK. Never make a private sheet public as a workaround.

V2 requirements:
- explicit user consent;
- authentication/authorization;
- transport security;
- encryption and key management appropriate to the threat model;
- conflict resolution and duplicate handling;
- deletion semantics;
- backup/restore validation and recovery;
- transparent privacy policy;
- no silent upload of private content.

## 6. Secrets and signing
- Never commit `.jks`, keystores, passwords, API secrets, service-account private keys, or signing credentials.
- Use local ignored files or CI secret storage.
- Do not include production signing material in a shared ZIP.
- Include `.gitignore` rules for secrets and build outputs.

## 7. Release privacy checklist
- Review Android permissions.
- Review exported components and intent filters.
- Confirm no accidental network requests.
- Inspect logs for private data.
- Include privacy policy and data disclosure required by current Play policies before publication.
- Verify all third-party SDKs and their data practices if any are added.
