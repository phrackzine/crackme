### The challenge:


```
ZGlnICtzaG9ydCBlZ2c/Pz8/LnBocmFjay5vcmcgVFhU
```


---

Base64 decode the above string to get:

```
dig +short egg????.phrack.org TXT⏎
```

Find the TXT record by guessing that the question marks are numbers and brute forcing:

```
# in fish
for i in (seq -w 0000 9999); echo $i; dig +short egg$i.phrack.org TXT @1.1.1.1; end

# result:
1337
"You found a Purple-Green-White Egg! CONGRATULATION! Send a PR at git@github.com/phrackzine/crackme"
```

