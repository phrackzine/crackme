### The challenge:


```
ZGlnICtzaG9ydCBlZ2c/Pz8/LnBocmFjay5vcmcgVFhU
```


---

### THE SOLUTION:

Step 1:
- Recognize that it is a base64 encoded string
- Decode it

```shell
base64 -d <<<'ZGlnICtzaG9ydCBlZ2c/Pz8/LnBocmFjay5vcmcgVFhU'
```
```
dig +short egg????.phrack.org TXT
```

Step 2:
- Recognize that it's a DNS request.
- Recognize that `?` is not a _valid_ character in a domain name.
- Recognize that `?` is a regex placeholder.
- Valid characters are `a-z`, `A-Z`, `0-9` and `-` and `.` (dot)!
- Brute force (roughly) `(26 + 26 + 10 + 2)^4 == 16,777,216` possibilities.
- Or be smart and try `0000` to `9999` first

```shell
for n in $(seq -w 0000 9999); do
  s="$(dig +short "egg${n}.phrack.org" TXT)"
  [ -n "$s" ] && break
done
echo "$n: $s"
```
🖖 Apology to our fine friends at CloudFlare for the extra traffic. 😘


Results in the FINAL SOLUTION:


# 💥You found a Purple-Green-White Egg!💥


CONGRATULATIONS TO 🫡 RT_SABER FOR BEING SO QUICK.

Thank you for all the other PULL REQUESTS, including d0h1e for a complete RUST implementation (you rock!).

👉 Stay tuned for the THREE CRACKME's in PHRACK #72 THIS SUMMER 2025 👈




