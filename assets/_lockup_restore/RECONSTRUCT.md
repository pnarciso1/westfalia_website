# Lockup restore sidecars

Shards: `partNN_MM.b64` (5000 ASCII chars each, except last of each part).
Or full parts: `part00.b64` .. `part07.b64`.

Assemble:

```bash
cd /workspace  # or repo root
git fetch origin fix/restore-approved-lockup && git checkout fix/restore-approved-lockup
mkdir -p /tmp/lockup-restore
if [[ -f assets/_lockup_restore/part00.b64 ]]; then
  for i in 00 01 02 03 04 05 06 07; do cp "assets/_lockup_restore/part${i}.b64" /tmp/lockup-restore/; done
else
  for i in 00 01 02 03 04 05 06 07; do cat assets/_lockup_restore/part${i}_*.b64 > /tmp/lockup-restore/part${i}.b64; done
fi
cat /tmp/lockup-restore/part{00..07}.b64 | tr -d '\n' | base64 -d > /tmp/emily-westfalia-lockup.png
sha256sum /tmp/emily-westfalia-lockup.png
# MUST equal c59b16bd357c8c09053b4193de09c08218358ac8a37494f5500bd724dccd9b5a
# size 530793
cp /tmp/emily-westfalia-lockup.png assets/westfalia-lockup.png
git rm -r assets/_lockup_restore
git add assets/westfalia-lockup.png
git commit -m "Restore approved Westfalia W Loop lockup"
git push
```
